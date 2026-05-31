# What-is-WebSQL

## Definition

WebSQL is a **relational database** in browsers (deprecated).

## Example

```javascript
const db = openDatabase('mydb', '1.0', 'My DB', 2 * 1024 * 1024);
db.transaction(tx => {
    tx.executeSql('CREATE TABLE IF NOT EXISTS users (id, name)');
});
```

## Quick Revision

- WebSQL = browser database (deprecated)
- Use IndexedDB instead
- SQL-based
- Limited support

---

## Related Topics

- [[What-is-WebSQL]] - [[What-is-WebSQL|WebSQL]]
- [[What-is-WebSQL]] - [[What-is-WebSQL|WebSQL]]
- [[WebSQL]] - [[WebSQL|WebSQL]]
- [[IndexedDB]] - [[IndexedDB|IndexedDB]]
