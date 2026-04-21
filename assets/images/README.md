# Tutorial 6 - Concurrency

## Commit 1 Reflection notes

In the first part of this tutorial, I learned how a Rust program can listen to TCP connections using TcpListener on 127.0.0.1:7878.

At first, the server only accepted incoming connections and printed a message in the terminal, so the browser still showed an empty response.
After that, I was instructed to modify the code by adding the handle_connection function to inspect the HTTP request sent by the browser.

Using BufReader and .lines(), I could see that the browser sends a request line and several headers such as host, user-agent, and accept.
This helped me understand that even very simple web server must first read and understand the structure of an HTTP request before responding properly.

I also noticed that the browser can send repeated requests, which explains why multiple logs appeared in the terminal.

## Commit 2 Reflection notes

I learned how the server can return an actual HTML page instead of only printing the request in the terminal.
The server reads the file hello.html, calculates its content length, and then formats a valid HTTP response before sending it to the browser.

This made me understand that the browser needs both the response body and the HTTP metadata, especially the status line and Content-Length, to render the page correctly.

I also learned that the Rust standard library is enough to build a very simple web server without using a web framework.

Compared to the previous milestone, this step makes the application feel like a real web server because the browser can now display content normally.
This milestone also showed me the difference between receiving a request and properly responding to that request.

Commit 2 Screenshot:
![Commit 2 screen capture](/assets/images/commit2.png)

## Commit 3 Reflection notes

In this part, I changed the server so it can give different responses depending on the request path.  
Previously, the server always returned the same HTML file, but now it checks the request line first and decides whether to send hello.html or 404.html.  
This split between responses is necessary because a proper web server should not treat every request as valid.  

I learned that the request line is enough for this simple tutorial to determine whether the browser is asking for the main page or for a path that does not exist.  
The refactoring is needed because both responses still share the same general process: reading a file, calculating its length, formatting the HTTP response, and writing it back to the stream.  
Instead of writing those steps twice, the code becomes much cleaner when only the status line and filename are selected first, and the rest of the logic is reused.  

This makes the program easier to read, easier to maintain, and easier to expand later if more routes are added.

Commit 3 Screenshot:
![Commit 3 screen capture](/assets/images/commit3.png)

## Commit 4 Reflection notes

In this milestone, I simulated a slow response by making the server sleep for 10 seconds when the browser requests /sleep.  
This experiment shows the weakness of a single-threaded server, because while one request is being processed, the server cannot handle other requests at the same time.  
When I opened /sleep in one browser window and then opened / in another window, the second request also had to wait until the first one finished.  

This happens because the server processes incoming connections sequentially, so one slow task blocks all other clients behind it.  
I learned that even though the server is functionally correct, its performance and responsiveness are poor when one request takes too long.  
This milestone clearly demonstrates why concurrency is important in server development, especially when many users may access the server simultaneously.  
It also prepares the motivation for the next milestone, where a multithreaded server is introduced to solve this blocking problem.

## Commit 5 Reflection notes

I changed the server from a single-threaded design into a multithreaded server by using a ThreadPool.  

Instead of handling each incoming connection directly in the main thread, the main thread now passes each connection as a job to the thread pool.  
I learned that the ThreadPool works by maintaining a fixed number of worker threads that wait for jobs sent through a channel.  
When a request arrives, one available worker takes the job and executes handle_connection, which allows multiple requests to be processed concurrently.  
This approach is better than creating unlimited new threads because it keeps resource usage more controlled and reduces the risk of exhausting system resources.  

After testing it with /sleep and /, I could see that a slow request no longer blocks every other request in the same way as before.  
This milestone helped me understand how concurrency improves server responsiveness and why thread pools are a practical design for building more scalable servers.