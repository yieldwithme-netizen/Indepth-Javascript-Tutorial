# AJAX

## Definition
AJAX (Asynchronous JavaScript and XML) is a technique that allows web pages to be updated asynchronously by exchanging data with a server in the background. It enables updating parts of a web page without reloading the entire page.

## Modern Approach
While XMLHttpRequest was the original AJAX method, modern JavaScript uses the `fetch()` API and `async/await`.

## Code Examples

### XMLHttpRequest
```javascript
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);

xhr.onreadystatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    const data = JSON.parse(xhr.responseText);
    console.log(data);
  }
};

xhr.onerror = function() {
  console.error('Request failed');
};

xhr.send();
```

### Fetch API
```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Fetch with Async/Await
```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### POST Request
```javascript
async function postData(url, data) {
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  });
  return response.json();
}

postData('https://api.example.com/users', { name: 'John', age: 30 });
```

### Fetch with Headers
```javascript
fetch('https://api.example.com/data', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer token123',
    'Accept': 'application/json'
  }
});
```

## Common Use Cases
- Form submissions without page reload
- Loading data dynamically
- Auto-saving user input
- Infinite scrolling
- Real-time updates

## Common Mistakes
- **Not handling errors**: Always use `.catch()` or `try/catch`
- **Ignoring HTTP status codes**: Check `response.ok` or `response.status`
- **Not setting Content-Type**: Required for POST/PUT requests
- **CORS issues**: Server must allow cross-origin requests

## Related Topics
- [[Fetch-API]]
- [[Promise]]
- [[AsyncAwait]]
- [[XMLHttpRequest]]
- [[CORS]]

## Quick Revision
- AJAX updates web pages without full reloads
- `fetch()` is the modern, Promise-based approach
- Always handle errors and check status codes
- Use `async/await` for cleaner asynchronous code
