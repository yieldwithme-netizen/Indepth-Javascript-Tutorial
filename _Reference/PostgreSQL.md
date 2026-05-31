# PostgreSQL

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

// Query
const result = await pool.query('SELECT * FROM users');
```

## Quick Revision

- PostgreSQL = relational database
- Use `pg` package for Node.js
- SQL queries
- Strong ACID compliance

---

## Related Topics

- [[What-is-PostgreSQL]] - [[What-is-PostgreSQL|PostgreSQL]]
- [[PostgreSQL]] - [[PostgreSQL|PostgreSQL]]
- [[Connect-MySQL]] - [[Connect-MySQL|Database connection]]
- [[What-is-SQL-vs-NoSQL]] - [[What-is-SQL-vs-NoSQL|SQL vs NoSQL]]
