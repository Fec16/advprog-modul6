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


### Commit 4 Reflection Notes
The modified `handle_connection` function, extends the functionality of the server to handle a special case where if the client requests `GET /sleep HTTP/1.1`, the server sleeps for 5 seconds before responding with the content of `hello.html`.

- So if the received path is `/sleep`, then the program will sleep for 5 seconds. Afterward, the server proceeds to generate a response with the status line `HTTP/1.1 200 OK` and serves the file `hello.html` as its displayed content.
- This delay demonstrates how server response time can affect user experience, especially in scenarios where multiple users may simultaneously access the website. Introducing response delays is certainly not desirable as it forces people to wait before accessing the content.


### Commit 5 Reflection Notes
- ThreadPool is a structure responsible for managing a collection of threads that will execute tasks concurrently. In the `main.rs` file, the `main` method is modified to:

```
fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);
    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }
}
```

- To use ThreadPool with a size of 4 to handle incoming TCP connections, each connection will be handled by a thread available in the ThreadPool. To make the ThreadPool work, we add it in `src/lib.rs`.

- By adding `src/lib.rs`, we create a ThreadPool structure capable of managing a collection of worker threads. Each worker thread within the ThreadPool will receive tasks sent through a channel managed by a sender. Whenever a new task is sent, a worker thread will take that task from the channel and execute it. Thus, we can use this ThreadPool to handle tasks concurrently, such as in the case of handling TCP connections as seen in the `main()` method in the file `src/main.rs`.


### Bonus Reflection Notes
- It is recommended to replace the `new` method with `build`, which returns `Result`. Then, when the value is returned to the caller, it can be unwrapped to get the value of the execution result.
- In this implementation, the `build` method replaces the `new` method. This method takes the size of the ThreadPool as a parameter and returns a new instance of the ThreadPool. Inside the `build` method, we use the `map` function to create a vector of worker threads, similar to what is done in the `new` method. 
- This provides a cleaner separation between the construction of the ThreadPool and its usage, making the code easier to understand. Additionally, it follows the *`Single Responsibility Principle`*, with each function having a clear and specific purpose.