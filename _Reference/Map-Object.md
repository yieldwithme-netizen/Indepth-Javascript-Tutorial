# Map Object

## Definition

Map is a **collection of key-value pairs** where keys can be any type.

## Basic Usage

```javascript
const map = new Map();

// Set
map.set("name", "John");
map.set("age", 30);

// Get
map.get("name"); // "John"

// Has
map.has("name"); // true

// Delete
map.delete("age");

// Size
map.size; // 1
```

## Iterating

```javascript
for (const [key, value] of map) {
    console.log(`${key}: ${value}`);
}

// Keys/Values/Entries
console.log([...map.keys()]);
console.log([...map.values()]);
console.log([...map.entries()]);
```

## Quick Revision

- Map = key-value pairs with any key type
- Methods: set, get, has, delete, size
- Iterate: forEach, for...of
- Better than objects for many keys

---

## Related Topics

- [[What-is-Map]] - [[What-is-Map|Map]]
- [[Map-Object]] - [[Map-Object|Map object]]
- [[Use-Map]] - [[Use-Map|Using Map]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
