# Iterating Maps in JavaScript

## Definition

A `Map` is a collection of key-value pairs where keys can be any data type. Iterating over Maps involves using built-in methods to access and process each entry.

## Creating a Map

```javascript
// Empty Map
const map1 = new Map();

// Map with initial values
const map2 = new Map([
  ["name", "John"],
  ["age", 30],
  ["city", "New York"]
]);

// Adding entries
const map3 = new Map();
map3.set("key1", "value1");
map3.set("key2", "value2");
map3.set(42, "numeric key");
map3.set(true, "boolean key");
```

## Iteration Methods

### Using `forEach()`

```javascript
const userRoles = new Map([
  ["admin", "Full Access"],
  ["editor", "Edit Posts"],
  ["viewer", "Read Only"]
]);

userRoles.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});
// Output:
// admin: Full Access
// editor: Edit Posts
// viewer: Read Only
```

### Using `for...of` Loop

```javascript
const fruitPrices = new Map([
  ["apple", 1.50],
  ["banana", 0.75],
  ["cherry", 3.00]
]);

// Iterate over entries
for (const [fruit, price] of fruitPrices) {
  console.log(`${fruit} costs $${price}`);
}

// Iterate over keys only
for (const fruit of fruitPrices.keys()) {
  console.log(fruit);
}

// Iterate over values only
for (const price of fruitPrices.values()) {
  console.log(price);
}
```

### Using `entries()`

```javascript
const config = new Map([
  ["debug", true],
  ["version", "1.0.0"],
  ["apiUrl", "https://api.example.com"]
]);

for (const [key, value] of config.entries()) {
  console.log(`${key} = ${value}`);
}
```

### Converting to Array

```javascript
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3]
]);

// Convert to array of entries
const entries = [...map];
console.log(entries); // [["a", 1], ["b", 2], ["c", 3]]

// Convert to array of keys
const keys = [...map.keys()];
console.log(keys); // ["a", "b", "c"]

// Convert to array of values
const values = [...map.values()];
console.log(values); // [1, 2, 3]
```

## Common Use Cases

- Storing unique key-value pairs with non-string keys
- Caching and memoization
- Counting occurrences
- Storing relationships between objects
- Implementing lookup tables

## Common Mistakes

1. **Using object methods on Map**: Maps don't have `length` or array methods directly

```javascript
const map = new Map([["a", 1]]);

// Wrong
// map.length; // undefined
// map.map();  // not a function

// Correct
console.log(map.size); // 1
```

2. **Forgetting `set()` returns the Map**: Can chain calls

```javascript
const map = new Map();
map.set("a", 1).set("b", 2).set("c", 3); // Valid chaining
```

3. **Confusing with plain objects**: Maps maintain insertion order and support any key type

## Related Topics

- [[Map-Object]] - Map data structure
- [[Set-Object]] - Set data structure
- [[Object-Entries]] - Similar iteration for plain objects
- [[WeakMap]] - Weakly referenced Map
- [[Data-Structures]] - Common data structures

## Quick Revision Summary

| Method | Description |
|--------|-------------|
| `forEach()` | Executes callback for each entry |
| `for...of` | Iterates over entries by default |
| `.keys()` | Returns iterator of keys |
| `.values()` | Returns iterator of values |
| `.entries()` | Returns iterator of [key, value] pairs |
| `[...map]` | Spreads into array of entries |
| `size` | Property for number of entries |
