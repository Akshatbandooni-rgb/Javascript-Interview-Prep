# Node.js All-In-One Guide

---

## 1. What is Node.js, and how does it work?

**Definition**:  
Node.js is an open-source, cross-platform JavaScript runtime built on Chrome’s V8 JavaScript engine. It allows you to run JavaScript code outside the browser, typically on the server-side.

### How it works:

1. Node.js uses the V8 engine to convert JavaScript code into machine code.
2. It uses a **non-blocking, event-driven architecture**.
3. At its core, it has an **event loop** and **libuv** (a C++ library for handling asynchronous I/O operations).

**Analogy**:  
Imagine Node.js as a chef who handles many small orders (I/O tasks) efficiently. Instead of waiting for one dish to be cooked (like blocking code), the chef quickly delegates it to assistants (like OS or libuv) and moves on to the next task.

**In short**:  
Node.js is a runtime environment that lets you run JavaScript on the server. It uses non-blocking I/O and an event loop to handle multiple operations efficiently.

---

## 2. How is Node.js different from JavaScript in the browser?

| **Feature**        | **Node.js**                      | **JavaScript in Browser**          |
| ------------------ | -------------------------------- | ---------------------------------- |
| **Environment**    | Runs on server                   | Runs in the browser                |
| **APIs**           | Access to file system, OS, etc.  | Access to DOM, local storage, etc. |
| **Modules**        | Uses CommonJS (`require`) or ESM | Uses ES modules (`import/export`)  |
| **Global Objects** | `global`, `process`              | `window`, `document`               |
| **Use Case**       | Backend/server-side development  | Frontend/client-side interactivity |

### Example:

```js
// In Node.js
const fs = require("fs");
fs.readFile("file.txt", "utf8", (err, data) => {
  console.log(data);
});

// In Browser
document.getElementById("btn").addEventListener("click", () => {
  alert("Button clicked!");
});
```

---

## 3. What are the advantages of using Node.js?

1. **Asynchronous & Non-blocking I/O**  
   Handles thousands of requests simultaneously.

2. **Single Programming Language (JavaScript)**  
   Same language on both frontend and backend.

3. **Fast Execution with V8 Engine**  
   Converts JavaScript into fast machine code.

4. **Large Ecosystem (NPM)**  
   Over 1 million packages/modules available.

5. **Real-time Apps**  
   Perfect for chat apps, games, live notifications.

6. **Scalability**  
   Ideal for microservices and distributed systems.

7. **Community Support**  
   Massive community with open-source contributions.

**Interview Tip**:  
"Node.js is ideal when you need performance, scalability, and real-time data handling with a lightweight backend."

---

## 4. What is the event loop in Node.js?

**Definition**:  
The event loop is the core mechanism in Node.js that handles asynchronous operations. It allows Node.js to perform non-blocking I/O even though it's single-threaded.

### How it works (simplified steps):

1. Node starts the event loop.
2. Executes synchronous code in the call stack.
3. Delegates async tasks (like `setTimeout`, file read) to libuv.
4. When async task completes, it adds the callback to the event queue.
5. Event loop picks callbacks from the queue when the stack is empty and executes them.

### Phases of the event loop (high-level):

1. **Timers** (e.g., `setTimeout`, `setInterval`)
2. **Pending callbacks**
3. **I/O callbacks**
4. **Idle, prepare**
5. **Poll** (waiting for I/O)
6. **Check** (e.g., `setImmediate`)
7. **Close callbacks**

### Example:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
```

**Output**:

```
Start
End
Timeout
```

**Why?**  
Even though the timeout is 0, it’s asynchronous and goes to the event queue.

---

## 5. What is the difference between synchronous and asynchronous code in Node.js?

| **Aspect**      | **Synchronous**         | **Asynchronous**                      |
| --------------- | ----------------------- | ------------------------------------- |
| **Execution**   | Line-by-line (blocking) | Non-blocking, uses callbacks/promises |
| **Performance** | Slower, blocks others   | Faster, allows multiple tasks at once |
| **Example**     | `fs.readFileSync()`     | `fs.readFile()`                       |

### Example:

```js
// Synchronous
const data = fs.readFileSync("file.txt", "utf8");
console.log(data); // Blocks here

// Asynchronous
fs.readFile("file.txt", "utf8", (err, data) => {
  console.log(data); // Non-blocking
});
```

**Analogy**:  
Synchronous code is like waiting at a red light—you can’t go anywhere until it turns green.  
Asynchronous code is like dropping a letter in the mailbox and continuing your walk—you don’t wait for the response.

**Interview Tip**:  
"In Node.js, asynchronous programming is the default pattern because it helps improve performance and scalability, especially for I/O-heavy applications."

---

## 6. How do you create and export a module in Node.js?

### 📦 Creating a Module (Local Module)

**math.js**:

```js
function add(a, b) {
  return a + b;
}

module.exports = {
  add,
};
```

### 📥 Importing the Module

**app.js**:

```js
const math = require("./math");

console.log(math.add(5, 3)); // Output: 8
```

### ✅ Exporting a Single Function or Variable

**math.js**:

```js
module.exports = function (a, b) {
  return a + b;
};
```

**app.js**:

```js
const add = require("./math");
console.log(add(2, 2)); // Output: 4
```

---

# Node.js All-In-One Guide

---

## 1. What is the purpose of the `require()` function in Node.js?

**Definition**:  
`require()` is a built-in Node.js function used to import modules — whether they're built-in (core), local, or third-party.

### Purpose:

- To bring external JavaScript files (modules) into your current file.
- It reads a JavaScript file, executes it, and returns the `exports` object.

### Example:

```js
// math.js
module.exports = {
  add: (a, b) => a + b,
};

// app.js
const math = require("./math");
console.log(math.add(2, 3)); // Output: 5
```

**Interview Tip**:  
"`require()` helps in modular programming by breaking code into reusable pieces."

---

## 2. What is the difference between `require` and `import`?

| **Feature**                | **`require`**                      | **`import`**                          |
| -------------------------- | ---------------------------------- | ------------------------------------- |
| **Module System**          | CommonJS                           | ES Modules (ESM)                      |
| **Synchronous/Async**      | Synchronous                        | Asynchronous (behind the scenes)      |
| **Usage**                  | Node.js (default)                  | Modern JavaScript (browser & Node.js) |
| **File Extension Support** | Works with `.js`, `.json`, `.node` | Needs file extensions in ESM          |

### Example (require):

```js
const fs = require("fs");
```

### Example (import):

```js
import fs from "fs";
```

### Important Notes:

- `require` works in CommonJS environments by default.
- To use `import` in Node.js, you must:
  1. Set `"type": "module"` in `package.json`, or
  2. Use `.mjs` file extension.

---

## 3. What are Callbacks in Node.js? Can you provide an example?

**Definition**:  
A callback is a function passed as an argument to another function, to be executed after an asynchronous task is completed.

### Why it's important in Node.js:

Node.js uses callbacks heavily due to its non-blocking I/O model.

### Example:

```js
const fs = require("fs");

fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

**Analogy**:  
Think of a callback like ordering food at a restaurant. You place the order (start the task), continue chatting (don’t block), and the waiter (callback) delivers your food once it’s ready.

**Interview Tip**:  
"Callbacks are one of the earliest ways to handle async operations, but can lead to callback hell if not managed properly."

---

## 4. What are Promises in Node.js? How do they work?

**Definition**:  
A Promise represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

### States of a Promise:

1. **Pending**
2. **Fulfilled**
3. **Rejected**

### Example:

```js
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Data received");
    }, 2000);
  });
};

fetchData()
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

### Why Promises?

- Avoids nested callbacks.
- Makes code more readable and easier to manage.

---

## 5. What is `async/await` in Node.js, and how is it used?

**Definition**:  
`async/await` is syntactic sugar over Promises. It allows you to write asynchronous code that looks like synchronous code.

- `async` makes a function return a Promise.
- `await` pauses execution until the Promise is resolved or rejected.

### Example:

```js
const fetchData = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Data received");
    }, 1000);
  });
};

const getData = async () => {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
};

getData();
```

### Benefits:

- Cleaner, more readable async code.
- Easier error handling using `try...catch`.

**Interview Tip**:  
"I prefer `async/await` in most modern Node.js code because it's easier to write and debug, but I understand how Promises and callbacks work under the hood."

---

## 🧠 Summary Table

| **Concept**   | **Description**                                           |
| ------------- | --------------------------------------------------------- |
| `require()`   | Imports modules using CommonJS syntax                     |
| `import`      | Imports modules using modern ES Modules                   |
| Callbacks     | Functions passed as arguments, run after async operations |
| Promises      | Objects that represent future success or failure          |
| `async/await` | Modern syntax for writing cleaner asynchronous code       |

---

## 6. What is the role of the `fs` (File System) module in Node.js?

**Explanation**:  
The `fs` module is a core module in Node.js that allows you to interact with the file system — meaning you can read, write, update, delete, and watch files and directories directly from your Node.js code.

### Why it exists:

Node.js runs on the server side. Any server must be able to:

1. Serve files (like HTML/CSS).
2. Log data.
3. Store or retrieve user-generated content (like image uploads or configs). This is where `fs` comes in.

### Features:

- Synchronous and asynchronous methods (with callback or promises).
- Work with both text and binary files.
- Supports streams for efficient reading/writing.

---

## 7. How do you read and write files in Node.js?

### 🖊️ Writing a file (Asynchronous):

```js
const fs = require("fs");

fs.writeFile("example.txt", "Hello, Node.js!", (err) => {
  if (err) {
    console.error("Error writing file:", err);
  } else {
    console.log("File written successfully!");
  }
});
```

### 📖 Reading a file (Asynchronous):

```js
fs.readFile("example.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error reading file:", err);
  } else {
    console.log("File contents:", data);
  }
});
```

### ⚡ Synchronous Version:

```js
const data = fs.readFileSync("example.txt", "utf8");
console.log(data);
```

**Interview Tip**:  
"In production, I prefer asynchronous methods to avoid blocking the event loop. Synchronous file access is good for quick scripts or startup tasks."

---

## 8. What is the `http` module in Node.js, and how do you create a basic server?

**Explanation**:  
The `http` module is a built-in Node.js module that allows you to create HTTP servers and handle HTTP requests and responses directly, without any external libraries.

### 🛠️ How it works:

You can:

1. Create a server with `http.createServer()`.
2. Listen to ports (e.g., port 3000).
3. Respond to HTTP methods like GET, POST, etc.

### 💡 Basic Server Example:

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.url === "/" && req.method === "GET") {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello, world!");
  } else {
    res.writeHead(404);
    res.end("Not Found");
  }
});

server.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
});
```

**Interview Tip**:  
"While `http` gives you full control, it can be verbose for large applications. That's where Express.js comes in to simplify things."

---

# Node.js All-In-One Guide

---

## 1. What is CORS, and how do you enable it in a Node.js application?

### 🔍 What is CORS?

CORS (Cross-Origin Resource Sharing) is a security feature implemented by browsers to restrict access to resources from a different origin (domain, port, or protocol).

### 🧠 Why it exists:

Imagine your frontend (React) running at `http://localhost:3000` is trying to make a request to a backend API at `http://localhost:5000`. This is a cross-origin request, and by default, the browser blocks it for security.

### 💡 How to Enable CORS in Node.js (Express):

Install the CORS middleware:

```bash
npm install cors
```

Use it in your app:

```js
const express = require("express");
const cors = require("cors");

const app = express();
app.use(cors()); // Enables CORS for all routes

app.get("/data", (req, res) => {
  res.json({ message: "CORS is working!" });
});

app.listen(5000);
```

### ⚙️ Advanced CORS Config:

```js
app.use(
  cors({
    origin: "http://localhost:3000",
    methods: ["GET", "POST"],
  })
);
```

### 🧠 Interview Tip:

"CORS is handled by the browser, not Node. We use middleware like `cors` to send appropriate headers to let the browser know it's okay to proceed."

---

## 2. What is the difference between `process.nextTick()` and `setImmediate()`?

### 🔁 Event Loop Context:

Both are timing functions in Node.js used to schedule callbacks, but they execute at different phases of the event loop.

### 🧠 Difference:

| **Function**         | **Executes When?**                                     | **Priority** |
| -------------------- | ------------------------------------------------------ | ------------ |
| `process.nextTick()` | Before the next event loop iteration (microtask queue) | High         |
| `setImmediate()`     | After the current I/O events, in the check phase       | Lower        |

### 🧪 Example:

```js
process.nextTick(() => {
  console.log("Next Tick");
});

setImmediate(() => {
  console.log("Set Immediate");
});

console.log("Main Code");
```

### 🖨️ Output:

```
Main Code
Next Tick
Set Immediate
```

### 🧠 Interview Tip:

"`nextTick()` is good for deferring work until the current operation completes. `setImmediate()` defers until I/O is complete. Be careful — too many `nextTick()` calls can block the event loop."

---

## 3. How do you handle errors in Node.js applications?

### 💥 Types of Errors:

1. **Synchronous Errors**
2. **Asynchronous Errors** (in callbacks, promises)
3. **Runtime Exceptions**

### ✅ Best Practices:

#### 1. Try-Catch (for sync and async/await):

```js
try {
  // code that may throw
} catch (error) {
  console.error("Error caught:", error);
}
```

#### 2. Error Handling in Callbacks:

```js
fs.readFile("nonexistent.txt", "utf8", (err, data) => {
  if (err) return console.error("File error:", err);
  console.log(data);
});
```

#### 3. Error Handling in Promises:

```js
fetchData()
  .then((data) => console.log(data))
  .catch((err) => console.error("Promise error:", err));
```

#### 4. Centralized Express Error Middleware:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

#### 5. Catching Uncaught Errors:

```js
process.on("uncaughtException", (err) => {
  console.error("Uncaught Exception:", err);
});
```

### 🧠 Interview Tip:

"Graceful error handling improves stability and user experience. I also log and monitor critical errors using tools like Winston or external logging services."

---

## 4. What is the difference between `console.log()` and `console.error()`?

| **Function**      | **Purpose**                    | **Output Stream**          |
| ----------------- | ------------------------------ | -------------------------- |
| `console.log()`   | General output, debugging info | `stdout` (standard output) |
| `console.error()` | Error messages                 | `stderr` (standard error)  |

### 🧪 Example:

```js
console.log("This is normal output");
console.error("This is an error");
```

### Why it matters:

- When logging is redirected (e.g., in production), `stderr` and `stdout` can go to different files or services.
- Helpful in monitoring, debugging, and alerting systems.

---

## 5. How do you exit a Node.js process programmatically?

### 🛑 Use: `process.exit([code])`

| **Code** | **Meaning**              |
| -------- | ------------------------ |
| `0`      | Success                  |
| `1`      | Failure (generic error)  |
| Others   | Custom exit status codes |

### 💡 Example:

```js
if (!configLoaded) {
  console.error("Configuration missing. Exiting...");
  process.exit(1); // Failure
}

// Else continue app...
console.log("App started successfully.");
process.exit(0); // Success
```

You can also use it in CLI tools:

```js
console.log("Done.");
process.exit(0);
```

### ⚠️ Warning:

Avoid using `process.exit()` abruptly in web servers unless it's critical, because it doesn’t wait for ongoing async operations to complete.

### 🧠 Interview Tip:

"I use `process.exit()` carefully — mostly in CLI tools or during critical startup checks, not during normal API server flow."

---

## 🧠 Summary Table

| **Concept**                      | **Summary**                                                                        |
| -------------------------------- | ---------------------------------------------------------------------------------- |
| **CORS**                         | Security mechanism to allow cross-origin resource access                           |
| **nextTick vs setImmediate**     | `nextTick()` runs before the event loop continues, `setImmediate()` runs after I/O |
| **Error Handling**               | Use `try/catch`, error middleware, and process event handlers                      |
| **console.log vs console.error** | Output goes to `stdout` and `stderr` respectively                                  |
| **process.exit()**               | Ends the Node.js process, optionally with an exit code                             |

---

## 6. Why do we need a success code in `process.exit()`?

### 🔍 Explanation:

`process.exit([code])` tells Node.js to exit the current process, and the code you pass indicates the result of the process to the OS or any script monitoring the process.

- `0` → Success (everything ran fine)
- Non-zero (e.g., `1`, `2`, `127`) → Failure (used to signal different error types)

### 📦 Use Cases:

1. In CI/CD pipelines, exit codes determine if a build/test passes or fails.
2. In scripts, the shell can check `echo $?` for the last command's exit code.
3. In process managers (like PM2 or Docker), exit codes help with monitoring/restarting services.

### 💡 Example:

```js
if (!dbConnected) {
  console.error("Database not connected. Exiting...");
  process.exit(1); // Failure
}

console.log("App started successfully.");
process.exit(0); // Success
```

---

## 7. What happens to asynchronous tasks when we call `process.exit()`?

### ⚠️ Important:

`process.exit()` immediately shuts down the process. That means any pending asynchronous operations (timers, network requests, file I/O, DB queries, etc.) are aborted.

### ❌ Example (bad):

```js
setTimeout(() => {
  console.log("This will never run.");
}, 1000);

process.exit(0); // exits before timeout callback runs
```

### ✅ Proper Way to Wait:

Let async operations finish before exiting:

```js
setTimeout(() => {
  console.log("Finishing async task");
  process.exit(0); // now safe to exit
}, 1000);
```

### 🧠 Best Practice:

Never use `process.exit()` while async tasks are still pending unless you're in a fail-fast situation, and you’re okay with losing unsaved data or responses.

---

## ✅ Summary

| **Concept**                     | **Explanation**                                                            |
| ------------------------------- | -------------------------------------------------------------------------- |
| `process.exit(0)`               | Tells the system the process ended successfully                            |
| `process.exit(1)`               | Signals an error occurred (used in scripts, CI/CD, monitoring tools)       |
| **What happens to async tasks** | They are immediately terminated; code inside callbacks won't execute       |
| **Best practice**               | Wait for async tasks to complete, or exit only when it’s safe to terminate |

---

# Node.js All-In-One Guide

---

## 1. How does the Node.js event-driven architecture work?

### 📌 Core Idea:

Node.js is event-driven and non-blocking, which means it doesn't wait for tasks to finish. Instead, it registers callbacks and continues execution. When an event occurs, it notifies the callback.

### 📦 Components:

- **Event Emitter**: Used to create and listen to custom events.
- **Event Loop**: Orchestrates how tasks are handled.
- **Callbacks/Handlers**: Invoked when a certain event is fired.
- **Async APIs**: Examples include `fs.readFile()`, HTTP requests, and database queries.

### 🔄 How it works:

```
┌────────────┐
│ Call Stack │ <— Executes code line-by-line
└────────────┘
        ↓
┌────────────┐
│ Node APIs  │ <— Async functions like fs, timers, etc.
└────────────┘
        ↓
┌───────────────┐
│ Event Queue    │ <— When async work is done, callback is queued
└───────────────┘
        ↓
┌─────────────┐
│ Event Loop   │ <— Picks callbacks from queue and runs them
└─────────────┘
```

### 🧠 Interview Tip:

"Node.js’s event-driven architecture makes it ideal for I/O-heavy apps. It avoids thread overhead by using a single-threaded event loop that offloads heavy tasks to the background."

---

## 2. What is the difference between the event loop phases?

The Event Loop in Node.js runs in phases, and each phase handles a specific type of callback. Here's a simplified breakdown:

```
┌────────────────────┐
│ Timers             │ → Executes `setTimeout()` and `setInterval()`.
├────────────────────┤
│ Pending Callbacks  │ → Executes I/O callbacks deferred to the next loop.
├────────────────────┤
│ Idle/Prepare       │ → Internal use (not in userland).
├────────────────────┤
│ Poll               │ → Retrieves new I/O events and executes I/O callbacks.
├────────────────────┤
│ Check              │ → Executes `setImmediate()` callbacks.
├────────────────────┤
│ Close Callbacks    │ → Handles things like `socket.on('close')`.
└────────────────────┘
```

### 🧪 Example:

```js
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
```

### 🖨️ Output:

```
immediate
timeout
```

**Why?**  
`setImmediate()` is executed in the **Check phase**, while `setTimeout()` is executed in the **Timers phase**.

---

## 3. What is a stream in Node.js? What are its types?

### 📌 Definition:

A Stream is a Node.js interface for working with continuous chunks of data (like files or network connections) instead of loading the entire data into memory.

### 🎯 Why Streams?

- Efficient for large data.
- Memory-friendly.
- Processes data in chunks rather than all at once.

### 📦 Types of Streams:

| **Stream Type** | **Description**                      | **Example**              |
| --------------- | ------------------------------------ | ------------------------ |
| **Readable**    | Data can be read from it.            | `fs.createReadStream()`  |
| **Writable**    | Data can be written to it.           | `fs.createWriteStream()` |
| **Duplex**      | Can read and write simultaneously.   | TCP sockets              |
| **Transform**   | Modifies data while reading/writing. | `zlib.createGzip()`      |

### 🧪 Example:

```js
const fs = require("fs");
const stream = fs.createReadStream("largefile.txt");

stream.on("data", (chunk) => {
  console.log("Received:", chunk.length, "bytes");
});
```

---

## 4. How does buffering differ from streaming in Node.js?

| **Concept**     | **Buffering**                            | **Streaming**                       |
| --------------- | ---------------------------------------- | ----------------------------------- |
| **Data Flow**   | Entire data is loaded into memory first. | Data is processed in chunks.        |
| **Performance** | Slower, uses more memory.                | Faster, memory-efficient.           |
| **Use Case**    | Small files, APIs that need full input.  | Large files, real-time video/audio. |

### 🧪 Analogy:

- **Buffering**: Downloading a movie before watching.
- **Streaming**: Watching a movie online as it loads.

---

## 5. How do you handle large file uploads in Node.js?

### ❌ Bad Approach:

Using `req.on('data')` and storing everything in memory can crash the server for large files.

### ✅ Best Practices:

1. **Use a streaming parser** like `multer`, `busboy`, or `formidable`.

#### Example with `multer`:

```bash
npm install multer
```

```js
const express = require("express");
const multer = require("multer");
const upload = multer({ dest: "uploads/" });

const app = express();

app.post("/upload", upload.single("file"), (req, res) => {
  console.log(req.file); // Metadata
  res.send("File uploaded!");
});
```

**How it works**:

- The file is streamed directly to disk.
- No buffering in memory.
- Efficient even for GB-sized uploads.

### 🧠 Interview Tip:

"For large uploads, I use streaming libraries like `multer` with disk storage or S3 upload streams. This avoids memory bottlenecks and scales well with concurrent users."

---

## 🧠 Final Summary

| **Question**              | **Key Takeaway**                                                            |
| ------------------------- | --------------------------------------------------------------------------- |
| Event-driven architecture | Uses callbacks + Event Loop to handle I/O asynchronously.                   |
| Event loop phases         | Different stages for timers, I/O, `setImmediate()`, etc.                    |
| Streams                   | Handle data chunk by chunk, ideal for large inputs/outputs.                 |
| Buffering vs Streaming    | Buffering waits for full data; streaming processes it on the go.            |
| Large file uploads        | Use `multer`, `busboy`, or direct stream handling to avoid memory overload. |

---

## ✅ What is **clustering** in Node.js, and how does it improve performance?

### 🔹 Problem:

Node.js runs in a **single thread** — meaning it can use **only one CPU core** at a time. So if you have an 8-core CPU, Node will still use just 1 core unless you do something extra.

### 🔹 Solution: Clustering

Clustering allows you to **run multiple instances** of your Node.js application, each on a different CPU core.

These instances (called **workers**) are created using Node's built-in `cluster` module.

They all share the same port, and the load is distributed between them.

### 🛠 Example:

```js
const cluster = require("cluster");
const http = require("http");
const os = require("os");

if (cluster.isMaster) {
  const cpuCount = os.cpus().length;

  for (let i = 0; i < cpuCount; i++) {
    cluster.fork();
  }

  cluster.on("exit", (worker) => {
    console.log(`Worker ${worker.process.pid} died`);
    cluster.fork();
  });
} else {
  http
    .createServer((req, res) => {
      res.end(`Handled by worker ${process.pid}`);
    })
    .listen(3000);
}
```

### ✅ Benefits:

- Uses all CPU cores → faster performance
- If one worker crashes, others still handle traffic
- Scales horizontally with zero downtime

---

## ✅ What are **worker threads** in Node.js?

### 🔹 Problem:

Node.js is single-threaded and struggles with **CPU-heavy tasks** (e.g., image resizing, file encryption, large computations) that can block the main thread.

### 🔹 Solution: Worker Threads

Worker threads allow you to **run code in a separate thread** (parallel) — inside the same process — without blocking the main event loop.

It's part of the `worker_threads` module.

### 🛠 Example:

```js
// main.js
const { Worker } = require("worker_threads");

const worker = new Worker("./heavy-task.js");

worker.postMessage("start");

worker.on("message", (data) => {
  console.log("Result from worker:", data);
});
```

```js
// heavy-task.js
const { parentPort } = require("worker_threads");

parentPort.on("message", () => {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) sum += i;
  parentPort.postMessage(sum);
});
```

### ✅ Benefits:

- Perfect for CPU-bound tasks
- Doesn’t block the main thread
- Lightweight compared to spawning a new process

---

## 🔄 Difference between **clustering** and **worker threads**

| Feature              | Clustering                       | Worker Threads                    |
| -------------------- | -------------------------------- | --------------------------------- |
| Uses CPU Cores       | Yes (multiple processes)         | Yes (multiple threads)            |
| Memory Usage         | More (each worker = new process) | Less (shared memory)              |
| Communication Method | Inter-process (slower)           | Message passing within process    |
| Best for             | Scaling web servers              | CPU-heavy tasks like calculations |

---

## ✅ Difference between `spawn()`, `exec()`, and `fork()` in Node.js

All of these are from Node's `child_process` module and are used to run **external commands or scripts**.

### 🔹 `spawn()`

- Starts a new process
- Streams the output (`stdout`, `stderr`)
- Great for **large data** or long-running commands

```js
const { spawn } = require("child_process");
const child = spawn("ls", ["-lh", "/usr"]);

child.stdout.on("data", (data) => {
  console.log(`Output: ${data}`);
});
```

### 🔹 `exec()`

- Starts a new process
- Buffers the entire output (not stream)
- Better for **small, quick commands**

```js
const { exec } = require("child_process");
exec("ls -lh", (error, stdout, stderr) => {
  console.log(stdout);
});
```

### 🔹 `fork()`

- Special version of `spawn()` for **running another Node.js script**
- Has built-in communication via `send()` and `on('message')`

```js
// parent.js
const { fork } = require("child_process");
const child = fork("child.js");

child.send({ message: "Hello child!" });

child.on("message", (data) => {
  console.log("Message from child:", data);
});
```

```js
// child.js
process.on("message", (data) => {
  console.log("Message from parent:", data);
  process.send({ message: "Hello back!" });
});
```

---

## ✅ How does Node.js handle **memory management** and **garbage collection**?

Node.js uses **V8 engine** for JavaScript, and V8 handles memory automatically.

### 📦 Memory is split into:

- **Heap** – where objects, arrays, closures are stored
- **Stack** – function calls and local variables

### ♻️ Garbage Collection (GC)

Garbage Collection = cleaning up memory that is **no longer used**.

V8 runs this automatically using:

- **Mark-and-sweep**
- **Minor GC** for short-lived objects
- **Major GC** for long-lived ones

### ⚙️ You can inspect memory with:

```js
console.log(process.memoryUsage());
```

And change memory limit with:

```bash
node --max-old-space-size=4096 app.js
```

---

## ✅ How to prevent **memory leaks** in Node.js?

### ⚠️ Common Causes:

- Global variables
- Forgotten `setInterval` or `setTimeout`
- Unremoved event listeners
- Caching too much data in memory
- Closures that retain variables unnecessarily

### ✅ Best Practices to Prevent Leaks:

1. **Clear intervals and timeouts**

```js
const interval = setInterval(() => {}, 1000);
clearInterval(interval);
```

2. **Remove unused listeners**

```js
emitter.removeListener("eventName", listener);
```

3. **Use `WeakMap` or `WeakSet`** for caching (auto cleaned by GC)

4. **Avoid unbounded in-memory storage**

5. **Monitor with tools**:
   - Chrome DevTools (`--inspect`)
   - Heap snapshots
   - `heapdump` or `memwatch-next`

### 🧠 Interview Pro Tip:

> “I keep my app healthy by monitoring memory with `process.memoryUsage()`, avoiding unnecessary object retention, and regularly profiling with Chrome DevTools.”

---

# Clustering vs. Worker Threads — What's the Difference?

| **Feature**          | **Cluster**                                     | **Worker Threads**                                 |
| -------------------- | ----------------------------------------------- | -------------------------------------------------- |
| **Parallelism Type** | Multi-process                                   | Multi-thread                                       |
| **Process Type**     | Each worker is a separate Node.js process       | All threads run inside the same process            |
| **Memory**           | Each process has its own memory heap            | Threads share memory (can use `SharedArrayBuffer`) |
| **Communication**    | Slower (uses IPC - Inter-Process Communication) | Faster (in-process messaging)                      |
| **Use Case**         | Scale network servers (like Express.js apps)    | Handle CPU-intensive or blocking tasks             |
| **Overhead**         | More overhead (separate processes)              | Less overhead (lightweight threads)                |
| **Fault Tolerance**  | One thread crash doesn’t affect others          | A crash in one thread can affect the main process  |

---

## 🧠 When to Use Cluster

### ✅ Use Cases:

- Handling high traffic web requests  
  e.g., Express, Koa, REST APIs
- Scaling horizontally across CPU cores
- Deploying on multi-core machines
- When your app is mostly I/O bound (disk, network)

### 🛠 Example:

A web server using all 8 cores of your CPU to serve incoming users faster.

```bash
# Without cluster: Only 1 core is used
node server.js

# With cluster: Uses all 8 cores
cluster.fork() x 8
```

---

## 🧠 When to Use Worker Threads

### ✅ Use Cases:

- Heavy CPU tasks that would block the event loop:
  - Data processing
  - Image resizing
  - Encryption/Decryption
  - Machine learning computations
- You want to avoid blocking main thread responsiveness.
- You want lightweight performance (lower memory use).

### 🛠 Example:

Compressing a large image or computing a hash of a 5GB file.

```js
// Without worker: main thread is blocked ❌
// With worker: task runs in background thread ✅
```

---

## 🎯 TL;DR: When to Use What?

| **Situation**                               | **Use Cluster or Worker Threads?** |
| ------------------------------------------- | ---------------------------------- |
| Web server under high traffic               | ✅ Cluster                         |
| CPU-heavy task blocking requests            | ✅ Worker Threads                  |
| Memory isolation needed (security)          | ✅ Cluster                         |
| Background computation with shared memory   | ✅ Worker Threads                  |
| Microservices or multiple apps on same port | ✅ Cluster                         |
| Real-time analytics or parsing large files  | ✅ Worker Threads                  |

---

## 👨‍🏫 Interview Tip:

"Clusters are great for scaling a Node.js server across CPU cores, while Worker Threads are best when I need to offload heavy CPU tasks without freezing the main event loop."

Let me know if you want me to write this out as a PDF or Markdown too!
