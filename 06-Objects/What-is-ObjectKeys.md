# What is Object.keys()?

## Definition

`Object.keys()` returns an **array** of an object's keys.

## Syntax

```javascript
Object.keys(obj)
```

## Examples

```javascript
const person = { name: "John", age: 30, city: "NYC" };

const keys = Object.keys(person);
console.log(keys); // ["name", "age", "city"]

// Iterate over keys
for (const key of Object.keys(person)) {
    console.log(`${key}: ${person[key]}`);
}

// Check if property exists
if ("name" in person) {
    console.log("Has name");
}

// Count properties
const count = Object.keys(person).length;
console.log(count); // 3
```

## Object.keys vs for...in

```javascript
const obj = { a: 1, b: 2 };

// Object.keys: only own properties
Object.keys(obj); // ["a", "b"]

// for...in: includes inherited properties
for (let key in obj) {
    console.log(key); // "a", "b"
}
```

## Quick Revision

- `Object.keys(obj)` returns array of keys
- Only includes own properties
- Use for iterating object properties
- Returns empty array for empty objects
- Can use with `Object.values()` and `Object.entries()`

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Get-Keys]] - Getting keys
- [[What-is-ObjectValues]] - Object.values()
- [[What-is-ObjectEntries]] - Object.entries()
- [[Iterate-Objects]] - Object iteration
