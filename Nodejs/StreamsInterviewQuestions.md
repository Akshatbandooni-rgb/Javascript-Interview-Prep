# Streams in Node.js

---

## readFile vs createReadStream

### 📘 `fs.readFile`

- Reads the entire file into memory.
- Ideal for small files.
- It's asynchronous, but not memory-efficient for large files.

```javascript
const fs = require("fs");

fs.readFile("bigfile.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

---

### 📘 `fs.createReadStream`

- Reads the file in chunks using a stream.
- Ideal for large files (e.g., videos, logs, CSVs).
- Memory-efficient, because it doesn't load the whole file at once.

```javascript
const fs = require("fs");

const stream = fs.createReadStream("bigfile.txt", "utf8");

stream.on("data", (chunk) => {
  console.log("Received chunk:", chunk);
});

stream.on("end", () => {
  console.log("Done reading file.");
});
```

---

### 🧠 Analogy:

- **`readFile`**: Reading a whole book into memory before reading a page.
- **`createReadStream`**: Reading the book page-by-page.

---

## 🧱 What is a Buffer?

- A Buffer is Node.js's way of handling binary data directly in memory.
- Useful when reading files, network packets, or any binary data.

```javascript
const buf = Buffer.from("hello"); // UTF-8 by default
console.log(buf); // <Buffer 68 65 6c 6c 6f>
console.log(buf.toString()); // 'hello'
```

- It works with streams: when you read a file via a stream, chunks are buffers unless you set encoding.

---

## 🔄 What is a Stream?

- A stream is an abstract interface for working with streaming data — data that may not be available all at once and needs to be processed piece-by-piece.
- Helps avoid loading everything into memory at once.

---

## 🔢 Types of Streams

- **Readable** – Can be read from.  
  _e.g., `fs.createReadStream()`_

- **Writable** – Can be written to.  
  _e.g., `fs.createWriteStream()`_

- **Duplex** – Both readable and writable.  
  _e.g., TCP sockets._

- **Transform** – Duplex stream that can modify or transform data.  
  _e.g., gzip compression._

---

## ⚡ How Streams Optimize I/O

- Instead of waiting for an entire file to load into memory, Node reads small chunks and passes them to your code.
- Less memory consumption and faster processing.

### Ideal for:

- Large file processing
- HTTP responses
- Real-time data

---

## 💧 What is High Watermark?

- A setting that defines how much data to store in the internal buffer before pausing reads/writes.
- For readable streams, it’s the max number of bytes to buffer.

### Defaults:

- 16 KB for binary
- 16 KB for strings

```javascript
const stream = fs.createReadStream("file.txt", {
  highWaterMark: 64 * 1024, // 64 KB chunks
});
```

---

## 📢 Readable Stream Events

| **Event**    | **Description**                        |
| ------------ | -------------------------------------- |
| **data**     | Emitted when a chunk is available.     |
| **end**      | No more data.                          |
| **error**    | An error occurred.                     |
| **readable** | Stream is ready to be read.            |
| **close**    | Stream and underlying resource closed. |

---

## 🖊 Writable Stream Events

| **Event**  | **Description**                            |
| ---------- | ------------------------------------------ |
| **drain**  | Buffer is empty, ready for more writes.    |
| **finish** | All writes are completed.                  |
| **error**  | Error in writing.                          |
| **pipe**   | Stream is piped into this writable stream. |
| **unpipe** | Pipe has been removed.                     |

---

## ⚠️ Stream Performance Issues

- If you write too fast and don't handle backpressure, memory will spike.
- If you read faster than you process/write, chunks may be lost or cause overload.

### Solution: Handle backpressure properly.

---

## ⛔ What is Backpressure?

- Happens when a writable stream can’t handle the speed of incoming data from a readable stream.
- Imagine pouring water into a bottle with a narrow neck — eventually, it spills (memory overload).

### Fix:

```javascript
const readable = fs.createReadStream("big.txt");
const writable = fs.createWriteStream("copy.txt");

readable.pipe(writable); // Automatically handles backpressure!
```

---

## ✅ How to Copy Enormous Files Using Streams

```javascript
const fs = require("fs");

const readStream = fs.createReadStream("large-file.mp4");
const writeStream = fs.createWriteStream("copy.mp4");

readStream.pipe(writeStream);

writeStream.on("finish", () => {
  console.log("File copied successfully!");
});
```

- This is memory-efficient and backpressure-safe.

---

## 📡 What is Piping / Stream Chaining?

- Use `.pipe()` to connect streams: `readable ➝ transform ➝ writable`.

```javascript
const fs = require("fs");
const zlib = require("zlib");

fs.createReadStream("file.txt")
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream("file.txt.gz"));
```

---

## ⚠️ Why not use `.pipe()` in Production?

- `.pipe()` doesn’t catch errors well. If one stream errors, others may leak resources.

### Instead, use `pipeline()`:

```javascript
const { pipeline } = require("stream");
const fs = require("fs");
const zlib = require("zlib");

pipeline(
  fs.createReadStream("file.txt"),
  zlib.createGzip(),
  fs.createWriteStream("file.txt.gz"),
  (err) => {
    if (err) console.error("Pipeline failed", err);
    else console.log("Pipeline succeeded");
  }
);
```

---

## 🛠 How to Implement a Custom Stream

### Example: A Transform Stream that Uppercases Text

```javascript
const { Transform } = require("stream");

const upperCase = new Transform({
  transform(chunk, encoding, callback) {
    const upper = chunk.toString().toUpperCase();
    callback(null, upper);
  },
});

// Usage:
process.stdin.pipe(upperCase).pipe(process.stdout);
```

---

## 🧠 Summary Chart

| **Concept**          | **Key Point**                                    |
| -------------------- | ------------------------------------------------ |
| **readFile**         | Loads whole file into memory                     |
| **createReadStream** | Reads file in chunks, better for large files     |
| **Buffer**           | Binary data handling                             |
| **Stream**           | Chunk-based data processing                      |
| **Types**            | Readable, Writable, Duplex, Transform            |
| **High Watermark**   | Buffer limit before pausing                      |
| **Backpressure**     | Flow control to prevent overload                 |
| **Pipe**             | Chain streams together                           |
| **Pipeline**         | Safer, production-ready alternative to `.pipe()` |
| **Custom Stream**    | Extend Transform, Readable, or Writable classes  |
