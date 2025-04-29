# Debounce and Throttle in JavaScript

---

## Debounce Function

```javascript
function debounce(func, delay) {
  let timer; // Store the timer ID

  return function (...args) {
    // Clear the previous timer if the function is called again
    clearTimeout(timer);

    // Set a new timer that will call the function after the specified delay
    timer = setTimeout(() => {
      func.apply(this, args); // Call the original function with correct context and arguments
    }, delay);
  };
}
```

### Convert to Debounced Function

To use the debounce function, you simply need to wrap the original function with it. For example:

```javascript
// Original function
function saveInput(value) {
  console.log("Saving data:", value);
}

// Convert to debounced version with 500ms delay
const debouncedSave = debounce(saveInput, 500);

// Usage
debouncedSave("A"); // Function call 1
debouncedSave("AB"); // Function call 2
debouncedSave("ABC"); // Function call 3
// Only the final call 'Saving data: ABC' will be executed after 500ms of no more events
```

---

## Throttle Function

```javascript
function throttle(func, delay) {
  let lastCall = 0; // Timestamp of the last function call

  return function (...args) {
    const now = new Date().getTime(); // Get the current time

    // Check if the delay period has passed since the last function call
    if (now - lastCall >= delay) {
      func.apply(this, args); // Call the original function with correct context and arguments
      lastCall = now; // Update last call time
    }
  };
}
```

### Convert to Throttled Function

To use the throttle function, you need to wrap the original function with it. For example:

```javascript
// Original function
function handleResize() {
  console.log("Window resized at", new Date().toLocaleTimeString());
}

// Convert to throttled version with 2000ms (2 seconds) delay
const throttledResize = throttle(handleResize, 2000);

// Usage
window.addEventListener("resize", throttledResize); // Will call handleResize at most once every 2 seconds
```

---

## Summary of How to Convert:

- **Debounce:** Wrap the original function with `debounce(func, delay)` to create a debounced version. This ensures the function only runs after a period of inactivity.
- **Throttle:** Wrap the original function with `throttle(func, delay)` to create a throttled version. This ensures the function runs at most once every specified delay.
