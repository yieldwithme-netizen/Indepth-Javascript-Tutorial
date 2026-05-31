# Web Workers - Background Threads

## Definition

Web Workers run scripts in background threads, allowing heavy computations without blocking the main thread. Workers communicate with the main thread via messages.

```javascript
const worker = new Worker('worker.js');
```

## Basic Usage

### Main Thread

```javascript
// main.js
const worker = new Worker('worker.js');

worker.postMessage({ data: [1, 2, 3, 4, 5] });

worker.onmessage = (e) => {
  console.log('Result:', e.data);
};

worker.onerror = (error) => {
  console.error('Worker error:', error);
};
```

### Worker Thread

```javascript
// worker.js
self.onmessage = (e) => {
  const result = e.data.data.reduce((sum, num) => sum + num, 0);
  self.postMessage(result);
};
```

## Common Use Cases

### 1. Heavy Computation

```javascript
// main.js
const worker = new Worker('calculator.js');

function calculatePrimes(max) {
  return new Promise((resolve) => {
    worker.onmessage = (e) => resolve(e.data);
    worker.postMessage({ max });
  });
}

calculatePrimes(1000000).then(primes => {
  console.log(`Found ${primes.length} primes`);
});
```

```javascript
// calculator.js
self.onmessage = (e) => {
  const { max } = e.data;
  const primes = [];
  
  for (let i = 2; i <= max; i++) {
    let isPrime = true;
    for (let j = 2; j <= Math.sqrt(i); j++) {
      if (i % j === 0) {
        isPrime = false;
        break;
      }
    }
    if (isPrime) primes.push(i);
  }
  
  self.postMessage(primes);
};
```

### 2. Data Processing Pipeline

```javascript
// main.js
const worker = new Worker('processor.js');

const data = new Float64Array(1000000);
for (let i = 0; i < data.length; i++) {
  data[i] = Math.random();
}

worker.postMessage(data, [data.buffer]); // Transfer buffer

worker.onmessage = (e) => {
  console.log('Processed data:', e.data);
};
```

```javascript
// processor.js
self.onmessage = (e) => {
  const data = e.data;
  const result = new Float64Array(data.length);
  
  for (let i = 0; i < data.length; i++) {
    result[i] = Math.sqrt(data[i]) * Math.sin(data[i]);
  }
  
  self.postMessage(result, [result.buffer]);
};
```

### 3. Image Processing

```javascript
// main.js
const canvas = document.querySelector('#canvas');
const ctx = canvas.getContext('2d');
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

const worker = new Worker('image-processor.js');
worker.postMessage({ imageData: imageData.data, width: canvas.width });

worker.onmessage = (e) => {
  const newImageData = new ImageData(
    new Uint8ClampedArray(e.data),
    canvas.width,
    canvas.height
  );
  ctx.putImageData(newImageData, 0, 0);
};
```

```javascript
// image-processor.js
self.onmessage = (e) => {
  const { imageData, width } = e.data;
  const result = new Uint8ClampedArray(imageData.length);
  
  for (let i = 0; i < imageData.length; i += 4) {
    // Grayscale conversion
    const avg = (imageData[i] + imageData[i + 1] + imageData[i + 2]) / 3;
    result[i] = avg;     // R
    result[i + 1] = avg; // G
    result[i + 2] = avg; // B
    result[i + 3] = imageData[i + 3]; // A
  }
  
  self.postMessage(result);
};
```

### 4. Parallel Task Processing

```javascript
// main.js
function parallelProcess(data, numWorkers) {
  return new Promise((resolve) => {
    const results = [];
    const chunkSize = Math.ceil(data.length / numWorkers);
    let completed = 0;
    
    for (let i = 0; i < numWorkers; i++) {
      const worker = new Worker('chunk-processor.js');
      const chunk = data.slice(i * chunkSize, (i + 1) * chunkSize);
      
      worker.onmessage = (e) => {
        results.push(e.data);
        completed++;
        if (completed === numWorkers) {
          resolve(results.flat());
        }
      };
      
      worker.postMessage(chunk);
    }
  });
}

parallelProcess([1, 2, 3, 4, 5, 6, 7, 8], 4)
  .then(results => console.log(results));
```

### 5. Worker Pool

```javascript
class WorkerPool {
  constructor(workerScript, poolSize = navigator.hardwareConcurrency || 4) {
    this.workers = [];
    this.queue = [];
    
    for (let i = 0; i < poolSize; i++) {
      const worker = new Worker(workerScript);
      this.workers.push({ worker, busy: false });
    }
  }
  
  async run(data) {
    return new Promise((resolve) => {
      const available = this.workers.find(w => !w.busy);
      
      if (available) {
        available.busy = true;
        available.worker.onmessage = (e) => {
          available.busy = false;
          resolve(e.data);
        };
        available.worker.postMessage(data);
      } else {
        this.queue.push({ data, resolve });
      }
    });
  }
  
  terminate() {
    this.workers.forEach(w => w.worker.terminate());
  }
}

const pool = new WorkerPool('processor.js', 4);
const results = await Promise.all([
  pool.run(data1),
  pool.run(data2),
  pool.run(data3),
  pool.run(data4)
]);
```

## Message Types

```javascript
// Structured cloning (default)
worker.postMessage({ 
  array: new Float64Array([1, 2, 3]),
  map: new Map([['a', 1]])
});

// Transferable objects (zero-copy)
const buffer = new ArrayBuffer(1024 * 1024);
worker.postMessage(buffer, [buffer]);
// buffer.byteLength is now 0 (transferred)

// SharedArrayBuffer (shared memory)
const sharedBuffer = new SharedArrayBuffer(1024);
const sharedArray = new Int32Array(sharedBuffer);
worker.postMessage(sharedBuffer);
```

## Common Mistakes

```javascript
// ❌ Wrong: Using DOM in worker
// worker.js
document.querySelector('#app'); // Error: document is not defined

// ✅ Correct: Use only worker-safe APIs
// worker.js
self.postMessage('Hello from worker');

// ❌ Wrong: Not handling errors
const worker = new Worker('worker.js');
// No error handling

// ✅ Correct: Always handle errors
const worker = new Worker('worker.js');
worker.onerror = (error) => {
  console.error('Worker failed:', error.message);
};

// ❌ Wrong: Forgetting to terminate workers
// Workers run until explicitly terminated
worker.terminate(); // Call when done
```

## Worker vs Main Thread APIs

| API | Main Thread | Worker |
|-----|-------------|--------|
| DOM | ✅ | ❌ |
| `window` | ✅ | ❌ |
| `importScripts()` | ❌ | ✅ |
| `XMLHttpRequest` | ✅ | ✅ (deprecated) |
| `fetch()` | ✅ | ✅ |
| `setTimeout()` | ✅ | ✅ |
| `navigator.hardwareConcurrency` | ✅ | ✅ |

## Quick Revision Summary

- Web Workers run code in background threads
- Communication via `postMessage()` and `onmessage`
- No DOM access in workers
- Use `Transferable` objects for large data
- Always terminate workers when done

## Related Topics

- [[ServiceWorkers]] - Background script for offline/caching
- [[SharedArrayBuffer]] - Shared memory between threads
- [[Atomics]] - Atomic operations on shared data
- [[AsyncAwait]] - Async patterns in main thread
- [[Performance]] - Optimizing heavy computations
