# What is REST

## Definition

**REST (Representational State Transfer)** is an architectural style for designing networked applications. It uses HTTP methods to perform operations on resources, which are identified by URIs. RESTful APIs are stateless, meaning each request contains all information needed to process it.

## REST Principles

1. **Stateless**: Each request is independent
2. **Client-Server**: Separation of concerns
3. **Cacheable**: Responses can be cached
4. **Uniform Interface**: Consistent API design
5. **Layered System**: Architecture can have multiple layers

## HTTP Methods

```javascript
// GET - Retrieve data
app.get('/api/users', (req, res) => {
  res.json(users);
});

// POST - Create new resource
app.post('/api/users', (req, res) => {
  const newUser = { id: users.length + 1, ...req.body };
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT - Update entire resource
app.put('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index !== -1) {
    users[index] = { id: parseInt(req.params.id), ...req.body };
    res.json(users[index]);
  } else {
    res.status(404).json({ error: 'User not found' });
  }
});

// PATCH - Partial update
app.patch('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index !== -1) {
    users[index] = { ...users[index], ...req.body };
    res.json(users[index]);
  } else {
    res.status(404).json({ error: 'User not found' });
  }
});

// DELETE - Remove resource
app.delete('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index !== -1) {
    users.splice(index, 1);
    res.status(204).send();
  } else {
    res.status(404).json({ error: 'User not found' });
  }
});
```

## RESTful API Design

```javascript
// Resource naming conventions
// Good
GET /api/users
GET /api/users/123
GET /api/users/123/orders
GET /api/users/123/orders/456

// Bad
GET /api/getUsers
GET /api/user?id=123
GET /api/userOrders?userId=123

// Status codes
app.post('/api/users', (req, res) => {
  try {
    const user = createUser(req.body);
    res.status(201).json(user);  // 201 Created
  } catch (error) {
    res.status(400).json({ error: error.message });  // 400 Bad Request
  }
});

app.get('/api/users/:id', (req, res) => {
  const user = findUser(req.params.id);
  if (!user) {
    return res.status(404).json({ error: 'Not found' });  // 404 Not Found
  }
  res.json(user);
});
```

## Pagination, Sorting, Filtering

```javascript
// Query parameters for list endpoints
app.get('/api/users', (req, res) => {
  const { page = 1, limit = 10, sort = 'name', order = 'asc', search } = req.query;
  
  let filteredUsers = [...users];
  
  // Search
  if (search) {
    filteredUsers = filteredUsers.filter(u => 
      u.name.toLowerCase().includes(search.toLowerCase())
    );
  }
  
  // Sort
  filteredUsers.sort((a, b) => {
    if (order === 'asc') return a[sort] > b[sort] ? 1 : -1;
    return a[sort] < b[sort] ? 1 : -1;
  });
  
  // Pagination
  const startIndex = (page - 1) * limit;
  const paginatedUsers = filteredUsers.slice(startIndex, startIndex + limit);
  
  res.json({
    data: paginatedUsers,
    pagination: {
      page: parseInt(page),
      limit: parseInt(limit),
      total: filteredUsers.length,
      pages: Math.ceil(filteredUsers.length / limit)
    }
  });
});
```

## Common Mistakes

1. **Not using proper HTTP methods**: Don't use GET for mutations
2. **Ignoring status codes**: Return appropriate codes
3. **Exposing internal IDs**: Use UUIDs if needed
4. **Not versioning APIs**: Use `/api/v1/` prefix
5. **Over-fetching data**: Return only needed fields

## Related Topics

- [[What-is-GraphQL]]
- [[What-is-CORS]]
- [[Handle-CORS]]
- [[What-is-JWT]]

## Quick Revision

- REST uses HTTP methods (GET, POST, PUT, PATCH, DELETE)
- Resources are identified by URIs
- Stateless architecture - each request is independent
- Use proper status codes (200, 201, 400, 404, 500)
- Support pagination, sorting, and filtering
