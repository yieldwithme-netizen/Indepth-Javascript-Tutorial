# Debounce and Throttle

## Definition

**Debounce** and **throttle** are techniques to control how often a function executes. They improve performance and prevent function call flooding in scenarios like window resizing, scrolling, and user input.

- **Debounce**: Delays execution until a pause in events (wait for user to stop)
- **Throttle**: Limits execution to once per interval (execute at regular intervals)

## Debounce

### How It Works

```
Events:    |--o--o--o--o-----o--o--o--o--o--|
Debounced: |__________o_____|_____________o__|
                        ^                 ^
                     (pause)           (pause)
```

Only fires after a specified delay of inactivity.

### Implementation

```javascript
function debounce(func, delay) {
  let timeoutId;

  return function (...args) {
    // Clear previous timeout
    clearTimeout(timeoutId);

    // Set new timeout
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}
```

### Usage Example

```javascript
// Search input - wait until user stops typing
const searchInput = document.getElementById("search");

function performSearch(query) {
  console.log("Searching for:", query);
  // API call here
}

const debouncedSearch = debounce(performSearch, 500);

searchInput.addEventListener("input", (e) => {
  debouncedSearch(e.target.value);
});
```

### Advanced Debounce

```javascript
function debounce(func, delay, immediate = false) {
  let timeoutId;

  return function (...args) {
    const callNow = immediate && !timeoutId;

    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      timeoutId = null;
      if (!immediate) {
        func.apply(this, args);
      }
    }, delay);

    if (callNow) {
      func.apply(this, args);
    }
  };
}

// Execute immediately on first call, then debounce
const debouncedFn = debounce(fn, 300, true);
```

## Throttle

### How It Works

```
Events:    |--o--o--o--o-----o--o--o--o--o--|
Throttled: |--o-----o-----|-----o-----o-----|
          ^              ^                  ^
       (exec)        (exec)             (exec)
```

Guarantees execution at regular intervals.

### Implementation

```javascript
function throttle(func, limit) {
  let inThrottle = false;

  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;

      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}
```

### Alternative (Leading Edge)

```javascript
function throttle(func, limit) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= limit) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}
```

### Usage Example

```javascript
// Scroll events - limit how often it fires
function handleScroll() {
  console.log("Scroll position:", window.scrollY);
}

const throttledScroll = throttle(handleScroll, 100);

window.addEventListener("scroll", throttledScroll);

// Resize events
function handleResize() {
  console.log("Window size:", window.innerWidth);
}

const throttledResize = throttle(handleResize, 200);
window.addEventListener("resize", throttledResize);
```

## Debounce vs Throttle

| Feature | Debounce | Throttle |
|---------|----------|----------|
| **Trigger** | After inactivity | At regular intervals |
| **Use case** | Search, form validation | Scroll, resize, drag |
| **Response time** | Delayed until pause | Immediate or periodic |
| **Execution count** | 1 per pause | Multiple per event |
| **Best for** | Expensive operations | Continuous events |

## Real-World Examples

### Debounce Examples

```javascript
// 1. Auto-save form
const autoSave = debounce((formData) => {
  fetch("/api/save", {
    method: "POST",
    body: JSON.stringify(formData),
  });
}, 1000);

form.addEventListener("input", (e) => {
  autoSave(getFormData());
});

// 2. Window resize (final size)
const handleResizeEnd = debounce(() => {
  recalculateLayout();
}, 300);

// 3. Button click prevention
const handleSubmit = debounce((data) => {
  submitOrder(data);
}, 500, true); // Immediate on first click
```

### Throttle Examples

```javascript
// 1. Scroll animations
const handleScroll = throttle(() => {
  const scrolled = window.scrollY;
  updateParallax(scrolled);
  checkVisibility(scrolled);
}, 16); // ~60fps

// 2. Mouse move tracking
const trackMouse = throttle((e) => {
  sendAnalytics("mouse_move", { x: e.clientX, y: e.clientY });
}, 100);

// 3. API rate limiting
const apiCall = throttle(async (endpoint) => {
  const response = await fetch(endpoint);
  return response.json();
}, 1000);
```

## Lodash Implementation

```javascript
import { debounce, throttle } from "lodash";

// Debounce
const debouncedSearch = debounce((query) => {
  searchAPI(query);
}, 300);

// Throttle
const throttledScroll = throttle(() => {
  updateUI();
}, 100);

// Cancel functions
debouncedSearch.cancel();
throttledScroll.cancel();
```

## Custom Hooks (React)

```javascript
// useDebounce hook
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(handler);
  }, [value, delay]);

  return debouncedValue;
}

// Usage
function SearchComponent() {
  const [query, setQuery] = useState("");
  const debouncedQuery = useDebounce(query, 300);

  useEffect(() => {
    if (debouncedQuery) {
      fetchResults(debouncedQuery);
    }
  }, [debouncedQuery]);

  return <input onChange={(e) => setQuery(e.target.value)} />;
}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Debouncing scroll events | Use throttle for scroll |
| Not canceling on unmount | Return cleanup function in React |
| Creating new debounce per render | Memoize or use ref |
| Using too short delay | Adjust based on use case |

## Quick Revision

- **Debounce**: Execute after a pause in events (wait for stop)
- **Throttle**: Execute at regular intervals (limit frequency)
- **Debounce** for: search, form validation, resize-end
- **Throttle** for: scroll, mousemove, continuous events
- Always clean up timers in React (useEffect return)
- Consider Lodash for production-ready implementations

## Related Topics

- [[Event-Handling]]
- [[Performance-Optimization]]
- [[Closures]]
- [[setTimeout]]
- [[RequestAnimationFrame]]
- [[React-Hooks]]
