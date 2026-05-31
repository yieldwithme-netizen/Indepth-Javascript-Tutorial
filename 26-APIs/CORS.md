# CORS

## Definition

CORS (Cross-Origin Resource Sharing) **controls cross-origin requests**.

## Express Setup

```javascript
const cors = require('cors');
app.use(cors());

// Or specific origin
app.use(cors({
    origin: 'https://example.com'
}));
```

## Quick Revision

- CORS = cross-origin security
- Blocks requests from different origins
- Use CORS middleware
- Configure allowed origins

---

## Related Topics

- [[What-is-CORS]] - [[What-is-CORS|CORS]]
- [[CORS]] - [[CORS|CORS]]
- [[Handle-CORS]] - [[Handle-CORS|Handling CORS]]
- [[What-is-API]] - [[What-is-API|APIs]]
