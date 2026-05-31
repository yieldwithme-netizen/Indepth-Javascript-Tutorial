# Writing Callbacks

A callback is a function passed as an argument to another function, to be executed later when an operation completes.

## Basic Callback

```javascript
function greet(name, callback) {
  console.log(`Hello, ${name}`);
  callback();
}

function sayGoodbye() {
  console.log('Goodbye!');
}

greet('Alice', sayGoodbye);
// Output:
// Hello, Alice
// Goodbye!
```

## Callback Pattern

```javascript
function fetchData(url, callback) {
  // Simulate async operation
  setTimeout(() => {
    const data = { id: 1, name: 'John' };
    callback(null, data);
  }, 1000);
}

fetchData('https://api.example.com', (error, data) => {
  if (error) {
    console.error('Error:', error);
    return;
  }
  console.log('Data:', data);
});
```

## Error-First Callbacks

```javascript
function readFile(path, callback) {
  fs.readFile(path, 'utf8', (err, data) => {
    if (err) {
      return callback(err);
    }
    callback(null, data);
  });
}

readFile('file.txt', (err, data) => {
  if (err) {
    console.error('Read failed:', err.message);
    return;
  }
  console.log('Content:', data);
});
```

## Async Callbacks

```javascript
function asyncOperation(callback) {
  setTimeout(() => {
    const success = Math.random() > 0.5;
    if (success) {
      callback(null, 'Operation completed');
    } else {
      callback(new Error('Operation failed'));
    }
  }, 1000);
}

asyncOperation((err, result) => {
  if (err) {
    console.error(err.message);
    return;
  }
  console.log(result);
});
```

## Callback Hell

```javascript
// Callback hell (avoid this)
getData(function(a) {
  getMoreData(a, function(b) {
    getEvenMoreData(b, function(c) {
      console.log(c);
    });
  });
});

// Better: Use Promises or async/await
getData()
  .then(getMoreData)
  .then(getEvenMoreData)
  .then(console.log)
  .catch(console.error);
```

## Common Use Cases

- Event handlers
- Async operations (file I/O, network requests)
- Array methods (map, filter, forEach)
- Timers (setTimeout, setInterval)
- Node.js error handling

## Common Mistakes

- Not handling errors in callbacks
- Creating callback hell
- Calling callback multiple times
- Not returning after error handling
- Blocking the event loop

## Related Topics

- [[Promises]]
- [[Async/Await]]
- [[Higher-Order Functions]]
- [[Event Handlers]]
- [[Node.js]]

## Quick Revision

- Callbacks are functions passed as arguments
- Error-first pattern: `(error, result) => {}`
- Avoid callback hell with Promises or async/await
- Always handle errors in callbacks
- Common in async operations and event handling
- Return after error to prevent further execution
