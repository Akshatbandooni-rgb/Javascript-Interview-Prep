# 📘 Node.js HTTP Module – Complete Interview Guide

The `http` module in Node.js is a **core module** that enables you to create HTTP servers and make HTTP requests. It's fundamental for building **web servers, REST APIs**, and **client-server communication**.

---

## 🔍 What is the `http` module in Node.js?

- A **built-in** module in Node.js.
- No need to install using `npm install`.
- Provides tools to:
  - Create **HTTP servers**.
  - Make **HTTP client requests**.

📌 **Useful for**:

- Building RESTful APIs.
- Creating custom backends.
- Making calls to external services (APIs, microservices).

---

## 🎯 Purpose of the `http` Module

1. **Create a Server** that listens for HTTP requests (like GET, POST).
2. **Send HTTP Requests** to other services or endpoints as a client.

---

## 🧠 How to Use the HTTP Module

### 1. Create a Basic HTTP Server

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello, Akshat!");
});

server.listen(3000, () => {
  console.log("Server running at http://localhost:3000");
});
```

---

### 2. Make a GET Request (Client)

```js
const http = require("http");

http.get("http://example.com", (res) => {
  let data = "";

  res.on("data", (chunk) => (data += chunk));
  res.on("end", () => console.log(data));
});
```

---

## 🔧 Common Methods and Classes

| **Method**            | **Description**                                 |
| --------------------- | ----------------------------------------------- |
| `http.createServer()` | Creates a server instance.                      |
| `server.listen(port)` | Starts listening for requests.                  |
| `http.get()`          | Makes a simple GET request.                     |
| `http.request()`      | Makes advanced HTTP requests (GET, POST, etc.). |
| `req.on('data')`      | Listens for data in the request body.           |
| `req.on('end')`       | Triggered when request data is fully received.  |
| `res.writeHead()`     | Sets status code and headers.                   |
| `res.write()`         | Sends data chunks to the client.                |
| `res.end()`           | Finalizes and sends the full response.          |

---

## 📦 Handling JSON Response

```js
res.writeHead(200, { "Content-Type": "application/json" });
res.end(JSON.stringify({ message: "Success", user: "Akshat" }));
```

---

## 📩 Manually Handling POST Request Data

POST data comes in chunks and must be collected manually.

### Example

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.method === "POST" && req.url === "/submit") {
    let body = "";

    req.on("data", (chunk) => {
      body += chunk.toString(); // Convert Buffer to string
    });

    req.on("end", () => {
      try {
        const parsed = JSON.parse(body);
        res.writeHead(200, { "Content-Type": "application/json" });
        res.end(JSON.stringify({ message: "Data received", data: parsed }));
      } catch (err) {
        res.writeHead(400, { "Content-Type": "application/json" });
        res.end(JSON.stringify({ error: "Invalid JSON" }));
      }
    });
  } else {
    res.writeHead(404);
    res.end("Not Found");
  }
});

server.listen(3000, () =>
  console.log("Server running on http://localhost:3000")
);
```

---

## 🧪 Difference Between `http.get()` and `http.request()`

| **Feature**    | **`http.get()`** | **`http.request()`**            |
| -------------- | ---------------- | ------------------------------- |
| **Method**     | Only GET         | Supports GET, POST, PUT, DELETE |
| **Simplicity** | Very simple      | More configurable               |
| **Use Case**   | Fetch data       | Send data or custom headers     |

---

### 🧪 `http.request()` with POST Example

```js
const options = {
  hostname: "example.com",
  path: "/api",
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
};

const req = http.request(options, (res) => {
  res.on("data", (chunk) => console.log(chunk.toString()));
});

req.write(JSON.stringify({ name: "Akshat" }));
req.end();
```

---

## ⚙️ Events on `req` and `res` Objects

### On `req` (Incoming request)

- **`data`** – Emitted when a data chunk is received.
- **`end`** – Emitted when all data is received.

### On `res` (Server response)

- **`write`** – Sends a chunk of data.
- **`end`** – Ends the response.
- **`writeHead`** – Sets status and headers.

---

## 🚀 Difference Between `res.write()` vs `res.end()`

| **Method**    | **Description**                                |
| ------------- | ---------------------------------------------- |
| `res.write()` | Sends part of the response (chunk).            |
| `res.end()`   | Ends the response. Can include the last chunk. |

### Example

```js
res.write("Hello ");
res.write("World");
res.end("!");
```

---

## 💡 Why Use `http` Module Instead of Express?

| **Feature**        | **Express**                                                                                                                                 | **HTTP Module**                 |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| **Routing**        | `app.get()`, `app.post()`                                                                                                                   | Manual `if` checks              |
| **JSON Parsing**   | `express.json()` middleware                                                                                                                 | Manually with `req.on('data')`  |
| **Send Response**  | `res.status().json()`                                                                                                                       | `res.writeHead()` + `res.end()` |
| **Error Handling** | Cleaner (middleware supported) <!-- filepath: c:\Users\akshatbandooni\Desktop\DoNotTouch\Javascript-Interview-Prep\Nodejs\httpModule.md --> |

# 📘 Node.js HTTP Module – Complete Interview Guide

The `http` module in Node.js is a **core module** that enables you to create HTTP servers and make HTTP requests. It's fundamental for building **web servers, REST APIs**, and **client-server communication**.

---

## 🔍 What is the `http` module in Node.js?

- A **built-in** module in Node.js.
- No need to install using `npm install`.
- Provides tools to:
  - Create **HTTP servers**.
  - Make **HTTP client requests**.

📌 **Useful for**:

- Building RESTful APIs.
- Creating custom backends.
- Making calls to external services (APIs, microservices).

---

## 🎯 Purpose of the `http` Module

1. **Create a Server** that listens for HTTP requests (like GET, POST).
2. **Send HTTP Requests** to other services or endpoints as a client.

---

## 🧠 How to Use the HTTP Module

### 1. Create a Basic HTTP Server

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello, Akshat!");
});

server.listen(3000, () => {
  console.log("Server running at http://localhost:3000");
});
```

---

### 2. Make a GET Request (Client)

```js
const http = require("http");

http.get("http://example.com", (res) => {
  let data = "";

  res.on("data", (chunk) => (data += chunk));
  res.on("end", () => console.log(data));
});
```

---

## 🔧 Common Methods and Classes

| **Method**            | **Description**                                 |
| --------------------- | ----------------------------------------------- |
| `http.createServer()` | Creates a server instance.                      |
| `server.listen(port)` | Starts listening for requests.                  |
| `http.get()`          | Makes a simple GET request.                     |
| `http.request()`      | Makes advanced HTTP requests (GET, POST, etc.). |
| `req.on('data')`      | Listens for data in the request body.           |
| `req.on('end')`       | Triggered when request data is fully received.  |
| `res.writeHead()`     | Sets status code and headers.                   |
| `res.write()`         | Sends data chunks to the client.                |
| `res.end()`           | Finalizes and sends the full response.          |

---

## 📦 Handling JSON Response

```js
res.writeHead(200, { "Content-Type": "application/json" });
res.end(JSON.stringify({ message: "Success", user: "Akshat" }));
```

## 📩 Manually Handling POST Request Data

POST data comes in chunks and must be collected manually.

### Example

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.method === "POST" && req.url === "/submit") {
    let body = "";

    req.on("data", (chunk) => {
      body += chunk.toString(); // Convert Buffer to string
    });

    req.on("end", () => {
      try {
        const parsed = JSON.parse(body);
        res.writeHead(200, { "Content-Type": "application/json" });
        res.end(JSON.stringify({ message: "Data received", data: parsed }));
      } catch (err) {
        res.writeHead(400, { "Content-Type": "application/json" });
        res.end(JSON.stringify({ error: "Invalid JSON" }));
      }
    });
  } else {
    res.writeHead(404);
    res.end("Not Found");
  }
});

server.listen(3000, () =>
  console.log("Server running on http://localhost:3000")
);
```

---

## 🧪 Difference Between `http.get()` and `http.request()`

| **Feature**    | **`http.get()`** | **`http.request()`**            |
| -------------- | ---------------- | ------------------------------- |
| **Method**     | Only GET         | Supports GET, POST, PUT, DELETE |
| **Simplicity** | Very simple      | More configurable               |
| **Use Case**   | Fetch data       | Send data or custom headers     |

---

### 🧪 `http.request()` with POST Example

```js
const options = {
  hostname: "example.com",
  path: "/api",
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
};

const req = http.request(options, (res) => {
  res.on("data", (chunk) => console.log(chunk.toString()));
});

req.write(JSON.stringify({ name: "Akshat" }));
req.end();
```

---

## ⚙️ Events on `req` and `res` Objects

### On `req` (Incoming request)

- **`data`** – Emitted when a data chunk is received.
- **`end`** – Emitted when all data is received.

### On `res` (Server response)

- **`write`** – Sends a chunk of data.
- **`end`** – Ends the response.
- **`writeHead`** – Sets status and headers.

---

## 🚀 Difference Between `res.write()` vs `res.end()`

| **Method**    | **Description**                                |
| ------------- | ---------------------------------------------- |
| `res.write()` | Sends part of the response (chunk).            |
| `res.end()`   | Ends the response. Can include the last chunk. |

### Example

```js
res.write("Hello ");
res.write("World");
res.end("!");
```

---

## 💡 Why Use `http` Module Instead of Express?

| **Feature**        | **Express**                    | **HTTP Module**                 |
| ------------------ | ------------------------------ | ------------------------------- |
| **Routing**        | `app.get()`, `app.post()`      | Manual `if` checks              |
| **JSON Parsing**   | `express.json()` middleware    | Manually with `req.on('data')`  |
| **Send Response**  | `res.status().json()`          | `res.writeHead()` + `res.end()` |
| **Error Handling** | Cleaner (middleware supported) |
