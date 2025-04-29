1. What is XSS (Cross-Site Scripting) attack?
   XSS allows attackers to inject malicious scripts into web pages viewed by users, executing in the victim’s browser, and stealing sensitive data (cookies, sessions).

Example:

html
Copy
Edit

<script>alert('Hacked!');</script>

2. How to prevent XSS attacks? What is sanitize-html module?
   Sanitize Input: Use libraries like sanitize-html to remove malicious content.

Example with sanitize-html:

javascript
Copy
Edit
const sanitizeHtml = require('sanitize-html');
const clean = sanitizeHtml('<script>alert("Hacked!")</script>');
console.log(clean); // Output: ""
Escape Output: Escape data before rendering it in HTML/JS.

CSP: Use Content Security Policy to restrict sources for scripts.

3. What is SQL Injection attack?
   SQL Injection occurs when attackers insert malicious SQL code into a query, gaining unauthorized access or modifying the database.

Example:

sql
Copy
Edit
SELECT \* FROM users WHERE username = 'admin' --' AND password = ''; 4. How do you prevent SQL injection attacks in Node?
Use Prepared Statements: Automatically escape user input.

Example with pg (PostgreSQL):

javascript
Copy
Edit
const query = 'SELECT \* FROM users WHERE username = $1 AND password = $2';
const values = ['admin', 'password'];
const result = await client.query(query, values);
Use ORM: ORMs like Sequelize automatically prevent SQL injection.

5. How can you improve the performance of a Node.js application?
   Use Clustering: Utilize multiple CPU cores.

Example:

javascript
Copy
Edit
const cluster = require('cluster');
if (cluster.isMaster) {
cluster.fork();
} else {
// App code
}
Optimize Database Queries: Use indexing, avoid N+1 queries.

Cache Data: Use Redis to cache frequently accessed data.

Asynchronous Code: Use async/await to avoid blocking operations.

6. How can you Deploy a Node.js application?
   Heroku: Push app with git push heroku master.

Example:

bash
Copy
Edit
heroku create my-node-app
git push heroku master
DigitalOcean: SSH into the server, install dependencies, and use pm2 to run the app.

Example with pm2:

bash
Copy
Edit
pm2 start server.js
Let me know if you need more details!

You said:
How does mongoose prevent SQL

ChatGPT said:
Mongoose is an Object Document Mapper (ODM) for MongoDB, not a SQL-based database, so it doesn’t directly face SQL Injection attacks as it doesn't use SQL. However, Mongoose prevents similar injection-style attacks by handling user input safely and abstracting away raw queries.

Here’s how it helps protect against injection attacks:

1. Built-in Query Sanitization:
   Mongoose automatically escapes input data when performing database queries, preventing malicious data from being executed.

For example, if a user inputs:

javascript
Copy
Edit
let userInput = '$ne": { $gt: "" }';
In a normal query, this might be executed maliciously. But in Mongoose, queries are properly escaped and sanitized, preventing this type of attack.

2. Avoiding Raw Queries:
   Mongoose encourages using model methods like .find(), .create(), etc., which automatically escape and validate inputs, reducing the risk of injection.

Example (safe Mongoose query):

javascript
Copy
Edit
User.findOne({ username: req.body.username }).exec();
This method ensures that the query structure remains safe, as opposed to using raw SQL-like queries.

3. Schema Validation:
   Mongoose supports schema-based validation, which ensures that data conforms to the expected structure before being saved to the database.

Example (schema validation):

javascript
Copy
Edit
const userSchema = new mongoose.Schema({
username: { type: String, required: true, unique: true },
email: { type: String, required: true, match: /.+@.+\..+/ }
});

const User = mongoose.model('User', userSchema);
This ensures that only validated data is stored, preventing malicious or malformed data from entering the database.

4. Query Builders:
   Instead of building raw MongoDB queries, Mongoose encourages the use of query builder methods (e.g., .find(), .update(), .delete()). This reduces the likelihood of constructing unsafe queries.

Example (query builder):

javascript
Copy
Edit
User.find({ username: req.body.username })
.where('age').gt(18)
.exec((err, users) => { /_ handle users _/ });
By abstracting away raw queries, Mongoose minimizes the risk of injection-style attacks common in SQL databases.
