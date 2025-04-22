# 📚 MongoDB Interview Notes

---

## How to Create and Drop a Database & Collection in MongoDB

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

### 💬 How to Say in an Interview:

"In the Mongo shell, a database is created when you switch to it using `use dbName` and insert data. Collections are created with `db.createCollection()` or implicitly on the first insert. You can drop a collection using `db.collection.drop()` and drop a database with `db.dropDatabase()`."

"In Node.js, I use Mongoose to define schemas and create collections automatically. Collections can be dropped using `dropCollection()` and entire databases with `dropDatabase()` on the connection."

---
