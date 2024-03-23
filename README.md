## Module 6
### Commit 1 Refletion Notes

The `handle_connection` function is responsible for processing incoming TCP streams until it encounters an empty line, indicating the end of the HTTP request headers. Then it prints out the collected lines as the HTTP request.  

- It takes a mutable reference to a `TcpStream` as its argument.  

- It creates a `BufReader` wrapping the given TCP stream. This helps with buffered reading from the stream efficiently.  

- Finally, it prints out the HTTP request for inspection.  
