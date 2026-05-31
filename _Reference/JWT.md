# JWT (JSON Web Token)

## Definition

JWT is a **compact, URL-safe token** for securely transmitting information.

## Structure

```
Header.Payload.Signature
```

## Basic Usage

```javascript
const jwt = require('jsonwebtoken');

// Create token
const token = jwt.sign(
    { userId: 123 },
    'secret-key',
    { expiresIn: '1h' }
);

// Verify token
const decoded = jwt.verify(token, 'secret-key');
```

## Quick Revision

- JWT = secure token
- Header, payload, signature
- Used for authentication
- Stateless (no server storage)

---

## Related Topics

- [[What-is-JWT]] - [[What-is-JWT|JWT]]
- [[JWT]] - [[JWT|JWT]]
- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
- [[Implement-Auth]] - [[Implement-Auth|Authentication]]
