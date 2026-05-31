# What is Memory Leak

A memory leak occurs when a program allocates memory but fails to release it back to the system after it's no longer needed. Over time, this causes the application to consume increasing amounts of memory, leading to performance degradation and eventual crashes.

## How Memory Management Works

```javascript
// Memory is allocated when variables are created
function example() {
  const largeArray = new Array(1000000).fill('data'); // Memory allocated
  // Memory should be freed when function exits
}

// Garbage collector automatically frees unused memory
function withGarbageCollection() {
  let obj = { data: 'value' };
  obj = null; // Now eligible for garbage collection
}
```

## Common Causes of Memory Leaks

### 1. Forgotten Event Listeners

```javascript
class Component {
  constructor() {
    this.handleClick = this.handleClick.bind(this);
    window.addEventListener('click', this.handleClick);
  }

  handleClick() {
    console.log('Clicked');
  }

  // Memory leak: never removes event listener
  destroy() {
    // Missing: window.removeEventListener('click', this.handleClick);
  }
}

// Fixed version
class ComponentFixed {
  constructor() {
    this.handleClick = this.handleClick.bind(this);
    window.addEventListener('click', this.handleClick);
  }

  handleClick() {
    console.log('Clicked');
  }

  destroy() {
    window.removeEventListener('click', this.handleClick);
  }
}
```

### 2. Uncleared Timers

```javascript
class PollingService {
  start() {
    // Memory leak if not cleared
    this.intervalId = setInterval(() => {
      this.fetchData();
    }, 1000);
  }

  stop() {
    clearInterval(this.intervalId);
  }
}

// React useEffect cleanup
function DataLoader() {
  useEffect(() => {
    const intervalId = setInterval(fetchData, 1000);

    // Cleanup function prevents memory leak
    return () => clearInterval(intervalId);
  }, []);
}
```

### 3. Detached DOM Elements

```javascript
function createLeak() {
  const div = document.createElement('div');
  document.body.appendChild(div);

  // Reference keeps div in memory even after removal
  return div;
}

const leakedDiv = createLeak();
document.body.removeChild(leakedDiv); // Removed from DOM
// But leakedDiv still references it in memory

// Fixed: clear reference after removal
function createNoLeak() {
  const div = document.createElement('div');
  document.body.appendChild(div);

  document.body.removeChild(div);
  // No reference returned, can be garbage collected
}
```

### 4. Closures Holding References

```javascript
function createClosure() {
  const largeData = new Array(1000000).fill('x');

  return function inner() {
    // This closure holds reference to largeData
    // even if largeData is not used
    console.log('Inner function');
  };
}

const fn = createClosure(); // largeData stays in memory

// Better approach - don't capture unnecessary data
function createBetterClosure() {
  const result = processLargeData(); // Process immediately

  return function inner() {
    console.log(result); // Only stores the result, not the large data
  };
}
```

### 5. Global Variables Accumulation

```javascript
// Bad: Global variables never get garbage collected
const cache = {};

function addToCache(key, value) {
  cache[key] = value; // Grows infinitely
}

// Better: Use WeakMap for temporary references
const weakCache = new WeakMap();

function addToWeakCache(key, value) {
  weakCache.set(key, value); // Can be garbage collected
}

// Or implement cache eviction
class LRUCache {
  constructor(maxSize) {
    this.maxSize = maxSize;
    this.cache = new Map();
  }

  set(key, value) {
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    this.cache.set(key, value);
  }
}
```

### 6. Detached Object References

```javascript
class DataManager {
  constructor() {
    this.data = new Array(1000000).fill({ value: 0 });
  }

  processData() {
    const processed = this.data.map(item => item.value * 2);
    this.lastProcessed = processed; // Keeps reference
    return processed;
  }
}

const manager = new DataManager();
manager.processData();
// manager.lastProcessed keeps processed array in memory
```

## Memory Leak Detection

```javascript
// Monitor memory usage
function logMemoryUsage() {
  if (performance.memory) {
    console.log({
      usedJSHeapSize: (performance.memory.usedJSHeapSize / 1048576).toFixed(2) + ' MB',
      totalJSHeapSize: (performance.memory.totalJSHeapSize / 1048576).toFixed(2) + ' MB'
    });
  }
}

// Chrome DevTools approach
// 1. Open DevTools > Memory tab
// 2. Take heap snapshot
// 3. Perform actions
// 4. Take another snapshot
// 5. Compare snapshots to find growing objects
```

## Common Use Cases

- **Single-page applications**: Long-running apps accumulate leaks
- **Node.js servers**: Server-side memory leaks cause crashes
- **Browser extensions**: Extensions running indefinitely
- **Real-time applications**: WebSocket connections and event listeners
- **Mobile apps**: Limited memory on mobile devices

## Common Mistakes

1. **Not cleaning up on component unmount** - Forgetting React/Vue lifecycle cleanup
2. **Adding event listeners in loops** - Creating duplicate listeners
3. **Using closures unnecessarily** - Capturing large objects in closures
4. **Not using WeakRef/WeakMap** - Holding strong references unnecessarily
5. **Ignoring browser developer tools** - Not profiling memory usage

## Related Topics

- [[Prevent-MemoryLeaks]]
- [[What-is-Debounce]]
- [[What-is-Throttle]]
- [[Implement-Auth]]

## Quick Revision

| Leak Type | Cause | Solution |
|-----------|-------|----------|
| Event Listeners | Not removing listeners | Remove in cleanup/destroy |
| Timers | Not clearing intervals/timeouts | Clear in cleanup function |
| Detached DOM | References to removed elements | Nullify references |
| Closures | Capturing unnecessary data | Process data immediately |
| Global Variables | Accumulating data | Use WeakMap, implement limits |
