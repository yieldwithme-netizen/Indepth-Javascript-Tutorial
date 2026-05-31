# What-is-PostgreSQL

## Definition

PostgreSQL is a **relational database** management system.

## Node.js Connection

```javascript
const { Pool } = require('pg');

const pool = new Pool({
    user: 'user',
    host: 'localhost',
    database: 'mydb',
    password: 'password',
    port: 5432,
});

const result = await pool.query('SELECT * FROM users');
```

## Quick Revision

- PostgreSQL = relational database
- Use `pg` package for Node.js
- SQL queries
- ACID compliant

---

## Related Topics

- [[What-is-PostgreSQL]] - [[What-is-PostgreSQL|PostgreSQL]]
- [[What-is-PostgreSQL]] - [[What-is-PostgreSQL|PostgreSQL]]
- [[PostgreSQL]] - [[PostgreSQL|PostgreSQL]]
- [[What-is-MySQL]] - [[What-is-MySQL|MySQL]]
