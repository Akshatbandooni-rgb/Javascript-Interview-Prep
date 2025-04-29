# MongoDB Aggregation Query with Useful Operators

---

### Example Aggregation Pipeline

```javascript
Order.aggregate([
  // 1. $match: Filters documents (like WHERE in SQL)
  {
    $match: {
      status: "completed", // Filter orders where the status is "completed"
    },
  },

  // 2. $group: Groups documents and performs operations (like GROUP BY)
  {
    $group: {
      _id: "$productId", // Group by productId
      totalSales: { $sum: "$amount" }, // Calculate total sales for each product
      productName: { $first: "$productName" }, // Take first product name (assuming all orders have the same product name)
    },
  },

  // 3. $project: Reshapes documents, includes/excludes fields
  {
    $project: {
      _id: 0, // Exclude the default _id field
      productName: 1, // Include product name
      totalSales: 1, // Include total sales
    },
  },

  // 4. $sort: Sorts documents
  {
    $sort: {
      totalSales: -1, // Sort by totalSales in descending order
    },
  },

  // 5. $limit: Limits the number of documents
  {
    $limit: 5, // Limit to top 5 products
  },

  // 6. $skip: Skips a number of documents (useful for pagination)
  {
    $skip: 5, // Skip the first 5 documents (for pagination)
  },

  // 7. $lookup: Joins another collection (like SQL JOIN)
  {
    $lookup: {
      from: "users", // Join with 'users' collection
      localField: "userId", // Match order's userId
      foreignField: "_id", // Match it with users._id
      as: "userDetails", // Store result in 'userDetails'
    },
  },

  // 8. $unwind: Deconstructs an array field from the input documents to output a document for each element
  {
    $unwind: {
      path: "$userDetails", // Unwind userDetails array
      preserveNullAndEmptyArrays: true, // Keep orders without matching users
    },
  },
])
  .then((result) => {
    console.log(result); // Final output after all stages
  })
  .catch((err) => {
    console.error(err);
  });
```

---

### Explanation of Stages

1. **`$match`**: Filters documents based on a condition (similar to `WHERE` in SQL).
2. **`$group`**: Groups documents by a field and performs aggregate calculations (like `GROUP BY` in SQL).
3. **`$project`**: Reshapes documents by including or excluding specific fields.
4. **`$sort`**: Sorts documents based on a field (ascending or descending).
5. **`$limit`**: Limits the number of documents returned (useful for top results).
6. **`$skip`**: Skips a specified number of documents (useful for pagination).
7. **`$lookup`**: Performs a join with another collection (similar to `JOIN` in SQL).
8. **`$unwind`**: Deconstructs an array field into separate documents for each element.

---
