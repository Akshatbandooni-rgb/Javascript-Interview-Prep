# 📜 Node.js Streams - Complete Guide

---

## What are Streams in Node.js?

Streams are a way to handle reading and writing data in chunks—efficiently and asynchronously.

Instead of reading or writing the entire data at once (like a full file), streams break it down into chunks, making them memory-efficient and faster, especially for large data (like videos, files, or network requests).

---

## ✅ Types of Streams in Node.js

| **Type**      | **Description**                                                                    |
| ------------- | ---------------------------------------------------------------------------------- |
| **Readable**  | You can read data from them (e.g., file read stream).                              |
| **Writable**  | You can write data to them (e.g., file write stream).                              |
| **Duplex**    | Can be both readable and writable (e.g., TCP socket).                              |
| **Transform** | Duplex stream that can modify data while reading/writing (e.g., gzip compression). |

---

## ✅ Common Stream Methods

### For Readable Streams:

- `.on('data', callback)` – Triggered when a chunk of data is available.
- `.on('end', callback)` – Triggered when no more data is available.
- `.on('error', callback)` – Triggered when an error occurs.
- `.pipe(destination)` – Sends the readable stream to a writable stream.

### For Writable Streams:

- `.write(chunk)` – Writes a chunk of data.
- `.end()` – Ends the writing process.
- `.on('finish', callback)` – Triggered when all data is written.
- `.on('error', callback)` – Triggered when an error occurs.

---

## ✅ How to Read from a Stream (Example)

```js
const fs = require("fs");

// Reading file as stream
const readStream = fs.createReadStream("example.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Received chunk:", chunk);
});

readStream.on("end", () => {
  console.log("Finished reading.");
});
```

---

## ✅ How to Write to a Stream (Example)

```js
const fs = require("fs");

// Writing to a file
const writeStream = fs.createWriteStream("output.txt");

writeStream.write("Hello, ");
writeStream.write("this is a stream!");
writeStream.end();

writeStream.on("finish", () => {
  console.log("Finished writing.");
});
```

---

## ✅ What is a Buffer?

A **Buffer** is a temporary storage for binary data (like chunks from a stream). Streams use buffers to hold data before it's processed.

### Analogy:

Think of a buffer as a **bucket** that collects water (data chunks) before pouring it somewhere else (writing it or sending it).

### Example:

```js
const buffer = Buffer.from("Hello");
console.log(buffer); // <Buffer 48 65 6c 6c 6f>
```

---

## ✅ What is Piping a Stream?

Piping connects a readable stream to a writable stream so that data automatically flows.

### Example:

```js
const fs = require("fs");

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);
```

### Benefits of Piping:

- Automatically handles **backpressure**.
- Memory-efficient.
- Cleaner and less error-prone.

---

## ✅ What is Chaining in Streams?

Chaining is combining multiple stream operations, typically with transform streams (like compression or encryption).

### Example – Read a file, compress it using gzip, and write it:

```js
const fs = require("fs");
const zlib = require("zlib");

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("input.txt.gz");
const gzip = zlib.createGzip();

readStream.pipe(gzip).pipe(writeStream);
```

---

## 🔗 Visual Representation of Chaining

```
      [ Readable Stream ]
            |
         (chunk)
            ↓
  [ Transform: TO_UPPER_CASE ]
            ↓
      [ Writable Stream ]
            |
         output.txt
```

### Explanation:

1. **Readable Stream**: Reads data in chunks.
2. **Transform Stream**: Modifies the data (e.g., converts to uppercase).
3. **Writable Stream**: Writes the transformed data to a file.

---

## 🧠 Why Use Transform Streams?

- Modify data on the fly (e.g., compression, encryption, data filtering).
- Works well in pipelined architectures.
- Keeps memory usage low and throughput high.

---

## ✅ Manual Stream Handling vs. Piping

### Without Piping (Manual Handling):

```js
const fs = require("fs");

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

// Manually listen for data events and write to output
readStream.on("data", (chunk) => {
  const canWrite = writeStream.write(chunk);
  if (!canWrite) {
    readStream.pause(); // Pause reading if buffer is full
  }
});

// Resume reading when writable stream is drained
writeStream.on("drain", () => {
  readStream.resume();
});

// End the write stream when read is complete
readStream.on("end", () => {
  writeStream.end();
});

// Handle errors
readStream.on("error", (err) => console.error("Read Error:", err));
writeStream.on("error", (err) => console.error("Write Error:", err));
```

### With Piping:

```js
const fs = require("fs");

fs.createReadStream("input.txt").pipe(fs.createWriteStream("output.txt"));
```

### Comparison:

| **Feature**             | **Without Pipe**        | **With Pipe**               |
| ----------------------- | ----------------------- | --------------------------- |
| Reads data chunks       | ✅ Manual               | ✅ Automatic                |
| Writes data chunks      | ✅ Manual               | ✅ Automatic                |
| Handles backpressure    | 🛠 Manual (pause/resume) | ✅ Automatic                |
| Listens for errors      | 🔥 Must add manually    | ⚠️ Optional but recommended |
| Closes streams properly | 🛠 Manual                | ✅ Automatic                |

---

## ✅ Transform Stream Example

Let’s say we want to capitalize all text from `input.txt` before writing to `output.txt`.

### Example:

```js
const { Transform } = require("stream");
const fs = require("fs");

// Custom transform stream to uppercase the data
const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    const upperChunk = chunk.toString().toUpperCase();
    callback(null, upperChunk);
  },
});

fs.createReadStream("input.txt")
  .pipe(upperCaseTransform) // Transform step
  .pipe(fs.createWriteStream("output.txt"));
```

---

## 🔄 Summary (Quick Revision)

| **Concept**  | **Meaning**                                                             |
| ------------ | ----------------------------------------------------------------------- |
| **Stream**   | Way to handle data in chunks.                                           |
| **Types**    | Readable, Writable, Duplex, Transform.                                  |
| **Buffer**   | Temporary memory storage for stream chunks.                             |
| **Pipe**     | Connects a readable stream to a writable stream.                        |
| **Chaining** | Links multiple stream transformations (e.g., read → transform → write). |

---

Let me know if you'd like further examples, diagrams, or coding tasks to practice these concepts!
