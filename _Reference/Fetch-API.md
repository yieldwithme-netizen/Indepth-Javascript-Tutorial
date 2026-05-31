# Fetch API

## Definition
The Fetch API is a modern interface for making HTTP requests in the browser and Node.js. It provides a cleaner, more powerful alternative to XMLHttpRequest with native Promise support.

## Basic Syntax

### GET Request
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### POST Request
```javascript
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

### Async/Await Syntax
```javascript
async function getUsers() {
  try {
    const response = await fetch('https://api.example.com/users');
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const users = await response.json();
    console.log(users);
  } catch (error) {
    console.error('Failed to fetch users:', error);
  }
}
```

## Request Options

### Full Configuration
```javascript
fetch('https://api.example.com/data', {
  method: 'POST',
  mode: 'cors',
  cache: 'no-cache',
  credentials: 'same-origin',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token123'
  },
  redirect: 'follow',
  referrerPolicy: 'no-referrer',
  body: JSON.stringify({ key: 'value' })
});
```

### Different Content Types
```javascript
// Form data
const formData = new FormData();
formData.append('file', fileInput.files[0]);
fetch('/upload', { method: 'POST', body: formData });

// URL encoded
fetch('/api', {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: 'username=john&password=123'
});
```

## Response Handling

### Checking Response Status
```javascript
async function fetchData(url) {
  const response = await fetch(url);
  
  if (response.ok) {
    const data = await response.json();
    return data;
  } else if (response.status === 404) {
    throw new Error('Resource not found');
  } else if (response.status === 500) {
    throw new Error('Server error');
  } else {
    throw new Error(`HTTP error: ${response.status}`);
  }
}
```

### Reading Different Response Types
```javascript
// JSON
const jsonData = await response.json();

// Text
const textData = await response.text();

// Blob
const blobData = await response.blob();

// ArrayBuffer
const arrayBuffer = await response.arrayBuffer();
```

## Common Use Cases
- REST API communication
- Loading data dynamically
- Form submissions
- File uploads
- Real-time data updates
- Authentication flows

## Common Mistakes
- Not handling HTTP errors (4xx, 5xx)
- Forgetting to parse JSON response
- Not checking `response.ok`
- Missing Content-Type header for POST
- Not handling network errors

## Quick Revision Summary
- `fetch()` returns a Promise
- Always check `response.ok` or status code
- Use `response.json()` for JSON data
- Supports all HTTP methods
- Use async/await for cleaner syntax
- Handle both network and HTTP errors

## Related Topics
- [[Promises]]
- [[Async-Await]]
- [[HTTP-Requests]]
- [[REST-API]]
- [[AJAX]]
