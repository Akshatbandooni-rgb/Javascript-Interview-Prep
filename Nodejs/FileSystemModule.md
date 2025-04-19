# 📁 Node.js `fs` Module - Interview Questions & Answers

---

## 🔹 Basic Level

### 1. What is the `fs` module in Node.js?

The `fs` (File System) module provides APIs for interacting with the file system. It supports reading, writing, appending, renaming, and deleting files or directories.

```js
const fs = require("fs");
```

### 2. How do you read a file in Node.js?

#### Asynchronous - Reading a File

```js
fs.readFile("example.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

#### Synchronous (Error Handling)

```js
const data = fs.readFileSync("example.txt", "utf8");
console.log(data);
```

### 3. What’s the difference between `readFile` and `readFileSync`?

| Feature         | `readFile()` (Async) | `readFileSync()` (Sync) |
| --------------- | -------------------- | ----------------------- |
| **Blocking**    | Non-blocking         | Blocking                |
| **Usage**       | Uses callback        | Returns data directly   |
| **Performance** | Better for servers   | Simpler for scripts     |
| **Execution**   | Doesn’t halt         | Halts until done        |

---

### 4. How do you write data to a file in Node.js?

#### Asynchronous - Writing Data

```js
fs.writeFile("example.txt", "Hello Node!", (err) => {
  if (err) throw err;
  console.log("File written!");
});
```

#### Synchronous

```js
fs.writeFileSync("example.txt", "Hello Node!");
```

---

### 5. How do you check if a file exists?

```js
if (fs.existsSync("example.txt")) {
  console.log("File exists!");
} else {
  console.log("File does not exist.");
}
```

---

## 🔹 Intermediate Level

### 6. How can you append content to an existing file?

#### Asynchronous

```js
fs.appendFile("example.txt", "\nNew line added.", (err) => {
  if (err) throw err;
  console.log("Content appended!");
});
```

#### Synchronous

```js
fs.appendFileSync("example.txt", "\nNew line added.");
```

---

### 7. What happens if the file doesn’t exist while using `readFile()`?

`fs.readFile()` throws an error in the callback if the file doesn't exist.

```js
fs.readFile("nonexistent.txt", "utf8", (err, data) => {
  if (err) {
    console.error("File not found:", err.message);
    return;
  }
  console.log(data);
});
```

---

### 8. How do you delete a file using `fs`?

#### Asynchronous

```js
fs.unlink("example.txt", (err) => {
  if (err) throw err;
  console.log("File deleted!");
});
```

#### Synchronous

```js
fs.unlinkSync("example.txt");
```

---

### 9. How do you rename a file in Node.js?

#### Asynchronous

```js
fs.rename("oldname.txt", "newname.txt", (err) => {
  if (err) throw err;
  console.log("File renamed!");
});
```

#### Synchronous

```js
fs.renameSync("oldname.txt", "newname.txt");
```

---

### 10. What is the purpose of `fs.stat()`?

`fs.stat()` is used to get metadata about a file or directory.

```js
fs.stat("example.txt", (err, stats) => {
  if (err) throw err;
  console.log(stats.isFile()); // true
  console.log(stats.size); // Size in bytes
});
```

---

## 🔹 Advanced Level

### 11. Explain the difference between synchronous and asynchronous methods in the `fs` module

| Feature            | Synchronous                    | Asynchronous                               |
| ------------------ | ------------------------------ | ------------------------------------------ |
| **Blocking**       | Yes                            | No                                         |
| **Performance**    | Slower for concurrent requests | Scalable                                   |
| **Use Case**       | Scripts, quick tasks           | Web servers, APIs                          |
| **Error Handling** | `try/catch`                    | Callback or `try/catch` with `async/await` |

---

### 12. How would you handle errors while reading a file?

#### Asynchronous

```js
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error reading file:", err.message);
    return;
  }
  console.log(data);
});
```

#### Synchronous

```js
try {
  const data = fs.readFileSync("file.txt", "utf8");
  console.log(data);
} catch (err) {
  console.error("Error reading file:", err.message);
}
```
