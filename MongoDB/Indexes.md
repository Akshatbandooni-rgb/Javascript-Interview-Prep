# MongoDB Indexes

---

## Single Field Index

An index created on a single field to improve query performance.

```javascript
userSchema.index({ username: 1 }); // Ascending order
```

---

## Compound Index

An index created on multiple fields to optimize queries involving multiple fields.

```javascript
userSchema.index({ username: 1, email: -1 });
```

---

## Unique Index

Ensures that the indexed field’s value is unique across all documents in the collection.

```javascript
userSchema.index({ email: 1 }, { unique: true });
```

---

## Text Index

Used for text search on string fields. Allows efficient text-based queries.

```javascript
userSchema.index({ username: "text", email: "text" });
```

---

## Hashed Index

Used for hash-based sharding in MongoDB. It creates a hashed value of the field for indexing.

```javascript
userSchema.index({ username: "hashed" });
```

---

### Summary of Index Types:

1. **Single Field Index**: Optimizes queries on a single field.
2. **Compound Index**: Optimizes queries involving multiple fields.
3. **Unique Index**: Ensures uniqueness of field values.
4. **Text Index**: Enables efficient text search.
5. **Hashed Index**: Used for hash-based sharding.
