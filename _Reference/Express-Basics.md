# Express Basics

## Definition

Express basics covers **fundamental Express.js concepts**.

## Creating App

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

- Express = web framework
- Routes: get, post, put, delete
- Middleware runs before routes
- req/res objects

---

## Related Topics

- [[What-is-Express]] - [[What-is-Express|Express]]
- [[Express.js]] - [[Express.js|Express.js]]
- [[Express-Basics]] - [[Express-Basics|Express basics]]
- [[Create-App]] - [[Create-App|Creating app]]
