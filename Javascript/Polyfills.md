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

## Polyfill for `call`

```javascript
Function.prototype.myCall = function (context, ...args) {
  context = context || window; // if context is null/undefined, set to window (global object)

  const fnSymbol = Symbol(); // create a unique key to avoid overwriting existing properties
  context[fnSymbol] = this; // this -> the function we want to call

  const result = context[fnSymbol](...args); // call the function with given arguments
  delete context[fnSymbol]; // clean up by removing temporary property

  return result; // return whatever the function returns
};
```

### 🔥 How it works:

1. `this` refers to the function we are trying to call (e.g., `greet`).
2. Temporarily assign the function to the given `context` object.
3. Call it as a method of `context`, so `this` inside the function now refers to `context`.
4. After calling, remove the temporary property.

---

## Polyfill for `apply`

```javascript
Function.prototype.myApply = function (context, args) {
  context = context || window;
  const fnSymbol = Symbol();
  context[fnSymbol] = this;

  const result = args ? context[fnSymbol](...args) : context[fnSymbol](); // If args provided, spread them
  delete context[fnSymbol];

  return result;
};
```

### 🔥 How it works:

- Same logic as `call`, with one difference:
  - `call` takes arguments individually (e.g., `arg1, arg2`).
  - `apply` takes an array of arguments (e.g., `[arg1, arg2]`).
- Spread the `args` array using `(...args)`.

---

## Polyfill for `bind`

```javascript
Function.prototype.myBind = function (context, ...args) {
  const func = this; // store reference to original function

  return function (...newArgs) {
    // return a NEW function
    return func.apply(context, [...args, ...newArgs]); // call original function with all arguments
  };
};
```

### 🔥 How it works:

1. `bind` does not call the function immediately.
2. It returns a new function that remembers the `this` and any arguments you provided.
3. When the returned function is called later, it merges arguments (those from `bind` + those from call-time) and calls the original function.

---

## Polyfill for `Map`

```javascript
// Adding a custom myMap method to Array's prototype
Array.prototype.myMap = function (callback) {
  let result = []; // Create an empty array to store transformed elements

  // 'this' refers to the array on which myMap is called
  for (let i = 0; i < this.length; i++) {
    // Call the callback function
    // - Pass the current element (this[i])
    // - Pass the current index (i)
    // - Pass the entire array (this)
    //
    // The callback returns the new value, which we push into result
    result.push(callback(this[i], i, this));
  }

  // After processing all elements, return the new array
  return result;
};
```

---

## Polyfill for `Filter`

```javascript
// Adding a custom myFilter method to Array's prototype
Array.prototype.myFilter = function (callback) {
  let result = []; // Create an empty array to store elements that pass the test

  // 'this' refers to the array on which myFilter is called
  for (let i = 0; i < this.length; i++) {
    // Call the callback function
    // - Pass the current element (this[i])
    // - Pass the current index (i)
    // - Pass the entire array (this)
    //
    // If callback returns true, it means keep the element
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }

  // After checking all elements, return the filtered array
  return result;
};
```

---

### 🔥 How `myMap` Works:

1. `this` refers to the array on which `myMap` is called.
2. Iterates over each element of the array.
3. Calls the provided `callback` function with:
   - The current element.
   - The current index.
   - The entire array.
4. Pushes the transformed value (returned by the `callback`) into a new array.
5. Returns the new array after processing all elements.

---

### 🔥 How `myFilter` Works:

1. `this` refers to the array on which `myFilter` is called.
2. Iterates over each element of the array.
3. Calls the provided `callback` function with:
   - The current element.
   - The current index.
   - The entire array.
4. If the `callback` returns `true`, the current element is added to the result array.
5. Returns the filtered array after processing all elements.
