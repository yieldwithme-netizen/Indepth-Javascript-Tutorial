# What is a [[for]]...[[of]] [[loop]]?

## Definition

A [[for]]...[[of]] [[loop]] **iterates over iterable values** ([[array|array]]s, [[string|string]]s, maps, sets).

## Basic Syntax

```javascript
for (let value of iterable) {
    // code using value
}
```

## Examples

```javascript
// Array
const fruits = ["apple", "banana", "orange"];
for (const fruit of fruits) {
    console.log(fruit); // apple, banana, orange
}

// String
const name = "John";
for (const char of name) {
    console.log(char); // J, o, h, n
}

// Map
const map = new Map();
map.set("name", "John");
map.set("age", 30);
for (const [key, value] of map) {
    console.log(`${key}: ${value}`);
}

// Set
const set = new Set([1, 2, 3, 4, 5]);
for (const num of set) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

## [[for]]...[[of]] vs [[for]]...[[in]]

```javascript
// for...of: values
const arr = ["a", "b", "c"];
for (let x of arr) {
    console.log(x); // "a", "b", "c"
}

// for...in: keys/indices
for (let x in arr) {
    console.log(x); // "0", "1", "2"
}
```

## [[break]] and [[continue]] Work

```javascript
// break
const nums = [1, 2, 3, 4, 5];
for (const num of nums) {
    if (num === 3) break;
    console.log(num); // 1, 2
}

// continue
for (const num of nums) {
    if (num === 3) continue;
    console.log(num); // 1, 2, 4, 5
}
```

## When to Use

```javascript
// ✅ Use for...of for:
// - Arrays
// - Strings
// - Maps
// - Sets
// - Any iterable

// ❌ Don't use for...of for:
// - Objects (use for...in or Object.keys/values/entries)
```

## Quick Revision

- [[for]]...[[of]] iterates over values
- Works on [[array|array]]s, [[string|string]]s, maps, sets
- Use [[for]]...[[in]] for [[object|objects]]
- Supports [[break]] and [[continue]]
- Cleaner than traditional [[for]] loop

---

## Related Topics

- [[What-is-ForIn]] - [[for]]...[[in]] loop
- [[What-is-ForLoop]] - Traditional [[for]] loop
- [[Use-ForOf]] - Using [[for]]...[[of]]
- [[Use-ForIn]] - Using [[for]]...[[in]]
