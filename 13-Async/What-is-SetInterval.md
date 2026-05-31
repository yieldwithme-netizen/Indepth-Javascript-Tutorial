# What is setInterval

`setInterval` is a browser/Node.js API that repeatedly executes a function at a **fixed time interval** until cleared.

## Definition

```javascript
const intervalId = setInterval(callback, interval);
```

- `callback` — Function to execute repeatedly
- `interval` — Time between executions in milliseconds
- Returns an `intervalId` for cancellation

## Basic Usage

```javascript
let count = 0;

const id = setInterval(() => {
  count++;
  console.log(`Count: ${count}`);
  if (count >= 5) {
    clearInterval(id);
  }
}, 1000);

// Count: 1
// Count: 2
// Count: 3
// Count: 4
// Count: 5
```

## Clearing Intervals

```javascript
const id = setInterval(() => {
  console.log("Running...");
}, 1000);

// Stop after 5 seconds
setTimeout(() => {
  clearInterval(id);
  console.log("Stopped");
}, 5000);
```

## Common Use Cases

### Clock Display
```javascript
function updateClock() {
  const now = new Date();
  document.getElementById("clock").textContent = now.toLocaleTimeString();
}

setInterval(updateClock, 1000);
updateClock(); // Run immediately
```

### Auto-refresh Data
```javascript
setInterval(async () => {
  const data = await fetch("/api/status").then(r => r.json());
  updateDashboard(data);
}, 30000); // Every 30 seconds
```

### Game Loop
```javascript
const gameState = { score: 0, running: true };

setInterval(() => {
  if (!gameState.running) return;
  updateEnemies();
  checkCollisions();
  render();
}, 1000 / 60); // ~60fps
```

### Progress Bar Animation
```javascript
function animateProgress(element, duration) {
  let start = null;
  const interval = 16; // ~60fps

  const id = setInterval(() => {
    start = start || Date.now();
    const progress = (Date.now() - start) / duration;
    element.style.width = `${Math.min(progress * 100, 100)}%`;

    if (progress >= 1) clearInterval(id);
  }, interval);
}
```

## setInterval vs setTimeout

```javascript
// setTimeout - waits, then runs once
setTimeout(() => console.log("once"), 1000);

// setInterval - runs repeatedly
let i = 0;
const id = setInterval(() => {
  console.log("repeating");
  if (++i === 5) clearInterval(id);
}, 1000);
```

### Recursive setTimeout (Preferred)
```javascript
function poll() {
  fetch("/api/data")
    .then(handleData)
    .finally(() => {
      setTimeout(poll, 5000); // Wait 5s between requests
    });
}

poll();
```

Why recursive setTimeout is often better:
- Guarantees delay **between** executions
- Won't queue up if callback is slow
- More predictable timing

## Common Mistakes

- Forgetting to clear intervals (memory leaks)
- Assuming exact timing (intervals drift)
- Not accounting for callback execution time

```javascript
// Drift problem
const start = Date.now();
setInterval(() => {
  console.log(`Expected: ${Date.now() - start}ms`);
  // Actual time drifts due to execution time
}, 1000);
```

## Quick Revision

- `setInterval` repeats code at fixed intervals
- Always clear intervals when done with `clearInterval`
- Prefer recursive `setTimeout` for variable-rate polling
- Interval timing is a minimum, not guaranteed exact
- Great for clocks, auto-refresh, and periodic checks

## Related Topics

- [[What-is-SetTimeout]]
- [[Clear-Timers]]
- [[Promises-and-Async-Await]]
- [[Event-Loop-and-Call-Stack]]
- [[RequestAnimationFrame]]
