# MongoDB All-In-One Guide

---

## ❓ What is MongoDB?

**Explanation:**  
MongoDB is a **NoSQL**, document-oriented database. It stores data in flexible, JSON-like documents called **BSON (Binary JSON)**. Each document can have a different structure, and there's no need for a fixed schema, unlike relational databases.

**Example:**

```json
{
  "name": "Akshat",
  "age": 29,
  "skills": ["Node.js", "Angular", "MongoDB"]
}
```

### 💬 How to Say in an Interview:

"MongoDB is a NoSQL database that stores data in flexible, schema-less documents. It's particularly useful when working with hierarchical or semi-structured data, and it's designed for high availability and scalability."

---

## ❓ What is NoSQL?

**Explanation:**  
NoSQL stands for "Not Only SQL". It refers to a category of databases that don't use the traditional table-based relational model. Instead, they use:

- **Document-based** (e.g., MongoDB)
- **Key-Value stores** (e.g., Redis)
- **Wide-Column stores** (e.g., Cassandra)
- **Graph databases** (e.g., Neo4j)

### 💬 How to Say in an Interview:

"NoSQL databases are designed for modern web applications that need speed, scalability, and flexibility. They don’t enforce rigid schemas and support horizontal scaling easily."

---

## ❓ Difference Between NoSQL and RDBMS

| **Feature**      | **RDBMS**                  | **NoSQL**                     |
| ---------------- | -------------------------- | ----------------------------- |
| **Data Model**   | Tables with rows & columns | Documents / Key-Value / Graph |
| **Schema**       | Fixed                      | Dynamic / Flexible            |
| **Joins**        | Supported                  | Not supported (denormalized)  |
| **Scalability**  | Vertical                   | Horizontal                    |
| **Transactions** | Strong ACID                | BASE (Eventually Consistent)  |
| **Example**      | MySQL, PostgreSQL          | MongoDB, Cassandra            |

### 💬 How to Say in an Interview:

"RDBMS is structured, relational, and schema-based, whereas NoSQL is unstructured and schema-less, ideal for modern, scalable apps. RDBMS is better for complex relationships, and NoSQL is better for fast, flexible data access."

---

## ❓ When to Use RDBMS vs NoSQL

### Use RDBMS when:

- Data is highly structured with complex relationships (e.g., ERP, Banking)
- You need multi-table joins and strong ACID compliance

### Use NoSQL when:

- You have unstructured/semi-structured data (e.g., user profiles, logs)
- You expect high volumes of read/write operations (e.g., social media apps)
- You need horizontal scalability (e.g., microservices, IoT)

**Example:**  
For an e-commerce app:

- Use RDBMS for payment and order transactions
- Use NoSQL for product catalogs, customer reviews, and activity logs

---

## ❓ What are Documents and Collections in MongoDB?

- **Document:** A JSON-like structure that holds data. Think of it as a record.
- **Collection:** A group of related documents (like a table in RDBMS).

**Example:**

```json
{
  "name": "Laptop",
  "brand": "Dell",
  "price": 75000
}
```

### 💬 How to Say in an Interview:

"In MongoDB, a document is a unit of data stored in a flexible JSON-like format. A collection is a group of such documents, similar to tables but without fixed schemas."

---

## ❓ Advantages of NoSQL over RDBMS

- **Flexible Schema** – Can evolve with the application without altering tables
- **Horizontal Scaling** – Easier to distribute across multiple servers
- **High Performance** – Optimized for large-scale read/write operations
- **Rapid Development** – JSON-like format maps easily to modern web APIs
- **Big Data Handling** – Efficient for handling large volumes and variety of data

**Example:**  
Social media apps can store different types of user content (text, images, videos) in MongoDB without needing schema changes.

---

## ❓ Disadvantages of NoSQL vs RDBMS

- **Lack of Standardization** – No common query language like SQL
- **Limited Joins** – Not designed for complex relationships
- **Eventual Consistency** – Not always immediately consistent like ACID
- **Tooling** – Fewer mature tools for backup, analytics
- **Transaction Support** – While improving, still not as strong as RDBMS

**Example:**  
For banking systems, consistency and transaction reliability are crucial — RDBMS like PostgreSQL is a better choice.

---

## ❓ What is Flexible Schema?

**Explanation:**  
A flexible schema means you can store documents with different fields and data types in the same collection — no need to define a structure upfront.

**MongoDB Example:**

```json
// Document 1
{ "name": "Akshat", "email": "akshat@example.com" }

// Document 2
{ "name": "Sunil", "age": 30, "skills": ["Node.js", "Angular"] }
```

**RDBMS Comparison:**  
In MySQL, you'd need to define all fields in advance, and every row must follow that structure. Adding a new field requires altering the entire table.

### 💬 How to Say:

"Flexible schema means MongoDB allows each document to have its own structure. This is ideal for evolving apps and unstructured data. In contrast, RDBMS enforces strict schemas which require migrations to change."

---

## ❓ What is BSON?

**Explanation:**  
BSON stands for Binary JSON – it's the format MongoDB uses internally to store data.

### Key Features:

- **Binary encoded** – faster for machines to parse
- **Supports more data types than JSON:**
  - Date, int32, int64, Decimal128, ObjectId, binary
- **Enables efficient querying and indexing**

**Example:**  
JSON input:

```json
{
  "name": "Akshat",
  "age": 29,
  "joined": "2023-01-01T00:00:00Z"
}
```

Internally stored as BSON (binary format, not human-readable).

### 💬 How to Say in an Interview:

"BSON is MongoDB’s binary format for storing documents. It supports more data types than JSON and enables faster parsing and querying. While developers use JSON to interact with MongoDB, the database stores it as BSON under the hood for performance and efficiency."

---

<!-- <Data to be formatted> -->

## Creating and Dropping a Database & Collection in MongoDB

### ✅ Using MongoDB Shell

#### ➕ Create a Database

You switch to a database, and MongoDB will create it when you insert data:

```javascript
use myDatabase
```

This doesn’t create it immediately — it's created when a collection with at least one document is added.

#### ➕ Create a Collection

```javascript
db.createCollection("users");
```

Or create it implicitly:

```javascript
db.users.insertOne({ name: "Akshat", age: 29 });
```

#### ❌ Drop a Collection

```javascript
db.users.drop();
```

#### ❌ Drop a Database

```javascript
use myDatabase
db.dropDatabase()
```

---

### ✅ Using Node.js with Mongoose

#### ➕ Connect and Create Collection (via Schema)

```javascript
const mongoose = require("mongoose");

mongoose.connect("mongodb://localhost:27017/myDatabase");

const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
});

const User = mongoose.model("User", userSchema);

// Create a document (this also creates the collection)
User.create({ name: "Akshat", age: 29 });
```

Mongoose automatically creates the collection with a pluralized name (e.g., `users` from `User`).

#### ❌ Drop a Collection (Mongoose)

```javascript
await mongoose.connection.dropCollection("users");
```

#### ❌ Drop a Database (Mongoose)

```javascript
await mongoose.connection.dropDatabase();
```

---

Let me know if you'd like further refinements or additional sections!
