# What is Object.values()?

## Definition

`Object.values()` returns an **array** of an object's values.

## Syntax

```javascript
Object.values(obj)
```

## Examples

```javascript
const person = { name: "John", age: 30, city: "NYC" };

const values = Object.values(person);
console.log(values); // ["John", 30, "NYC"]

// Get all names
const names = users.map(user => Object.values(user)[0]);

// Sum all values
const scores = { math: 90, english: 85, science: 95 };
const total = Object.values(scores).reduce((sum, score) => sum + score, 0);
console.log(total); // 270
```

## Quick Revision

- `Object.values(obj)` returns array of values
- Only includes own properties
- Use for getting all values
- Returns empty array for empty objects
- Works with any object

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Get-Values]] - Getting values
- [[What-is-ObjectKeys]] - Object.keys()
- [[What-is-ObjectEntries]] - Object.entries()
- [[Iterate-Objects]] - Object iteration
