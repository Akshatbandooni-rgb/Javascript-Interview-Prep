# Concurrency and Parallelism in Node.js

---

## Is Node.js capable of both concurrency and parallelism?

**Concurrency:**  
Yes, Node.js is capable of concurrency, but it does so using an event loop and non-blocking I/O, allowing it to handle many operations concurrently, even though it runs on a single thread. This means Node.js can manage multiple operations in an asynchronous manner (e.g., handling I/O operations without blocking the execution of other tasks).

**Parallelism:**  
Node.js does not natively provide parallelism on a single thread. However, through child processes, worker threads, and tools like PM2 or the Cluster module, Node.js can distribute tasks across multiple CPU cores, enabling parallelism for CPU-intensive tasks.

---

## How do you leverage `libuv` to achieve a level of parallelism?

`libuv` is a C library that underlies Node.js, providing an event loop, thread pool, and async I/O operations. It helps achieve concurrency in Node.js by managing asynchronous operations (like reading files, network requests, etc.) in a non-blocking manner.

For parallelism, `libuv` uses a thread pool for handling certain tasks like file system operations and DNS lookups, which are offloaded to these threads. This allows Node.js to continue processing other events while the parallel tasks are handled in the background.

---

## How does a promise work under the hood?

Promises in JavaScript are part of the asynchronous programming model. Here's how they work under the hood:

1. When a promise is created, it starts in a "pending" state.
2. Once the operation (like an async task) completes, it transitions to either:
   - **"fulfilled"** (resolved successfully), or
   - **"rejected"** (an error occurred).
3. The `.then()` and `.catch()` handlers are added to the **microtask queue** and will run once the synchronous execution stack is empty, ensuring they are processed before other tasks in the event loop.

---

## What's the difference between Cluster and PM2?

### **Cluster Module**

- **Definition:**  
  The Cluster module is a built-in Node.js module that allows you to create child processes (workers) that can run on multiple CPU cores, improving performance for multi-core systems.
- **Key Features:**
  - Each worker in the cluster has its own event loop and can handle requests independently.
  - Workers share the same server port, coordinated by the master process.
  - Requires more manual configuration and management.
- **Use Case:**  
  Basic load balancing for multi-core systems.

### **PM2 (Process Manager 2)**

- **Definition:**  
  PM2 is an advanced process manager that simplifies managing and monitoring Node.js applications.
- **Key Features:**
  - Built-in load balancing, process monitoring, logging, and auto-restart on crashes.
  - Supports running Node.js apps in cluster mode.
  - Provides log aggregation and process scaling.
- **Use Case:**  
  Simplifies deployment and monitoring of production applications.

---

## Pros and Cons of PM2

### **Pros:**

- **Easy Process Management:** Simplifies starting, stopping, and monitoring Node.js applications.
- **Load Balancing:** Automatically balances the load across multiple CPU cores.
- **Crash Recovery:** Restarts apps automatically if they crash.
- **Logging & Monitoring:** Provides built-in logs, metrics, and monitoring dashboards.

### **Cons:**

- **Overhead:** Introduces some overhead since it is an extra layer on top of your application.
- **Dependency:** Relies on an external tool for managing processes.
- **Limited Customization:** May not suit very specialized use cases.

---

## How does Cluster mode compare to Worker Threads?

### **Cluster Mode**

- **Use Case:** Best for scaling applications across multiple cores.
- **How It Works:**  
  Spawns multiple processes (workers) that run on separate CPU cores, with each worker handling a portion of the incoming requests.
- **Key Features:**
  - Workers do not share memory directly.
  - Suitable for stateless applications where workers don’t need to share state frequently.

### **Worker Threads**

- **Use Case:** Ideal for CPU-bound tasks or when you need parallel execution in a single Node.js process.
- **How It Works:**  
  Runs JavaScript code in parallel in multiple threads, making them perfect for CPU-intensive tasks (e.g., heavy computation).
- **Key Features:**
  - Allows communication between threads via messages.
  - Does not have direct access to the main thread's event loop.

---

## What problem do Worker Threads solve? What are their pros and cons?

### **Problem Solved:**

Worker threads allow Node.js to perform CPU-bound operations in parallel without blocking the main event loop. This is crucial for heavy computations, like processing large datasets or image processing.

### **Pros:**

- **Parallelism:** Enables multi-threading in Node.js, allowing computational tasks to be distributed and processed in parallel.
- **Non-blocking:** Prevents blocking the main thread, keeping I/O operations responsive.

### **Cons:**

- **Overhead:** Introduces overhead due to the need for communication between the main thread and worker threads.
- **Complexity:** Adds complexity to the code, especially when managing communication and shared state between threads.

---

## How do Worker Threads compare to Child Processes?

### **Worker Threads**

- **Runs in the same process** but in separate threads.
- Communication is done using a **message-passing model**.
- Shares memory via `SharedArrayBuffer`.

### **Child Processes**

- **Runs in separate processes** with their own memory space.
- Communication is done using **IPC (Inter-Process Communication)**, which can be more expensive than worker thread messaging.
- Better suited for tasks requiring complete isolation.

---

## What does "sharing the port" mean in Cluster?

In Cluster mode, all workers share the same server port. The master process opens the port and distributes incoming requests to the workers. This allows multiple processes to handle requests on the same port, improving scalability.

---

## How does IPC (Inter-Process Communication) work in Cluster vs Child Process?

### **Cluster IPC**

- Automatic IPC setup between the master process and workers.
- Communication is done via `process.send()` and `process.on('message')`.
- Example:

  ```javascript
  // Worker process
  process.send({ type: "log", msg: "Worker did something!" });

  // Master process
  worker.on("message", (msg) => {
    console.log("Message from worker:", msg);
  });
  ```

### **Child Process IPC**

- Automatic IPC setup when using `child_process.fork()`.
- Communication is also done via `process.send()` and `process.on('message')`.
- Example:

  ```javascript
  // parent.js
  const { fork } = require("child_process");
  const child = fork("./child.js");

  child.send({ action: "start" });

  child.on("message", (msg) => {
    console.log("Received from child:", msg);
  });

  // child.js
  process.on("message", (msg) => {
    console.log("Received from parent:", msg);
    process.send({ result: "done" });
  });
  ```

---

## Key Differences in IPC

| **Feature**            | **Cluster**                          | **Child Process**                                         |
| ---------------------- | ------------------------------------ | --------------------------------------------------------- |
| **IPC Setup**          | Automatic between master and workers | Automatic (only with `fork()`)                            |
| **Typical Usage**      | Distribute and coordinate requests   | Send data or commands for isolated tasks                  |
| **Communication Type** | Message-based (via `.send()`)        | Message-based (via `.send()`)                             |
| **Shared Port**        | Yes (via master process)             | No (each child process must bind its own port, if needed) |

---

### 🧠 Summary:

- **Sharing the port in Cluster:** All workers serve requests on the same port, coordinated by the master process.
- **IPC in both Cluster and Child Process:** Works via message passing using `.send()` and `.on('message')`.
