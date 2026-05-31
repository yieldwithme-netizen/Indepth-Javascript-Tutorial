# API Development

## Definition

API development involves **creating interfaces** for software components to communicate.

## REST API

```javascript
const express = require('express');
const app = express();

// GET
app.get('/api/users', (req, res) => {
    res.json(users);
});

// POST
app.post('/api/users', (req, res) => {
    const user = createUser(req.body);
    res.status(201).json(user);
});

// PUT
app.put('/api/users/:id', (req, res) => {
    const user = updateUser(req.params.id, req.body);
    res.json(user);
});

// DELETE
app.delete('/api/users/:id', (req, res) => {
    deleteUser(req.params.id);
    res.status(204).send();
});
```

## Quick Revision

- API = interface for communication
- REST = architectural style
- HTTP methods: GET, POST, PUT, DELETE
- Status codes indicate result
- JSON for data format

---

## Related Topics

- [[What-is-API]] - [[What-is-API|APIs]]
- [[What-is-REST]] - [[What-is-REST|REST]]
- [[API-Development]] - [[API-Development|API development]]
- [[What-is-Express]] - [[What-is-Express|Express]]
