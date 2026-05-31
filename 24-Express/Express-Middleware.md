# Express Middleware

## Definition

Express middleware **processes requests** before handlers.

## Example

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
- Must call `next()`
- Use for: logging, parsing, auth
- Chain multiple middleware

---

## Related Topics

- [[What-is-Middleware]] - [[What-is-Middleware|Middleware]]
- [[Use-Middleware]] - [[Use-Middleware|Using middleware]]
- [[Express-Middleware]] - [[Express-Middleware|Express middleware]]
- [[What-is-Express]] - [[What-is-Express|Express]]
