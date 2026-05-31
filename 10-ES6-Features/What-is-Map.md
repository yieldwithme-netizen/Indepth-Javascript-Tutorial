# What is Map Object

## Definition
A `Map` is a collection of key-value pairs where keys can be of any type (objects, functions, primitives). Unlike plain objects, Maps preserve insertion order and provide efficient size lookup.

## Key Differences: Map vs Object

| Feature | Map | Object |
|---------|-----|--------|
| Key types | Any type | Strings/Symbols only |
| Size | `.size` property | Manual calculation |
| Order | Insertion order | Not guaranteed |
| Performance | Better for frequent add/remove | Better for static data |
| Iteration | `.keys()`, `.values()`, `.entries()` | `Object.keys()`, etc. |

## Creating a Map

```javascript
// Empty Map
const map1 = new Map();

// From array of key-value pairs
const map2 = new Map([
  ["name", "John"],
  ["age", 30],
  ["city", "NYC"]
]);

// From another Map
const map3 = new Map(map2);
```

## Map Key Types

```javascript
const map = new Map();

// String keys
map.set("stringKey", "value1");

// Number keys
map.set(42, "value2");

// Boolean keys
map.set(true, "value3");

// Object keys
const objKey = { id: 1 };
map.set(objKey, "value4");

// Function keys
const fnKey = () => {};
map.set(fnKey, "value5");

// Maps can even use other Maps as keys
const innerMap = new Map();
innerMap.set("nested", "value6");
map.set(innerMap, "nested map value");
```

## Map Properties

```javascript
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3]
]);

// Number of entries
console.log(map.size);  // 3
```

## When to Use Map Over Object

```javascript
// Object: Simple key-value for known structure
const user = {
  name: "John",
  age: 30
};

// Map: Dynamic keys or frequent modifications
const cache = new Map();
cache.set(user, { lastAccess: Date.now() });
cache.set("api:users", userData);

// Map for counting
const wordCount = new Map();
const words = "hello world hello world hello".split(" ");
words.forEach(word => {
  wordCount.set(word, (wordCount.get(word) || 0) + 1);
});
```

## Common Use Cases

### Lookup Table

```javascript
const statusMessages = new Map([
  [200, "OK"],
  [400, "Bad Request"],
  [404, "Not Found"],
  [500, "Server Error"]
]);

const message = statusMessages.get(404);  // "Not Found"
```

### Caching

```javascript
const cache = new Map();

function getCached(key, computeFn) {
  if (cache.has(key)) {
    return cache.get(key);
  }
  const result = computeFn();
  cache.set(key, result);
  return result;
}
```

## Common Mistakes

```javascript
// Wrong: Using Map as plain object
const map = new Map();
map.key = "value";  // This creates a regular property, not a Map entry

// Correct: Use .set()
map.set("key", "value");

// Wrong: Comparing Maps for equality
const map1 = new Map([["a", 1]]);
const map2 = new Map([["a", 1]]);
console.log(map1 === map2);  // false (different references)

// Wrong: Using object as key without reference
const map = new Map();
const key = { id: 1 };
map.set(key, "value");
console.log(map.get({ id: 1 }));  // undefined (different object)
```

## Quick Revision

- `Map` stores key-value pairs with any type keys
- Use `.set()` to add, `.get()` to retrieve, `.has()` to check
- `.size` gives the number of entries
- Better than objects for dynamic keys or frequent modifications
- Preserves insertion order during iteration

## Related Topics

- [[Use-Map]] - Practical Map methods
- [[What-is-Set]] - Set data structure
- [[What-is-WeakMap]] - WeakMap reference collection
- [[Iterate-Map]] - Iterating over Maps
