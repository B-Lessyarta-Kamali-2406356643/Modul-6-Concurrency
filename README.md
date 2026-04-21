# Tutorial 6 - Concurrency

## Commit 1 Reflection notes

In the first part of this tutorial, I learned how a Rust program can listen to TCP connections using TcpListener on 127.0.0.1:7878.

At first, the server only accepted incoming connections and printed a message in the terminal, so the browser still showed an empty response.

After that, I was instructed to modify the code by adding the handle_connection function to inspect the HTTP request sent by the browser.

Using BufReader and .lines(), I could see that the browser sends a request line and several headers such as host, user-agent, and accept.

This helped me understand that even very simple web server must first read and understand the structure of an HTTP request before responding properly.

I also noticed that the browser can send repeated requests, which explains why multiple logs appeared in the terminal.