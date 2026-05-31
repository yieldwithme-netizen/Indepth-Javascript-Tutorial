# How to Clear Timers

JavaScript provides `clearTimeout` and `clearInterval` to cancel pending timers set by `setTimeout` and `setInterval`.

## Clearing setTimeout

```javascript
const timeoutId = setTimeout(() => {
  console.log("This won't run");
}, 5000);

clearTimeout(timeoutId); // Cancelled
```

### Practical Example: Prevent Double Submit
```javascript
let submitTimeout;

form.addEventListener("submit", (e) => {
  e.preventDefault();

  clearTimeout(submitTimeout);
  submitTimeout = setTimeout(() => {
    submitForm();
  }, 300);
});
```

## Clearing setInterval

```javascript
const intervalId = setInterval(() => {
  console.log("Repeating...");
}, 1000);

// Stop after 10 seconds
setTimeout(() => {
  clearInterval(intervalId);
  console.log("Interval stopped");
}, 10000);
```

### Practical Example: Countdown Timer
```javascript
function countdown(seconds, onTick, onDone) {
  let remaining = seconds;
  const id = setInterval(() => {
    remaining--;
    onTick(remaining);
    if (remaining <= 0) {
      clearInterval(id);
      onDone();
    }
  }, 1000);

  // Return cancel function
  return () => clearInterval(id);
}

const cancel = countdown(
  10,
  (n) => console.log(`${n} seconds left`),
  () => console.log("Time's up!")
);

// Cancel early if needed
setTimeout(cancel, 3000);
```

## Auto-clearing Patterns

### Debounce with Cleanup
```javascript
function debounce(fn, delay) {
  let timerId;

  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}

const search = debounce((query) => {
  console.log("Searching:", query);
}, 300);

input.addEventListener("input", (e) => search(e.target.value));
```

### Throttle with Clear
```javascript
function throttle(fn, limit) {
  let inThrottle = false;

  return function (...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}
```

## Clearing All Timers

```javascript
// Store all timer IDs
const timers = {
  ids: [],
  set(fn, delay) {
    const id = setTimeout(fn, delay);
    this.ids.push(id);
    return id;
  },
  clearAll() {
    this.ids.forEach(id => clearTimeout(id));
    this.ids = [];
  }
};

// Use
timers.set(() => console.log("one"), 1000);
timers.set(() => console.log("two"), 2000);

timers.clearAll(); // Cancel all
```

## Common Mistakes

- Storing timer IDs in wrong scope
- Clearing timers that have already executed
- Not clearing intervals on component unmount (React)
- Using wrong clear function (`clearTimeout` for intervals)

```javascript
// Wrong
clearTimeout(intervalId); // Should be clearInterval

// Correct
clearInterval(intervalId);
```

## Cleanup in React

```javascript
useEffect(() => {
  const id = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => clearInterval(id); // Cleanup on unmount
}, []);
```

## Quick Revision

- Use `clearTimeout(id)` to cancel setTimeout
- Use `clearInterval(id)` to cancel setInterval
- Store timer IDs in accessible scope for cleanup
- Always clear timers when components unmount
- Return cleanup functions from useEffect hooks

## Related Topics

- [[What-is-SetTimeout]]
- [[What-is-SetInterval]]
- [[Debounce-and-Throttle]]
- [[React-UseEffect-Cleanup]]
- [[Memory-Management]]
