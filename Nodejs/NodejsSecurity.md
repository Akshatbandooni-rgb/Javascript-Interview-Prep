# Node.js Security Best Practices

---

## 1. What is XSS (Cross-Site Scripting) Attack?

XSS allows attackers to inject malicious scripts into web pages viewed by users. These scripts execute in the victim’s browser and can steal sensitive data (e.g., cookies, sessions).

### Example:

```html
<script>
  alert("Hacked!");
</script>
```

---

## 2. How to Prevent XSS Attacks?

### **Sanitize Input**:

Use libraries like `sanitize-html` to remove malicious content.

#### Example with `sanitize-html`:

```javascript
const sanitizeHtml = require("sanitize-html");
const clean = sanitizeHtml('<script>alert("Hacked!")</script>');
console.log(clean); // Output: ""
```

### **Escape Output**:

Escape data before rendering it in HTML or JavaScript.

### **Content Security Policy (CSP)**:

Use CSP to restrict sources for scripts.

---

## 3. What is SQL Injection Attack?

SQL Injection occurs when attackers insert malicious SQL code into a query, gaining unauthorized access or modifying the database.

### Example:

```sql
SELECT * FROM users WHERE username = 'admin' --' AND password = '';
```

---

## 4. How to Prevent SQL Injection Attacks in Node.js?

### **Use Prepared Statements**:

Prepared statements automatically escape user input.

#### Example with `pg` (PostgreSQL):

```javascript
const query = "SELECT * FROM users WHERE username = $1 AND password = $2";
const values = ["admin", "password"];
const result = await client.query(query, values);
```

### **Use ORM**:

ORMs like Sequelize automatically prevent SQL injection by abstracting raw queries.

---

## 5. How Does Mongoose Prevent Injection Attacks?

Mongoose is an Object Document Mapper (ODM) for MongoDB, not a SQL-based database, so it doesn’t directly face SQL Injection attacks. However, it prevents similar injection-style attacks by handling user input safely and abstracting away raw queries.

### **1. Built-in Query Sanitization**:

Mongoose automatically escapes input data when performing database queries, preventing malicious data from being executed.

#### Example:

```javascript
let userInput = '$ne": { $gt: "" }';
User.findOne({ username: userInput }).exec(); // Properly sanitized
```

### **2. Avoiding Raw Queries**:

Mongoose encourages using model methods like `.find()`, `.create()`, etc., which automatically escape and validate inputs.

#### Example (Safe Query):

```javascript
User.findOne({ username: req.body.username }).exec();
```

### **3. Schema Validation**:

Mongoose supports schema-based validation, ensuring data conforms to the expected structure before being saved to the database.

#### Example (Schema Validation):

```javascript
const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  email: { type: String, required: true, match: /.+@.+\..+/ },
});

const User = mongoose.model("User", userSchema);
```

This ensures that only validated data is stored, preventing malicious or malformed data from entering the database.

### **4. Query Builders**:

Instead of building raw MongoDB queries, Mongoose encourages the use of query builder methods (e.g., `.find()`, `.update()`, `.delete()`). This reduces the likelihood of constructing unsafe queries.

#### Example (Query Builder):

```javascript
User.find({ username: req.body.username })
  .where("age")
  .gt(18)
  .exec((err, users) => {
    // Handle users
  });
```

By abstracting away raw queries, Mongoose minimizes the risk of injection-style attacks common in SQL databases.

---

## 6. How Can You Improve the Performance of a Node.js Application?

### **1. Use Clustering**:

Utilize multiple CPU cores.

#### Example:

```javascript
const cluster = require("cluster");
if (cluster.isMaster) {
  cluster.fork();
} else {
  // App code
}
```

### **2. Optimize Database Queries**:

- Use indexing.
- Avoid N+1 queries.

### **3. Cache Data**:

Use Redis to cache frequently accessed data.

### **4. Asynchronous Code**:

Use `async/await` to avoid blocking operations.

---

## 7. How Can You Deploy a Node.js Application?

### **1. Heroku**:

Push the app using `git push heroku master`.

#### Example:

```bash
heroku create my-node-app
git push heroku master
```

### **2. DigitalOcean**:

SSH into the server, install dependencies, and use `pm2` to run the app.

#### Example with `pm2`:

```bash
pm2 start server.js
```

---

## 8. Helmet: Secure Your Node.js Applications

In Node.js (especially with Express.js), **Helmet** is a middleware used to help secure your applications by setting various HTTP headers.

### 🔒 What is Helmet?

Helmet is a collection of middleware functions that set security-related HTTP response headers.

It helps protect web apps from common vulnerabilities like:

- Cross-Site Scripting (XSS)
- Clickjacking
- MIME-type sniffing
- And other web vulnerabilities

---

### 🛡️ Why Use Helmet?

- Browsers have built-in defenses against certain attacks, but they rely on correct headers.
- Helmet sets these headers automatically so developers don't have to do it manually.
- Helps enforce best practices in HTTP response headers.

---

## 🧨 1. Cross-Site Scripting (XSS)

### What is XSS?

XSS lets attackers inject malicious JavaScript into your website. This can:

- Steal cookies
- Hijack sessions
- Deface your site

### How Helmet Helps:

Helmet sets the **Content-Security-Policy (CSP)** header.

```http
Content-Security-Policy: default-src 'self'
```

✅ This tells the browser:

- "Only load scripts, styles, etc., from the same domain."
- "Ignore any inline `<script>` tags or 3rd-party URLs unless explicitly allowed."

This blocks most injected scripts from running — even if they get into your page.

---

## 🖼️ 2. Clickjacking

### What is Clickjacking?

A malicious page embeds your site in a hidden `<iframe>`, tricking users into clicking buttons they can't see.

### How Helmet Helps:

Helmet sets the **X-Frame-Options** header.

```http
X-Frame-Options: DENY
```

✅ This tells the browser:

- "Don’t allow this site to be displayed in any iframe."

This prevents your site from being embedded into another, which stops clickjacking.

---

## 📦 3. MIME-Type Sniffing

### What is MIME Sniffing?

Browsers try to guess a file's type (like treating `.txt` as JavaScript), which can be exploited.

### How Helmet Helps:

Helmet sets the **X-Content-Type-Options** header.

```http
X-Content-Type-Options: nosniff
```

✅ This tells the browser:

- "Only treat files as their declared Content-Type."

So if a file is `text/plain`, the browser won't try to treat it as JavaScript — preventing unintended execution.

---

## 🔐 4. Force HTTPS (Prevent Downgrade Attacks)

### What are Downgrade Attacks?

If someone can trick a user into using HTTP instead of HTTPS, they can intercept data (man-in-the-middle attack).

### How Helmet Helps:

Helmet sets the **Strict-Transport-Security (HSTS)** header.

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

✅ This tells the browser:

- "Always use HTTPS for this domain for the next year."
- Even if a user types `http://example.com`, the browser upgrades it to `https://example.com`.

---

## Summary of Helmet Features

| **Feature**                   | **Header**                        | **Purpose**                                   |
| ----------------------------- | --------------------------------- | --------------------------------------------- |
| **Content Security Policy**   | `Content-Security-Policy`         | Prevents XSS by restricting script sources.   |
| **X-Frame-Options**           | `X-Frame-Options: DENY`           | Prevents clickjacking by disallowing iframes. |
| **X-Content-Type-Options**    | `X-Content-Type-Options: nosniff` | Prevents MIME-type sniffing.                  |
| **Strict-Transport-Security** | `Strict-Transport-Security`       | Forces HTTPS to prevent downgrade attacks.    |

---

## How to Use Helmet in Your Node.js Application

### Installation:

```bash
npm install helmet
```

### Usage:

```javascript
const express = require("express");
const helmet = require("helmet");

const app = express();

// Use Helmet middleware
app.use(helmet());

app.get("/", (req, res) => {
  res.send("Hello, secure world!");
});

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

---
