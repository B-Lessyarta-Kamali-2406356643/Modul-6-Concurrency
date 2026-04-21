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