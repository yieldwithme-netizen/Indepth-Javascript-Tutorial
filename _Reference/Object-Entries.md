# Object Entries

## Definition

`Object.entries()` returns an **array of key-value pairs** from an object.

## Basic Usage

```javascript
const person = { name: "John", age: 30 };

const entries = Object.entries(person);
console.log(entries); // [["name", "John"], ["age", 30]]
```

## Iterating

```javascript
for (const [key, value] of Object.entries(person)) {
    console.log(`${key}: ${value}`);
}
```

## Transformations

```javascript
// Filter entries
const filtered = Object.entries(person)
    .filter(([key, value]) => typeof value === "string");

// Convert to Map
const map = new Map(Object.entries(person));

// From entries
const obj = Object.fromEntries([["a", 1], ["b", 2]]);
```

## Quick Revision

- `Object.entries()` returns `[key, value]` pairs
- Use destructuring to access
- Can convert to Map
- Use with array methods

---

## Related Topics

- [[What-is-ObjectEntries]] - [[What-is-ObjectEntries|Object.entries()]]
- [[Object-Entries]] - [[Object-Entries|Object entries]]
- [[What-is-ObjectKeys]] - [[What-is-ObjectKeys|Object.keys()]]
- [[What-is-ObjectValues]] - [[What-is-ObjectValues|Object.values()]]
