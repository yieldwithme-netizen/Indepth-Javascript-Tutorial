# Express.js

## Definition

Express.js is a **web framework** for Node.js that simplifies HTTP server creation.

## Basic App

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000);
```

## Routes

```javascript
app.get('/users', (req, res) => { });
app.post('/users', (req, res) => { });
app.put('/users/:id', (req, res) => { });
app.delete('/users/:id', (req, res) => { });
```

## Middleware

```javascript
app.use(express.json());
app.use(express.static('public'));
```

## Quick Revision

- Express = Node.js web framework
- Routes: get, post, put, delete
- Middleware runs before routes
- req = request, res = response

---

## Related Topics

- [[What-is-Express]] - [[What-is-Express|Express]]
- [[Express.js]] - [[Express.js|Express.js]]
- [[Create-App]] - [[Create-App|Creating app]]
- [[What-is-Routes]] - [[What-is-Routes|Routes]]
- [[What-is-Middleware]] - [[What-is-Middleware|Middleware]]
