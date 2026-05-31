# Sessions

## Definition

Sessions **store user data** across multiple requests on the server.

## Express Sessions

```javascript
const session = require('express-session');

app.use(session({
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true }
}));

// Set
req.session.user = { name: "John" };

// Get
const user = req.session.user;
```

## Quick Revision

- Sessions: server-side storage
- Client gets session ID cookie
- Data persists across requests
- Use for: authentication, shopping carts

---

## Related Topics

- [[What-is-Sessions]] - [[What-is-Sessions|Sessions]]
- [[What-is-Sessions]] - [[What-is-Sessions|Sessions]]
- [[Sessions]] - [[Sessions|Sessions]]
- [[Session-Management]] - [[Session-Management|Session management]]
- [[What-is-Cookies]] - [[What-is-Cookies|Cookies]]
