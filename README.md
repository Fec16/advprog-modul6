## Module 6
### Commit 1 Refletion Notes

The `handle_connection` function is responsible for processing incoming TCP streams until it encounters an empty line, indicating the end of the HTTP request headers. Then it prints out the collected lines as the HTTP request.  

- It takes a mutable reference to a `TcpStream` as its argument.  
- It creates a `BufReader` wrapping the given TCP stream. This helps with buffered reading from the stream efficiently.  
- Finally, it prints out the HTTP request for inspection.  


### Commit 2 Reflection Notes

The modified `handle_connection` function reads the content of `hello.html` file, constructs an HTTP response with a `200 OK` status line, and sends it back to the client over the TCP stream. 

- Define a status_line indicating that the HTTP response is ```200 OK```. This means the request was successful.  
- Read the contents of a file named `hello.html` into a string using `fs::read_to_string()`. This assumes that there is a file named `hello.html` in the same directory as the executable.  
- Calculate the length of the contents string.  
- Format the HTTP response, including the status line, content length, and the contents of the `hello.html` file. 
- Writes the response back to the TCP stream, sending it to the client using `write_all()`.  

![Commit 2 screen capture](/assets/images/commit2.png)


### Commit 3 Reflection Notes

The modified `handle_connection` function acts as a simple HTTP server. It reads the request line from the client's HTTP request and checks if it is a `GET / HTTP/1.1` request, indicating a request for the root path (`/`):

- If the request is for the root path `GET / HTTP/1.1`, it responds with a status line `HTTP/1.1 200 OK` then sends the content of a file named `hello.html` to the client.
- If the request is not for the root path, it responds with a status line `HTTP/1.1 404 NOT FOUND` then sends the content of a file named `404.html` to the client.

![Commit 3 screen capture](/assets/images/commit3.png)