# Database Connections

## Definition

Database connections **establish links** to databases.

## MongoDB

```javascript
const { MongoClient } = require('mongodb');
const client = new MongoClient(uri);
await client.connect();
```

## PostgreSQL

```javascript
const { Pool } = require('pg');
const pool = new Pool({ connectionString: uri });
```

## Quick Revision

- Use connection pools
- Handle connection errors
- Close connections when done
- Use environment variables for URIs

---

## Related Topics

- [[What-is-MongoDB]] - [[What-is-MongoDB|MongoDB]]
- [[What-is-MySQL]] - [[What-is-MySQL|MySQL]]
- [[Connect-Mongo]] - [[Connect-Mongo|Connecting to MongoDB]]
- [[Database-Connections]] - [[Database-Connections|Database connections]]
