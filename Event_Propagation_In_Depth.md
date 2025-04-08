# What is Event Propagation? (In-Depth)

When a user interacts with a web page — for example, by clicking a button — that interaction (the event) travels through the Document Object Model (DOM). This travel has a specific direction and order, and that whole process is called Event Propagation.

Think of the DOM as a tree of nested boxes:

```
document
 └── html
      └── body
           └── div#container
                └── div#card
                     └── button#submit
```

If the user clicks #submit, the event travels through this tree in three distinct phases:

---

## 📐 The Three Phases of Event Propagation

### 1. 📥 Capturing Phase (a.k.a. Trickling Phase)
**Direction**: From the outermost element (document) down to the target element.

The event starts at window, moves down to document, then through html, body, etc., until it reaches the element you clicked.

In this phase, event listeners can intercept the event before it reaches the target.

✅ **Use case**:  
Intercepting an event early to prevent something happening later (like blocking clicks on a secure section).

### 2. 🎯 Target Phase
**Direction**: The event reaches the element where it occurred.

This is the actual element that the user interacted with (e.g., the button).

Any listeners on this element are triggered regardless of whether they're set to capture or bubble.

✅ **Use case**:  
Direct handling of a user action like submitting a form or clicking a button.

### 3. 📤 Bubbling Phase
**Direction**: From the target element back up through its ancestors to the root (document).

The event "bubbles" back up the DOM tree.

This is the default phase most event listeners use (unless `{ capture: true }` is set).

✅ **Use case**:  
Centralized event handling (event delegation).  
Handling clicks outside a modal by listening on document.

---

## 🔁 Visualizing Event Propagation

If you click a `<button>` inside nested elements:

```html
<div id="grandparent">
  <div id="parent">
    <button id="child">Click me</button>
  </div>
</div>
```

The event flow will be:

Capturing: document → html → body → #grandparent → #parent → #child  
Target: Event triggered on #child  
Bubbling: #child → #parent → #grandparent → body → html → document

---

## 🔎 How to Attach Listeners in Each Phase

### Capturing Phase
```javascript
element.addEventListener('click', handler, true); // true = capture
```

### Bubbling Phase (default)
```javascript
element.addEventListener('click', handler); // or pass false
```

---

## 🧪 Code Example with Logging for All Phases

```html
<div id="outer">
  Outer
  <div id="middle">
    Middle
    <button id="inner">Click</button>
  </div>
</div>

<script>
  document.getElementById('outer').addEventListener('click', () => {
    console.log('Outer - Capturing');
  }, true); // Capture

  document.getElementById('middle').addEventListener('click', () => {
    console.log('Middle - Capturing');
  }, true);

  document.getElementById('inner').addEventListener('click', () => {
    console.log('Inner - Target');
  });

  document.getElementById('middle').addEventListener('click', () => {
    console.log('Middle - Bubbling');
  });

  document.getElementById('outer').addEventListener('click', () => {
    console.log('Outer - Bubbling');
  });
</script>
```

### 🖨 Output when clicking the button:

```
Outer - Capturing
Middle - Capturing
Inner - Target
Middle - Bubbling
Outer - Bubbling
```

---

## 🚨 Important Methods

### event.stopPropagation()
Stops the event from continuing to bubble or capture further.

```javascript
button.addEventListener('click', (e) => {
  e.stopPropagation();
});
```

### event.stopImmediatePropagation()
Stops other listeners on the same element from running.

---

## 🎯 When to Use Each Phase

| Use Case                     | Phase to Use | Why                                            |
|-----------------------------|---------------|------------------------------------------------|
| Global event logging        | Capturing     | Can inspect all clicks before they hit target  |
| Button or direct interaction| Target        | Handle the specific interaction                |
| Delegation (listening on parent)| Bubbling | Efficient handling of many children            |
| Click-outside detection     | Bubbling      | Modal/dropdown logic                           |
| Prevent child triggering parent | stopPropagation() | Fine control                           |

---

## 🧠 Think of it like...

Imagine you drop a pebble into a calm pond:

- **Capturing** is like the ripple approaching the center.
- **Target** is the exact point the pebble hits the water.
- **Bubbling** is the ripple moving back out from that point.
