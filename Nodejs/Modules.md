# 📦 Node.js Modules – Complete Guide

---

## 1. What are modules in Node.js?

**Interview Answer**:  
In Node.js, a module is a reusable block of code encapsulated in its own file. Every `.js` file is treated as a module and has its own scope. Modules help in organizing code and avoiding global scope pollution.

**Add-on if asked more**:  
Node follows the **CommonJS module system** by default, where we use `require()` to load modules and `module.exports` to export them.

---

## 2. What is module scope?

**Interview Answer**:  
Module scope means that all variables and functions declared in a module are private to that module unless explicitly exported. This prevents accidental variable collisions between files.

**Example**:

```js
// user.js
const secret = "hidden"; // private
exports.name = "Akshat"; // public
```

---

## 3. What is the module wrapper function? What are its parameters?

**Interview Answer**:  
Node internally wraps every module inside a function like this:

```js
(function (exports, require, module, __filename, __dirname) {
  // Your module code
});
```

This provides isolation and gives each module access to helper variables.

**Parameters**:

- `exports`: Shortcut to export values.
- `require`: Function to load other modules.
- `module`: Object representing the current module.
- `__filename`: Absolute path of the current file.
- `__dirname`: Directory of the current module.

---

### 🛠 Execution Flow of the Wrapper Function

1. Your file content is wrapped into the function above.
2. It is compiled using `vm.runInThisContext()` (like `eval` but safer).
3. The wrapper is then invoked immediately with the actual arguments.

**Example**:

```js
// hello.js
console.log(__dirname); // injected by wrapper
```

Internally becomes:

```js
(function (exports, require, module, __filename, __dirname) {
  console.log(__dirname);
})({}, require, module, "/full/path/hello.js", "/full/path");
```

---

## 4. What is module caching?

**Interview Answer**:  
When a module is required for the first time, Node executes and caches its result. On subsequent `require()` calls, the cached version is returned instead of re-executing the file. This improves performance and preserves state.

**Pro Tip**:  
Cached modules are stored in `require.cache`.

---

## 5. What is the difference between `module.exports` and `exports`?

**Interview Answer**:  
`exports` is just a reference to `module.exports`. If you assign something new to `exports`, it breaks the reference and won’t be exported correctly.

**Rule of Thumb**:

- Use `module.exports` when exporting a single value like a function or class.
- Use `exports.someFunction` for named exports.

**Wrong**:

```js
exports = () => {}; // ❌
```

**Right**:

```js
module.exports = () => {}; // ✅
```

---

## 6. CommonJS vs ESModules

**Interview Answer**:  
Node.js supports two module systems:

1. **CommonJS (CJS)** – Uses `require()` and `module.exports`. It’s synchronous and default in Node.
2. **ESModules (ESM)** – Uses `import/export` syntax. It’s asynchronous and closer to browser JS.

**Difference**:

| **Feature**   | **CommonJS**             | **ESModules**                                |
| ------------- | ------------------------ | -------------------------------------------- |
| **Syntax**    | `require/module.exports` | `import/export`                              |
| **Loading**   | Synchronous              | Asynchronous                                 |
| **File Type** | `.js` (default)          | `.mjs` or `type: "module"` in `package.json` |

---

## 7. How many ways can you export a module?

**Interview Answer**:  
There are mainly 3 ways:

1. **Export multiple values**:

   ```js
   exports.add = () => {};
   exports.sub = () => {};
   ```

2. **Export an object**:

   ```js
   module.exports = { add, sub };
   ```

3. **Export a single function or class**:
   ```js
   module.exports = function () {};
   ```

---

## 8. What happens if you don’t export anything from a module?

**Interview Answer**:  
The file still runs and executes any code inside it, but `require()` will return an empty object `{}`.

**Use Case**:  
Useful for configuration files or scripts that initialize something like a DB connection.

---

## 9. How to import single and multiple functions from a module?

**Interview Answer**:

1. **To import multiple**:

   ```js
   const { add, sub } = require("./math");
   ```

2. **To import everything**:

   ```js
   const math = require("./math");
   math.add(2, 3);
   ```

3. **To import default export**:
   ```js
   const greet = require("./greet"); // when module.exports is a function
   ```

---

## 10. What are the types of modules in Node.js?

**Interview Answer**:  
There are 4 types:

1. **Core modules** – Built into Node.js (e.g., `fs`, `path`, `http`).
2. **Local modules** – Files you create (e.g., `require('./utils')`).
3. **Third-party modules** – Installed via npm (e.g., `express`, `lodash`).
4. **C++ Addons** – Native modules written in C++ for performance.

---

## 🧠 Bonus Smart Tips (Use These If You're Asked Deeper Questions)

1. **Use `require.resolve()`** to get the full path of a module:

   ```js
   console.log(require.resolve("./someModule"));
   ```

2. **To clear a module from cache**:
   ```js
   delete require.cache[require.resolve("./someModule")];
   ```
