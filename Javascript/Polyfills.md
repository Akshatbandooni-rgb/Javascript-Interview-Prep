# Promise Polyfills

---

## ✅ `Promise.all` Polyfill

```javascript
if (!Promise.all) {
  Promise.all = function (promises) {
    return new Promise((resolve, reject) => {
      // Check if the input is actually an array
      if (!Array.isArray(promises)) {
        return reject(new TypeError("Promise.all accepts an array"));
      }

      // Array to store the resolved values in the original order
      const results = [];

      // A counter to track how many promises have successfully resolved
      let completed = 0;

      // If the array is empty, resolve immediately with an empty array
      if (promises.length === 0) {
        return resolve([]);
      }

      // Iterate over each promise (or value) in the input array
      promises.forEach((promise, index) => {
        // Convert the value to a promise (if it's not already one)
        Promise.resolve(promise)
          .then((value) => {
            // Store the resolved value at the corresponding index
            results[index] = value;

            // Increase the counter for successfully resolved promises
            completed++;

            // If all promises have resolved, resolve the parent promise with the results array
            if (completed === promises.length) {
              resolve(results);
            }
          })
          .catch((error) => {
            // If any promise rejects, immediately reject the parent promise
            // `Promise.all` fails fast — it doesn't wait for other promises
            reject(error);
          });
      });
    });
  };
}
```

---

## ✅ `Promise.allSettled` Polyfill (without using `Promise.all`)

```javascript
if (!Promise.allSettled) {
  Promise.allSettled = function (promises) {
    return new Promise((resolve, reject) => {
      // Validate that the input is an array
      if (!Array.isArray(promises)) {
        return reject(new TypeError("Promise.allSettled accepts an array"));
      }

      // Array to hold the final result objects { status, value } or { status, reason }
      const results = [];

      // Counter to track how many promises have finished (settled)
      let completed = 0;

      // If the array is empty, resolve immediately with an empty array
      if (promises.length === 0) {
        return resolve([]);
      }

      // Iterate over the array of promises (or values)
      promises.forEach((promise, index) => {
        // Normalize to a promise, so we can use `.then` and `.catch` reliably
        Promise.resolve(promise)
          .then((value) => {
            // If fulfilled, store the value and a status of 'fulfilled'
            results[index] = {
              status: "fulfilled",
              value: value,
            };
          })
          .catch((reason) => {
            // If rejected, store the reason and a status of 'rejected'
            results[index] = {
              status: "rejected",
              reason: reason,
            };
          })
          .finally(() => {
            // Increment the counter after either resolution or rejection
            completed++;

            // Once all have settled, resolve the main promise with the results array
            if (completed === promises.length) {
              resolve(results);
            }
          });
      });
    });
  };
}
```

---
