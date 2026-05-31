# Session Management

## Definition

Session management **tracks user state** across requests.

## Express Sessions

```javascript
const session = require('express-session');

app.use(session({
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true
}));

// Set
req.session.user = { name: "John" };

// Get
const user = req.session.user;
```

## Quick Revision

- Sessions store server-side data
- Use express-session
- Client gets session ID cookie
- Data persists across requests

---

## Related Topics

- [[What-is-Sessions]] - [[What-is-Sessions|Sessions]]
- [[Sessions]] - [[Sessions|Sessions]]
- [[Session-Management]] - [[Session-Management|Session management]]
- [[What-is-Cookies]] - [[What-is-Cookies|Cookies]]
