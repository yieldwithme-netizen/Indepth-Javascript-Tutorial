# Express

## Definition

Express is a **web framework** for Node.js.

## Basic App

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000);
```

## Quick Revision

- Express = Node.js web framework
- Routes: get, post, put, delete
- Middleware for processing
- req/res objects

---

## Related Topics

- [[What-is-Express]] - [[What-is-Express|Express]]
- [[Express]] - [[Express|Express]]
- [[Express.js]] - [[Express.js|Express.js]]
- [[Create-App]] - [[Create-App|Creating app]]
