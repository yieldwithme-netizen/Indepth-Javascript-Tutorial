# Sessions

## Definition

Sessions **store user data** across multiple requests.

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

- Sessions store server-side data
- Use express-session for Express
- Data stored on server
- Client gets session ID cookie

---

## Related Topics

- [[What-is-Sessions]] - [[What-is-Sessions|Sessions]]
- [[Sessions]] - [[Sessions|Sessions]]
- [[What-is-Cookies]] - [[What-is-Cookies|Cookies]]
- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
