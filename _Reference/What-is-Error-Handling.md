# Error Handling

Error handling in JavaScript uses `try`, `catch`, `finally`, and `throw` to manage runtime errors gracefully.

## Basic Try-Catch

```javascript
try {
  const result = riskyOperation();
  console.log(result);
} catch (error) {
  console.error('Something went wrong:', error.message);
} finally {
  console.log('This always runs');
}
```

## Throwing Errors

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error('Division by zero');
  }
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (error) {
  console.error(error.message); // Division by zero
}
```

## Custom Error Classes

```javascript
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
  }
}

class NotFoundError extends Error {
  constructor(resource) {
    super(`${resource} not found`);
    this.name = 'NotFoundError';
    this.resource = resource;
  }
}

function validateAge(age) {
  if (age < 0 || age > 150) {
    throw new ValidationError('Invalid age', 'age');
  }
  return true;
}

try {
  validateAge(-5);
} catch (error) {
  if (error instanceof ValidationError) {
    console.error(`Validation failed: ${error.message}`);
    console.error(`Field: ${error.field}`);
  }
}
```

## Async Error Handling

```javascript
// Async/Await
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Fetch failed:', error.message);
    throw error;
  }
}

// Promise chains
function fetchDataWithPromises(url) {
  return fetch(url)
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json();
    })
    .catch(error => {
      console.error('Fetch failed:', error.message);
      throw error;
    });
}
```

## Error Propagation

```javascript
function processData(data) {
  try {
    const validated = validateData(data);
    const transformed = transformData(validated);
    return saveData(transformed);
  } catch (error) {
    if (error instanceof ValidationError) {
      console.error('Validation failed:', error.message);
      return null;
    }
    throw error; // Re-throw if not handled
  }
}
```

## Common Use Cases

- Form validation
- API error handling
- File operations
- Network requests
- User input validation

## Common Mistakes

- Catching all errors without specific handling
- Swallowing errors silently
- Not re-throwing errors that should propagate
- Using try-catch in performance-critical loops
- Forgetting finally block for cleanup

## Related Topics

- [[Promises]]
- [[Async/Await]]
- [[Throw Statement]]
- [[Error Object]]
- [[Try Statement]]

## Quick Revision

- Use `try-catch-finally` for synchronous error handling
- `throw` creates custom errors
- Extend `Error` class for custom error types
- Use try-catch with async/await for async error handling
- Always handle errors or re-throw them
- `finally` runs regardless of success or failure
