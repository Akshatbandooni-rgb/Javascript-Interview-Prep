```javascript
function createCalculator() {
  let count = 0; // private variable

  return {
    increment() {
      count++;
      console.log(`Count: ${count}`);
    },
    decrement() {
      count--;
      console.log(`Count: ${count}`);
    },
    reset() {
      count = 0;
      console.log(`Reset! Count: ${count}`);
    },
    getCount() {
      return count;
    }
  };
}

// Usage:
const counter = createCalculator();

counter.increment();  // Count: 1
counter.increment();  // Count: 2
counter.decrement();  // Count: 1
console.log(counter.getCount());  // 1
counter.reset();  // Reset! Count: 0
