# What is a Map?

## Definition

A Map is a **collection of key-value pairs** where keys can be any type.

## Creating Maps

```javascript
// Empty map
const map = new Map();

// With initial values
const map = new Map([
    ["name", "John"],
    ["age", 30],
    [1, "one"]
]);
```

## Map Methods

```javascript
const map = new Map();

// Set
map.set("name", "John");
map.set("age", 30);
map.set(1, "one");

// Get
map.get("name"); // "John"

// Has
map.has("name"); // true

// Delete
map.delete("age");

// Size
map.size; // 2

// Clear
map.clear();
```

## Iterating Maps

```javascript
const map = new Map([["a", 1], ["b", 2]]);

// forEach
map.forEach((value, key) => {
    console.log(`${key}: ${value}`);
});

// for...of
for (const [key, value] of map) {
    console.log(`${key}: ${value}`);
}

// Keys/Values/Entries
console.log([...map.keys()]);   // ["a", "b"]
console.log([...map.values()]); // [1, 2]
console.log([...map.entries()]); // [["a", 1], ["b", 2]]
```

## Map vs Object

| Feature | Map | Object |
|---------|-----|--------|
| Key types | Any | String/Symbol |
| Size | `.size` | `Object.keys().length` |
| Iteration | Direct | Need to convert |
| Performance | Better for large | Better for small |

## Quick Revision

- Map = key-value pairs with any key type
- Methods: set, get, has, delete, size
- Iterate: forEach, for...of
- Better than objects for many keys
- Preserves insertion order

---

## Related Topics

- [[What-is-Map]] - Map overview
- [[Use-Map]] - Using Map
- [[What-is-Set]] - Set
- [[What-is-Object]] - Objects
- [[Iterate-Objects]] - Object iteration
