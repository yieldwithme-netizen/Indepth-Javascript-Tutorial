# Middleware

## Definition

Middleware **processes requests** before they reach route handlers.

## Express Middleware

```javascript
// Built-in
app.use(express.json());
app.use(express.static('public'));

// Custom
const logger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next();
};
app.use(logger);
```

## Quick Revision

- Middleware runs before routes
- Must call `next()` to continue
- Use for: logging, parsing, auth
- Chain multiple middleware

---

## Related Topics

- [[What-is-Middleware]] - [[What-is-Middleware|Middleware]]
- [[Middleware]] - [[Middleware|Middleware]]
- [[Use-Middleware]] - [[Use-Middleware|Using middleware]]
- [[What-is-Express]] - [[What-is-Express|Express]]
