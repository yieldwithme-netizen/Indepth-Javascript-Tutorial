# What-is-Indexing (Databases)

## Definition

Database indexing **speeds up data retrieval** by creating lookup structures.

## Example

```javascript
// MongoDB
db.users.createIndex({ email: 1 });

// MySQL
CREATE INDEX idx_email ON users(email);
```

## Quick Revision

- Index = lookup structure
- Speeds up queries
- Uses extra storage
- Index frequently queried fields

---

## Related Topics

- [[What-is-Indexing]] - [[What-is-Indexing|Indexing]]
- [[What-is-MongoDB]] - [[What-is-MongoDB|MongoDB]]
- [[What-is-MySQL]] - [[What-is-MySQL|MySQL]]
