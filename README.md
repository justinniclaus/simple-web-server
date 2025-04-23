# Multithreaded Web Server in Rust

A lightweight, multithreaded HTTP server written in Rust that demonstrates core concepts of concurrent programming and network handling.

## Overview

This project implements a simple web server with a custom thread pool for handling multiple concurrent connections. The server responds to HTTP requests with either a greeting page or a 404 error page, and includes a simulated slow response endpoint for demonstrating concurrent processing.

## Features

- Listens for HTTP requests on `127.0.0.1:7878`
- Custom thread pool implementation with configurable number of worker threads
- Graceful shutdown with proper resource cleanup
- Different response types:
  - Root path (`/`): Returns a greeting HTML page
  - Sleep path (`/sleep`): Simulates a slow response (5 seconds delay)
  - Any other path: Returns a 404 error page

## Project Structure

- `main.rs` - Server entrypoint and request handling logic
- `lib.rs` - Thread pool implementation
- `hello.html` - Success response HTML template
- `404.html` - Error response HTML template
- `Cargo.toml` - Project configuration

## Implementation Details

### Server (main.rs)

The server binds to a local address and port, creates a thread pool, and listens for incoming TCP connections. When a connection is received, it's passed to the thread pool for handling.

The `handle_connection` function:
1. Reads and parses the HTTP request
2. Determines the appropriate response based on the request path
3. Sends back the response with the appropriate status code and content

### Thread Pool (lib.rs)

The thread pool manages a collection of worker threads that process incoming connections concurrently. Key components include:

- `ThreadPool` - Manages workers and distributes jobs
- `Worker` - Executes jobs in its own thread
- Job channel - Facilitates communication between the pool and workers

The implementation demonstrates several Rust concurrency primitives:
- Channels (`mpsc`)
- Thread-safe shared ownership (`Arc<Mutex<T>>`)
- Thread spawning and joining
- Safe resource cleanup via the `Drop` trait

## Getting Started

### Prerequisites

- Rust and Cargo (install from [rustup.rs](https://rustup.rs))

### Running the Server

1. Clone this repository
2. Build and run the project:

```bash
cargo run
```
![image](https://github.com/user-attachments/assets/18a54848-017f-4f18-84b5-4f10653e9089)

3. Open a web browser and navigate to:
   - [http://127.0.0.1:7878](http://127.0.0.1:7878) - For the greeting page
     ![image](https://github.com/user-attachments/assets/2ee5a407-bd04-4a16-a498-0093a35b1974)
   - [http://127.0.0.1:7878/sleep](http://127.0.0.1:7878/sleep) - For the delayed response
   - Any other path for a 404 error
     ![image](https://github.com/user-attachments/assets/77132deb-0781-4d2e-9299-a230567c3e27)

### Testing Concurrency

To demonstrate the multithreaded capability:

1. Open one browser tab and navigate to [http://127.0.0.1:7878/sleep](http://127.0.0.1:7878/sleep)
2. Immediately open another tab and navigate to [http://127.0.0.1:7878](http://127.0.0.1:7878)
![image](https://github.com/user-attachments/assets/337555de-7788-49cb-b08e-d2e1e046a658)

The second request will be processed immediately, even while the first request is still "sleeping" - demonstrating the benefits of multithreaded request handling.

## Customization

- Modify the HTML files to change the response content
- Change the number of worker threads in `main.rs` (default is 4)
- Add new paths and response types in the `handle_connection` function
