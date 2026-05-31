# What is Debounce

Debounce is a technique that delays function execution until a specified period of inactivity has passed. If the function is called again within the delay period, the timer resets and the previous call is cancelled. This prevents unnecessary function executions during rapid repeated calls.

## How Debounce Works

```javascript
// Without debounce: function called on every event
function search(query) {
  console.log('Searching for:', query);
}

input.addEventListener('input', (e) => {
  search(e.target.value); // Called on every keystroke
});

// With debounce: function called only after pause
const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 300);

input.addEventListener('input', (e) => {
  debouncedSearch(e.target.value); // Only called after 300ms of no typing
});
```

## Basic Debounce Implementation

```javascript
function debounce(func, delay) {
  let timeoutId;

  return function (...args) {
    // Clear previous timer
    clearTimeout(timeoutId);

    // Set new timer
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Usage
const handleResize = debounce((event) => {
  console.log('Window resized');
}, 250);

window.addEventListener('resize', handleResize);
```

## Debounce with Leading and Trailing Options

```javascript
function debounce(func, delay, options = {}) {
  let timeoutId;
  const { leading = false, trailing = true } = options;
  let lastArgs = null;

  return function (...args) {
    const callNow = leading && !timeoutId;

    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      timeoutId = null;
      if (trailing && lastArgs) {
        func.apply(this, lastArgs);
        lastArgs = null;
      }
    }, delay);

    if (callNow) {
      func.apply(this, args);
    } else {
      lastArgs = args;
    }
  };
}

// Leading edge: function called immediately on first call
const leadingDebounce = debounce(fn, 300, { leading: true, trailing: false });

// Both edges: function called immediately and after delay
const bothDebounce = debounce(fn, 300, { leading: true, trailing: true });
```

## Debounce with Cancel and Flush

```javascript
function debounce(func, delay) {
  let timeoutId;
  let lastArgs = null;
  let lastThis = null;

  function debounced(...args) {
    lastArgs = args;
    lastThis = this;

    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func.apply(lastThis, lastArgs);
      lastArgs = null;
      lastThis = null;
    }, delay);
  }

  // Cancel pending execution
  debounced.cancel = function () {
    clearTimeout(timeoutId);
    timeoutId = null;
    lastArgs = null;
    lastThis = null;
  };

  // Execute immediately if pending
  debounced.flush = function () {
    if (timeoutId && lastArgs) {
      func.apply(lastThis, lastArgs);
      debounced.cancel();
    }
  };

  return debounced;
}

// Usage
const debouncedFn = debounce(console.log, 1000);

debouncedFn('a'); // Will execute after 1000ms
debouncedFn.cancel(); // Cancels the pending execution
```

## Search Input Example

```javascript
// Search functionality with debounce
class SearchComponent {
  constructor() {
    this.input = document.getElementById('search-input');
    this.results = document.getElementById('results');
    this.abortController = null;

    this.debouncedSearch = debounce(this.search.bind(this), 300);
    this.input.addEventListener('input', (e) => {
      this.debouncedSearch(e.target.value);
    });
  }

  async search(query) {
    // Cancel previous request
    if (this.abortController) {
      this.abortController.abort();
    }

    if (!query) {
      this.results.innerHTML = '';
      return;
    }

    this.abortController = new AbortController();

    try {
      const response = await fetch(`/api/search?q=${query}`, {
        signal: this.abortController.signal
      });
      const results = await response.json();
      this.displayResults(results);
    } catch (error) {
      if (error.name !== 'AbortError') {
        console.error('Search failed:', error);
      }
    }
  }

  displayResults(results) {
    this.results.innerHTML = results
      .map(item => `<div class="result">${item.title}</div>`)
      .join('');
  }
}
```

## React Debounce Hook

```javascript
import { useState, useEffect, useRef } from 'react';

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

// Usage in component
function SearchInput() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 300);

  useEffect(() => {
    if (debouncedSearchTerm) {
      performSearch(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);

  return (
    <input
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

## Common Use Cases

- **Search inputs**: Wait until user stops typing
- **Window resize**: Recalculate layout after resize ends
- **Button clicks**: Prevent double-clicking submissions
- **Scroll events**: Process scroll after user stops scrolling
- **Form validation**: Validate after user pauses typing
- **Auto-save**: Save draft after editing pauses

## Common Mistakes

1. **Using debounce on every event without need** - Some events need immediate response
2. **Wrong delay value** - Too short doesn't help, too long feels sluggish
3. **Not handling cancel** - Leaving pending executions
4. **Using debounce instead of throttle** - Different use cases
5. **Forgetting to bind context** - Using wrong `this` value

## Related Topics

- [[What-is-Throttle]]
- [[What-is-MemoryLeak]]
- [[Prevent-MemoryLeaks]]
- [[Implement-Auth]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Debounce | Delays execution until inactivity period |
| Leading Edge | Execute immediately on first call |
| Trailing Edge | Execute after delay period |
| Cancel | Stop pending execution |
| Flush | Execute immediately if pending |
| Delay | Time to wait for inactivity |

**Debounce vs Throttle:**
- Debounce: Call after user stops (search input)
- Throttle: Call at regular intervals (scroll position)
