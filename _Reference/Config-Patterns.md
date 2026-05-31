# Config Patterns

## Definition

Config patterns are **best practices** for managing configuration.

## Environment Variables

```javascript
// .env
API_KEY=secret123
DATABASE_URL=postgresql://...

// Access
const apiKey = process.env.API_KEY;
```

## Config Objects

```javascript
// config.js
const config = {
    port: process.env.PORT || 3000,
    db: process.env.DATABASE_URL,
    secret: process.env.JWT_SECRET
};

module.exports = config;
```

## Quick Revision

- Use environment variables
- Config files for defaults
- Never commit secrets
- Use .env files

---

## Related Topics

- [[Config-Patterns]] - [[Config-Patterns|Config patterns]]
- [[Environment-Variables]] - [[Environment-Variables|Environment variables]]
- [[What-is-Node]] - [[What-is-Node|Node.js]]
- [[Node.js-Configuration]] - [[Node.js-Configuration|Node config]]
