# Node.js Security

## Definition

Node.js security **protects applications** from vulnerabilities.

## Best Practices

```javascript
// 1. Never store secrets in code
const dbPassword = process.env.DB_PASSWORD;

// 2. Validate input
const Joi = require('joi');
const schema = Joi.object({ email: Joi.string().email() });

// 3. Use helmet
const helmet = require('helmet');
app.use(helmet());

// 4. Rate limiting
const rateLimit = require('express-rate-limit');
app.use(rateLimit());
```

## Quick Revision

- Never hardcode secrets
- Validate all input
- Use helmet for HTTP headers
- Rate limit requests
- Keep dependencies updated

---

## Related Topics

- [[What-is-Node]] - [[What-is-Node|Node.js]]
- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
- [[Security-Best-Practices]] - [[Security-Best-Practices|Security]]
- [[Node.js-Security]] - [[Node.js-Security|Node.js security]]
