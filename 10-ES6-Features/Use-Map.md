# How to Use Map Object

## Definition
A `Map` is a collection of key-value pairs with efficient lookups and a maintained insertion order. Use Maps when you need keys of any type or frequent additions/removals.

## Creating Maps

```javascript
// Empty map
const map = new Map();

// From array
const map2 = new Map([
  ["name", "John"],
  ["age", 30]
]);

// From another map
const map3 = new Map(map2);
```

## Map Methods

### Set and Get

```javascript
const users = new Map();

users.set("john@email.com", { name: "John", role: "Admin" });
users.set("jane@email.com", { name: "Jane", role: "User" });

// Get values
const john = users.get("john@email.com");
console.log(john);  // { name: "John", role: "Admin" }

// Overwrite existing key
users.set("john@email.com", { name: "John", role: "Super Admin" });
```

### Has and Delete

```javascript
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3]
]);

// Check if key exists
console.log(map.has("a"));  // true
console.log(map.has("d"));  // false

// Delete a key
map.delete("b");
console.log(map.has("b"));  // false
console.log(map.size);      // 2
```

### Clear

```javascript
const map = new Map([["a", 1], ["b", 2]]);
map.clear();
console.log(map.size);  // 0
```

## Iterating Maps

```javascript
const map = new Map([
  ["x", 1],
  ["y", 2],
  ["z", 3]
]);

// Iterate over entries
for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}

// Iterate keys only
for (const key of map.keys()) {
  console.log(key);  // "x", "y", "z"
}

// Iterate values only
for (const value of map.values()) {
  console.log(value);  // 1, 2, 3
}

// forEach
map.forEach((value, key) => {
  console.log(`${key} => ${value}`);
});
```

## Converting Between Map and Object

```javascript
// Map to Object
const map = new Map([["a", 1], ["b", 2], ["c", 3]]);
const obj = Object.fromEntries(map);
console.log(obj);  // { a: 1, b: 2, c: 3 }

// Object to Map
const obj2 = { x: 10, y: 20 };
const map2 = new Map(Object.entries(obj2));
```

## Converting Between Map and Array

```javascript
const map = new Map([["a", 1], ["b", 2]]);

// Map to Array
const arr = [...map];
console.log(arr);  // [["a", 1], ["b", 2]]

// Array to Map
const map2 = new Map(arr);
```

## Common Use Cases

### Frequency Counter

```javascript
function countWords(text) {
  const words = text.toLowerCase().split(/\s+/);
  const frequencies = new Map();

  words.forEach(word => {
    frequencies.set(word, (frequencies.get(word) || 0) + 1);
  });

  return frequencies;
}

const freq = countWords("hello world hello");
console.log(freq.get("hello"));  // 2
```

### Caching

```javascript
function createCache(compute) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = compute.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const expensiveCalc = createCache(n => {
  console.log("Computing...");
  return n * n;
});

expensiveCalc(5);  // "Computing..." 25
expensiveCalc(5);  // 25 (from cache)
```

### Object as Key Reference Storage

```javascript
const userData = new Map();

const user1 = { id: 1, name: "John" };
const user2 = { id: 2, name: "Jane" };

userData.set(user1, { lastLogin: "2026-05-30", sessions: 5 });
userData.set(user2, { lastLogin: "2026-05-29", sessions: 3 });

// Later, find user's data
const johnData = userData.get(user1);
```

## Common Mistakes

```javascript
// Wrong: Using Map as object
const map = new Map();
map.key = "value";  // Regular property, not Map entry

// Correct
map.set("key", "value");

// Wrong: Comparing objects as keys
const map2 = new Map();
const key = { a: 1 };
map2.set(key, "value");
map2.get({ a: 1 });  // undefined (different object reference)

// Correct: Keep reference to key
const keyObj = { a: 1 };
map2.set(keyObj, "value");
map2.get(keyObj);  // "value" (same reference)

// Wrong: Using .size on non-Map
const notMap = { a: 1, b: 2 };
console.log(notMap.size);  // undefined
```

## Quick Revision

- `new Map()` creates empty map
- `.set(key, value)` adds entries, `.get(key)` retrieves
- `.has(key)` checks existence, `.delete(key)` removes
- `.size` gives number of entries
- Iterate with `for...of`, `.forEach()`, `.keys()`, `.values()`
- Use `Object.entries()` to convert objects to Maps

## Related Topics

- [[What-is-Map]] - Map object overview
- [[What-is-Set]] - Set data structure
- [[Iterate-Map]] - Advanced iteration patterns
- [[Use-WeakMap]] - WeakMap for private data
