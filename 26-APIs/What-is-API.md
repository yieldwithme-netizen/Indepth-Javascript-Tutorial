# What is an API?

## Definition

An API (Application Programming Interface) is a **contract for how software components communicate**.

## REST API

```javascript
// REST endpoints
GET    /api/users      - Get all users
GET    /api/users/1    - Get user 1
POST   /api/users      - Create user
PUT    /api/users/1    - Update user 1
DELETE /api/users/1    - Delete user 1
```

## HTTP Methods

| Method | Purpose | Idempotent |
|--------|---------|------------|
| GET | Read data | Yes |
| POST | Create data | No |
| PUT | Update data | Yes |
| DELETE | Delete data | Yes |

## Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 500 | Server Error |

## Quick Revision

- API = contract for communication
- REST = architectural style
- Methods: GET, POST, PUT, DELETE
- Status codes indicate result
- JSON is common data format

---

## Related Topics

- [[What-is-API]] - API overview
- [[What-is-REST]] - REST architecture
- [[What-is-GraphQL]] - GraphQL
- [[What-is-WebSocket]] - WebSockets
- [[What-is-CORS]] - CORS
- [[Make-HTTP]] - HTTP requests
