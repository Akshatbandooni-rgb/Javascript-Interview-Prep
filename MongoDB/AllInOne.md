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

<!-- New Content goes here -->

# MongoDB All-In-One Guide

---

## CRUD Operations in MongoDB

### 1. Create (Insert Documents)

#### ➤ `insertOne()` – Add a single document

```javascript
db.users.insertOne({
  name: "Akshat",
  age: 29,
  skills: ["Node.js", "MongoDB"],
});
```

#### ➤ `insertMany()` – Add multiple documents

```javascript
db.users.insertMany([
  { name: "Sunil", age: 30 },
  { name: "Vinay", age: 27 },
]);
```

---

### 2. Read (Find Documents)

#### ➤ `find()` – Get all documents

```javascript
db.users.find();
```

#### ➤ `find({ filter })` – Get documents with a condition

```javascript
db.users.find({ age: { $gt: 28 } }); // Users older than 28
```

#### ➤ `findOne()` – Get a single document

```javascript
db.users.findOne({ name: "Akshat" });
```

---

### 3. Update Documents

#### ➤ `updateOne()` – Update the first matching document

```javascript
db.users.updateOne({ name: "Akshat" }, { $set: { age: 30 } });
```

#### ➤ `updateMany()` – Update all matching documents

```javascript
db.users.updateMany({ age: { $lt: 30 } }, { $set: { status: "Junior" } });
```

#### ➤ `replaceOne()` – Replace the entire document

```javascript
db.users.replaceOne(
  { name: "Sunil" },
  { name: "Sunil", age: 31, city: "Delhi" }
);
```

---

### 4. Delete Documents

#### ➤ `deleteOne()` – Delete the first matching document

```javascript
db.users.deleteOne({ name: "Vinay" });
```

#### ➤ `deleteMany()` – Delete all matching documents

```javascript
db.users.deleteMany({ age: { $lt: 28 } });
```

---

### 🎤 How to Say in Interviews:

"In MongoDB, we use methods like `insertOne`, `find`, `updateOne`, and `deleteOne` for CRUD operations. These methods are simple and work directly with JSON-like documents. MongoDB’s flexibility allows quick data manipulation without strict schema constraints, making it ideal for dynamic applications."

---

## Complex CRUD Operations with Nested Documents and Arrays

### 🧾 Sample Collection: `orders`

```json
{
  "_id": ObjectId("..."),
  "customerName": "Akshat Bandooni",
  "status": "processing",
  "address": {
    "street": "123 Dev Street",
    "city": "Bangalore",
    "zip": "560001"
  },
  "items": [
    { "product": "Laptop", "quantity": 1, "price": 75000 },
    { "product": "Mouse", "quantity": 2, "price": 500 }
  ],
  "orderedAt": ISODate("2025-04-20T10:00:00Z")
}
```

---

### 1. Create

#### ➤ Method: `db.collection.insertOne(document)`

```javascript
db.orders.insertOne({
  customerName: "Akshat Bandooni",
  status: "processing",
  address: {
    street: "123 Dev Street",
    city: "Bangalore",
    zip: "560001",
  },
  items: [
    { product: "Laptop", quantity: 1, price: 75000 },
    { product: "Mouse", quantity: 2, price: 500 },
  ],
  orderedAt: new Date(),
});
```

---

### 2. Read

#### ➤ Method: `db.collection.find(query, projection)`

**Get all orders where `items.product = "Laptop"`**

```javascript
db.orders.find({ "items.product": "Laptop" });
```

**Get only `customerName` and `items` (hide `_id`)**

```javascript
db.orders.find({ status: "processing" }, { _id: 0, customerName: 1, items: 1 });
```

**Find orders shipped to Bangalore**

```javascript
db.orders.find({ "address.city": "Bangalore" });
```

---

### 3. Update

#### ➤ Method: `db.collection.updateOne(filter, update, options)`

**a) Update order status**

```javascript
db.orders.updateOne(
  { customerName: "Akshat Bandooni" },
  { $set: { status: "shipped" } }
);
```

**b) Update nested address field (zip)**

```javascript
db.orders.updateOne(
  { "address.city": "Bangalore" },
  { $set: { "address.zip": "560002" } }
);
```

**c) Add a new item to the `items` array**

```javascript
db.orders.updateOne(
  { customerName: "Akshat Bandooni" },
  { $push: { items: { product: "Keyboard", quantity: 1, price: 1500 } } }
);
```

**d) Update quantity of a specific product in the array**

```javascript
db.orders.updateOne(
  {
    customerName: "Akshat Bandooni",
    "items.product": "Mouse",
  },
  {
    $set: { "items.$.quantity": 3 },
  }
);
```

---

### 4. Delete

#### ➤ Method: `db.collection.deleteOne(filter)` / `db.collection.deleteMany(filter)`

**a) Delete order by customer name**

```javascript
db.orders.deleteOne({ customerName: "Akshat Bandooni" });
```

**b) Remove an item from the array (e.g., "Mouse")**

```javascript
db.orders.updateOne(
  { customerName: "Akshat Bandooni" },
  { $pull: { items: { product: "Mouse" } } }
);
```

---

### 🎤 How to Explain in Interviews:

"In a real-world scenario, MongoDB handles deeply nested structures and arrays effortlessly. For instance, in an `orders` collection with embedded documents for `address` and an array of `items`, we can use dot notation to query or update nested fields. This structure makes MongoDB ideal for modeling hierarchical data without complex joins."

---

## Implementing Complex Structures in Mongoose (Node.js)

This section demonstrates schema definition and CRUD operations for documents containing nested documents and arrays.

### 🧾 Step 1: Mongoose Schema Design

```javascript
const mongoose = require("mongoose");

const itemSchema = new mongoose.Schema({
  product: String,
  quantity: Number,
  price: Number,
});

const addressSchema = new mongoose.Schema({
  street: String,
  city: String,
  zip: String,
});

const orderSchema = new mongoose.Schema({
  customerName: { type: String, required: true },
  status: { type: String, default: "processing" },
  address: addressSchema,
  items: [itemSchema],
  orderedAt: { type: Date, default: Date.now },
});

const Order = mongoose.model("Order", orderSchema);
```

---

### 📦 Step 2: CREATE

#### ➤ Method: `Order.create(doc)` or `new Order(doc).save()`

```javascript
const newOrder = new Order({
  customerName: "Akshat Bandooni",
  address: {
    street: "123 Dev Street",
    city: "Bangalore",
    zip: "560001",
  },
  items: [
    { product: "Laptop", quantity: 1, price: 75000 },
    { product: "Mouse", quantity: 2, price: 500 },
  ],
});

await newOrder.save();
```

---

### 🔍 Step 3: READ

#### ➤ Method: `Order.find(query, projection)`

**a) Get all orders with a "Laptop"**

```javascript
const orders = await Order.find({ "items.product": "Laptop" });
```

**b) Get specific fields**

```javascript
const orders = await Order.find({ status: "processing" }, "customerName items");
```

**c) Find by nested city**

```javascript
const orders = await Order.find({ "address.city": "Bangalore" });
```

---

### ✏️ Step 4: UPDATE

#### ➤ Method: `Order.updateOne(filter, update)`

**a) Update order status**

```javascript
await Order.updateOne(
  { customerName: "Akshat Bandooni" },
  { $set: { status: "shipped" } }
);
```

**b) Update zip inside address**

```javascript
await Order.updateOne(
  { "address.city": "Bangalore" },
  { $set: { "address.zip": "560002" } }
);
```

**c) Push a new item**

```javascript
await Order.updateOne(
  { customerName: "Akshat Bandooni" },
  { $push: { items: { product: "Keyboard", quantity: 1, price: 1500 } } }
);
```

**d) Update item quantity**

```javascript
await Order.updateOne(
  {
    customerName: "Akshat Bandooni",
    "items.product": "Mouse",
  },
  {
    $set: { "items.$.quantity": 3 },
  }
);
```

---

### ❌ Step 5: DELETE

#### ➤ Method: `Order.deleteOne(filter)` or `$pull` for array elements

**a) Delete entire order**

```javascript
await Order.deleteOne({ customerName: "Akshat Bandooni" });
```

**b) Remove item from array**

```javascript
await Order.updateOne(
  { customerName: "Akshat Bandooni" },
  { $pull: { items: { product: "Mouse" } } }
);
```

---

### 🎤 How to Explain in Interviews:

"In Mongoose, we define schemas for nested documents and arrays using sub-schemas. CRUD operations like `find`, `updateOne`, and `$push` allow us to manipulate deeply nested structures efficiently. This makes Mongoose ideal for handling hierarchical data in Node.js applications."
