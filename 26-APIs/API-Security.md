# API Security

## Definition

API security **protects APIs** from attacks and unauthorized access.

## Best Practices

```javascript
// Authentication
app.use('/api', authenticate);

// Rate limiting
const rateLimit = require('express-rate-limit');
app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));

// Input validation
const Joi = require('joi');
const schema = Joi.object({ name: Joi.string().required() });
```

## Quick Revision

- Authenticate all requests
- Rate limit to prevent abuse
- Validate input
- Use HTTPS
- Never expose secrets

---

## Related Topics

- [[What-is-API]] - [[What-is-API|APIs]]
- [[API-Security]] - [[API-Security|API security]]
- [[What-is-Authentication]] - [[What-is-Authentication|Authentication]]
- [[What-is-RateLimit]] - [[What-is-RateLimit|Rate limiting]]
