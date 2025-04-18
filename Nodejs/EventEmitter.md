# Node.js Event Module & EventEmitter - Complete Guide

## 1. What is the Event Module in Node.js?

The **Event Module** in Node.js is a built-in module that allows you to work with events and listeners. It provides an API to handle asynchronous events.  
It’s primarily based on the **Observer (Pub-Sub) pattern**, where events are emitted, and listeners handle those events.

### Why is it used?

- **Asynchronous communication** between different parts of an application.
- **Decouples** logic — so that your application components can operate independently.
- Helps in **non-blocking event-driven architecture**, which is ideal for I/O-bound operations.

---

## 2. What is `EventEmitter`?

`EventEmitter` is the core class of the **Event module**. It's used to handle events by emitting and listening to those events.

### Key Features:

- **Emit events** using `.emit()`.
- **Register listeners** using `.on()`, `.once()`.
- **Remove listeners** using `.off()` or `.removeListener()`.

---

## 3. Methods Provided by `EventEmitter` and Their Use

Here are the commonly used methods with examples:

### 1. `.on(eventName, listener)`

- **Purpose**: Adds a listener to an event.
- **Use Case**: Responds to events when they are emitted.

```js
const EventEmitter = require("events");
const emitter = new EventEmitter();

emitter.on("greet", () => {
  console.log("Hello there!");
});

emitter.emit("greet"); // Output: Hello there!
```

### 2. `.emit(eventName, [...args])`

- **Purpose**: Triggers the event and calls all listeners attached to it.
- **Use Case**: You emit events to notify other parts of your app.

```js
emitter.on("message", (msg) => {
  console.log("Received:", msg);
});

emitter.emit("message", "Hello!"); // Output: Received: Hello!
```

### 3. `.once(eventName, listener)`

- **Purpose**: Adds a listener that is invoked only the first time the event is emitted.
- **Use Case**: For one-time event handling.

```js
emitter.once("greet", () => {
  console.log("First greeting!");
});

emitter.emit("greet"); // Output: First greeting!
emitter.emit("greet"); // No output since `.once` is triggered only once
```

### 4. `.removeListener(eventName, listener)` or `.off(eventName, listener)`

- **Purpose**: Removes a specific listener from an event.
- **Use Case**: If you want to unsubscribe a listener.

```js
const listener = () => {
  console.log("This will be removed.");
};

emitter.on("greet", listener);
emitter.emit("greet"); // Output: This will be removed.

emitter.removeListener("greet", listener);
emitter.emit("greet"); // No output as listener is removed.
```

### 5. `.listenerCount(eventName)`

- **Purpose**: Returns the number of listeners for a specific event.
- **Use Case**: Check how many listeners are registered for a particular event.

```js
console.log(emitter.listenerCount("greet")); // Output: 1 (if we have listeners for 'greet')
```

### 6. `.prependListener(eventName, listener)`

- **Purpose**: Adds a listener to the beginning of the listeners list for a given event.
- **Use Case**: Use it if you want the listener to be called before any other listeners.

```js
emitter.prependListener("greet", () => {
  console.log("Prepend listener");
});

emitter.emit("greet"); // Output: Prepend listener, Hello there!
```

---

## 4. How Does a Class Extend EventEmitter?

You can extend `EventEmitter` to create your own custom classes that can emit and listen to events.

### Example:

```js
const EventEmitter = require("events");

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

// Adding a listener
myEmitter.on("event", () => {
  console.log("Event has been emitted!");
});

// Emitting an event
myEmitter.emit("event"); // Output: Event has been emitted!
```

In this example:

- `MyEmitter` is a custom class that extends `EventEmitter`.
- It inherits the `.on()` and `.emit()` methods, allowing you to listen for and emit events.
- You can now use this custom class to emit and handle events just like the built-in `EventEmitter`.

---

## 5. Real-World Use Case for EventEmitter

### Use Case: User Registration with Email Notification

Imagine you're building a user registration feature for a web app. When a user registers, you want to:

1. Save the user to the database.
2. Send a welcome email.
3. Log the event for analytics.

You don’t want these tasks to block the registration process, so you use `EventEmitter` to decouple them and handle them asynchronously.

### Example:

#### `eventBus.js`

```js
const EventEmitter = require("events");
module.exports = new EventEmitter();
```

#### `userService.js` (Controller)

```js
const eventBus = require("./eventBus");

async function registerUser(req, res) {
  const { name, email, password } = req.body;

  const user = await User.create({ name, email, password });

  // Emit the event for user registration
  eventBus.emit("userRegistered", user);

  res.status(201).json({ message: "User registered successfully" });
}

module.exports = { registerUser };
```

#### `mailService.js` (Listener)

```js
const eventBus = require("./eventBus");

eventBus.on("userRegistered", async (user) => {
  await sendWelcomeEmail(user.email);
  console.log("Welcome email sent to:", user.email);
});
```

#### `loggerService.js` (Listener)

```js
const eventBus = require("./eventBus");

eventBus.on("userRegistered", (user) => {
  console.log(`User ${user.name} registered at ${new Date()}`);
});
```

### Flow:

1. When the user registers, the `registerUser` method emits a `userRegistered` event.
2. The `mailService` and `loggerService` listen to that event and act on it asynchronously (send email, log registration).

This way:

- Your API doesn’t get blocked.
- Logic is decoupled and modular.
- You can easily add new listeners (like SMS, analytics) later.

---

## Summary: Key Points to Remember

- **EventEmitter**: A Node.js class for managing events and listeners.
- **Methods**: `.on()`, `.emit()`, `.once()`, `.removeListener()`, `.listenerCount()`, etc.
- **Extending EventEmitter**: You can create custom classes that emit and listen to events.
- **Real-world Use Case**: Use it to decouple asynchronous tasks like emails and logging, which enhances maintainability and performance.
