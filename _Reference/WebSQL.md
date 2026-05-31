# WebSQL

## Definition

WebSQL is a **relational database** in the browser (deprecated).

## Example

```javascript
const db = openDatabase('mydb', '1.0', 'My Database', 2 * 1024 * 1024);

db.transaction((tx) => {
    tx.executeSql('CREATE TABLE IF NOT EXISTS users (id, name)');
    tx.executeSql('INSERT INTO users (id, name) VALUES (1, "John")');
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
- [[WebSQL]] - [[WebSQL|WebSQL]]
- [[IndexedDB]] - [[IndexedDB|IndexedDB]]
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
