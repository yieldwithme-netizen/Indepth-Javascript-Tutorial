# How to Prevent Memory Leaks

Preventing memory leaks requires awareness of common patterns that cause them and disciplined cleanup practices. This guide covers techniques to keep your applications memory-efficient.

## React Memory Leak Prevention

### useEffect Cleanup

```javascript
import { useState, useEffect, useRef } from 'react';

function DataFetcher({ url }) {
  const [data, setData] = useState(null);
  const abortControllerRef = useRef(null);

  useEffect(() => {
    abortControllerRef.current = new AbortController();

    const fetchData = async () => {
      try {
        const response = await fetch(url, {
          signal: abortControllerRef.current.signal
        });
        const json = await response.json();
        setData(json);
      } catch (error) {
        if (error.name !== 'AbortError') {
          console.error(error);
        }
      }
    };

    fetchData();

    // Cleanup: abort request on unmount
    return () => {
      abortControllerRef.current.abort();
    };
  }, [url]);

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

### Event Listener Cleanup

```javascript
function WindowSizeTracker() {
  const [size, setSize] = useState({ width: 0, height: 0 });

  useEffect(() => {
    const handleResize = () => {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    };

    window.addEventListener('resize', handleResize);

    // Cleanup: remove listener on unmount
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return <div>Width: {size.width}, Height: {size.height}</div>;
}
```

### Timer Cleanup

```javascript
function Countdown({ initialSeconds }) {
  const [seconds, setSeconds] = useState(initialSeconds);
  const intervalRef = useRef(null);

  useEffect(() => {
    intervalRef.current = setInterval(() => {
      setSeconds(prev => (prev > 0 ? prev - 1 : 0));
    }, 1000);

    // Cleanup: clear interval on unmount
    return () => clearInterval(intervalRef.current);
  }, []);

  return <div>Time remaining: {seconds}s</div>;
}
```

## Vanilla JavaScript Cleanup

```javascript
class ResourceManager {
  constructor() {
    this.timers = [];
    this.listeners = [];
    this.intervals = [];
  }

  addTimer(callback, delay) {
    const id = setTimeout(callback, delay);
    this.timers.push(id);
    return id;
  }

  addInterval(callback, delay) {
    const id = setInterval(callback, delay);
    this.intervals.push(id);
    return id;
  }

  addEventListener(element, event, handler) {
    element.addEventListener(event, handler);
    this.listeners.push({ element, event, handler });
  }

  cleanup() {
    // Clear all timers
    this.timers.forEach(id => clearTimeout(id));
    this.timers = [];

    // Clear all intervals
    this.intervals.forEach(id => clearInterval(id));
    this.intervals = [];

    // Remove all event listeners
    this.listeners.forEach(({ element, event, handler }) => {
      element.removeEventListener(event, handler);
    });
    this.listeners = [];
  }
}

// Usage
const manager = new ResourceManager();

manager.addTimer(() => console.log('timer'), 1000);
manager.addEventListener(window, 'resize', () => {});

// Cleanup when done
manager.cleanup();
```

## WeakRef and FinalizationRegistry

```javascript
// WeakRef allows references to be garbage collected
class Cache {
  constructor() {
    this.cache = new Map();
  }

  set(key, value) {
    this.cache.set(key, new WeakRef(value));
  }

  get(key) {
    const ref = this.cache.get(key);
    if (ref) {
      const value = ref.deref();
      if (value !== undefined) {
        return value;
      }
      this.cache.delete(key);
    }
    return undefined;
  }
}

// FinalizationRegistry for cleanup callbacks
const registry = new FinalizationRegistry((heldValue) => {
  console.log(`Object with ${heldValue} was garbage collected`);
});

class ExpensiveObject {
  constructor(id) {
    this.id = id;
    registry.register(this, id);
  }
}
```

## Node.js Memory Management

```javascript
// Stream processing to avoid loading entire files
const fs = require('fs');
const zlib = require('zlib');

// Bad: loads entire file into memory
function processLargeFileBad(filePath) {
  const data = fs.readFileSync(filePath); // Memory intensive
  return processData(data);
}

// Good: stream processing
function processLargeFileGood(filePath) {
  return new Promise((resolve, reject) => {
    const chunks = [];
    const stream = fs.createReadStream(filePath);

    stream.on('data', chunk => chunks.push(chunk));
    stream.on('end', () => resolve(Buffer.concat(chunks)));
    stream.on('error', reject);
  });
}

// Event listener cleanup in Node.js
class Server {
  constructor() {
    this.connections = new Set();
  }

  handleConnection(socket) {
    this.connections.add(socket);

    socket.on('close', () => {
      this.connections.delete(socket);
    });
  }

  cleanup() {
    this.connections.forEach(socket => socket.destroy());
    this.connections.clear();
  }
}
```

## Memory-Efficient Data Structures

```javascript
// Use TypedArrays for numeric data
const float32Array = new Float32Array(1000000); // 4MB
const regularArray = new Array(1000000); // Much more memory

// Use Maps instead of objects for frequent additions/deletions
const map = new Map();
const obj = {};

// Map is better for dynamic key operations
map.delete('key'); // O(1)
delete obj['key']; // Slower

// Use WeakMap for temporary object references
const weakMap = new WeakMap();
let tempObj = { data: 'value' };
weakMap.set(tempObj, 'metadata');
tempObj = null; // Object can now be garbage collected
```

## Common Use Cases

- **Single-page applications**: Managing component lifecycle
- **Server-side rendering**: Cleaning up between requests
- **WebSocket connections**: Properly closing connections
- **File upload handlers**: Processing large files efficiently
- **Background tasks**: Cleaning up completed tasks

## Common Mistakes

1. **Not returning cleanup functions from useEffect**
2. **Forgetting to clear timers in componentWillUnmount**
3. **Storing references to DOM elements after removal**
4. **Using large objects in closures when not needed**
5. **Not implementing cache eviction policies**

## Related Topics

- [[What-is-MemoryLeak]]
- [[What-is-Debounce]]
- [[What-is-Throttle]]
- [[Implement-Auth]]

## Quick Revision

| Pattern | Prevention |
|---------|------------|
| Event Listeners | Always remove in cleanup function |
| Timers | Clear all intervals/timeouts |
| DOM References | Nullify references after removal |
| Closures | Avoid capturing large objects |
| Cache | Implement size limits and eviction |
| Streams | Use streaming for large data |
| WeakRef | Use for temporary references |
