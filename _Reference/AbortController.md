# AbortController - Cancellation API

## Definition

`AbortController` provides a signal to cancel fetch requests, event listeners, and other asynchronous operations. It's the modern, standardized way to handle cancellation in JavaScript.

```javascript
const controller = new AbortController();
const signal = controller.signal;
```

## Basic Usage

```javascript
const controller = new AbortController();
const signal = controller.signal;

fetch('https://api.example.com/data', { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('Request cancelled');
    } else {
      console.error('Request failed:', err);
    }
  });

// Cancel the request
controller.abort();
```

## Common Use Cases

### 1. Timeouts

```javascript
function fetchWithTimeout(url, timeoutMs) {
  const controller = new AbortController();
  const signal = controller.signal;
  
  // Set timeout
  setTimeout(() => controller.abort(), timeoutMs);
  
  return fetch(url, { signal });
}

// Use it
fetchWithTimeout('https://api.example.com/slow', 5000)
  .then(res => res.json())
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('Request timed out');
    }
  });
```

### 2. Race Condition Prevention

```javascript
let currentController = null;

async function search(query) {
  // Cancel previous search
  if (currentController) {
    currentController.abort();
  }
  
  currentController = new AbortController();
  const signal = currentController.signal;
  
  try {
    const response = await fetch(`/api/search?q=${query}`, { signal });
    const results = await response.json();
    return results;
  } catch (err) {
    if (err.name === 'AbortError') {
      console.log('Search cancelled');
      return null;
    }
    throw err;
  }
}

// User types quickly
search('ap');  // Starts
search('app'); // Cancels previous, starts new
search('appl'); // Cancels previous, starts new
```

### 3. Cancelling Event Listeners

```javascript
function onClickWithAbort(element) {
  const controller = new AbortController();
  const signal = controller.signal;
  
  element.addEventListener('click', () => {
    console.log('Clicked!');
  }, { signal });
  
  // Return cleanup function
  return () => controller.abort();
}

const cleanup = onClickWithAbort(document.querySelector('#myButton'));

// Later, remove the listener
cleanup(); // Removes click listener
```

### 4. Multiple Requests Cancellation

```javascript
async function fetchMultiple(urls) {
  const controller = new AbortController();
  const signal = controller.signal;
  
  try {
    const responses = await Promise.all(
      urls.map(url => fetch(url, { signal }))
    );
    return await Promise.all(responses.map(r => r.json()));
  } catch (err) {
    if (err.name === 'AbortError') {
      console.log('All requests cancelled');
    }
    throw err;
  }
}

// Cancel all requests
const controller = new AbortController();
fetchMultiple(urls, { signal: controller.signal });
controller.abort();
```

### 5. Custom Event Cancellation

```javascript
class DataLoader {
  constructor() {
    this.controller = null;
  }
  
  async load(url) {
    // Cancel previous load
    if (this.controller) {
      this.controller.abort();
    }
    
    this.controller = new AbortController();
    const signal = this.controller.signal;
    
    try {
      const response = await fetch(url, { signal });
      return await response.json();
    } catch (err) {
      if (err.name === 'AbortError') {
        console.log('Load cancelled');
        return null;
      }
      throw err;
    }
  }
  
  cancel() {
    if (this.controller) {
      this.controller.abort();
      this.controller = null;
    }
  }
}

const loader = new DataLoader();
loader.load('/api/data1'); // Starts
loader.load('/api/data2'); // Cancels first
loader.cancel();           // Cancels current
```

### 6. AbortController with setTimeout

```javascript
function delay(ms, { signal } = {}) {
  return new Promise((resolve, reject) => {
    if (signal?.aborted) {
      reject(new DOMException('Aborted', 'AbortError'));
      return;
    }
    
    const timer = setTimeout(resolve, ms);
    
    signal?.addEventListener('abort', () => {
      clearTimeout(timer);
      reject(new DOMException('Aborted', 'AbortError'));
    });
  });
}

const controller = new AbortController();
delay(5000, { signal: controller.signal })
  .then(() => console.log('Done'))
  .catch(err => console.log('Cancelled'));

controller.abort(); // Cancels after any time
```

## Common Mistakes

```javascript
// ❌ Wrong: Not checking AbortError
fetch(url, { signal })
  .catch(err => {
    console.error(err); // Logs AbortError as error
  });

// ✅ Correct: Handle AbortError specifically
fetch(url, { signal })
  .catch(err => {
    if (err.name === 'AbortError') return;
    console.error(err);
  });

// ❌ Wrong: Aborting after operation completes
const controller = new AbortController();
fetch(url, { signal: controller.signal })
  .then(res => res.json())
  .then(data => {
    controller.abort(); // Unnecessary, but harmless
  });

// ✅ Correct: Only abort when needed
if (needsCancellation) {
  controller.abort();
}
```

## EventListenerOptions with Signal

```javascript
// Modern way to add event listeners with cleanup
const controller = new AbortController();

document.addEventListener('scroll', () => {
  console.log('Scrolling');
}, { signal: controller.signal });

// Later, remove all listeners using this signal
controller.abort();
```

## Quick Revision Summary

- `AbortController` creates cancellation signals
- Pass `signal` to `fetch()` or `addEventListener()`
- Call `controller.abort()` to cancel
- Check for `AbortError` name in catch blocks
- Use for timeouts, race conditions, cleanup

## Related Topics

- [[Fetch]] - HTTP requests with cancellation
- [[Promises]] - Async operation handling
- [[AsyncAwait]] - Modern async syntax
- [[EventListeners]] - DOM event handling
- [[CancellationPatterns]] - Other cancellation approaches
