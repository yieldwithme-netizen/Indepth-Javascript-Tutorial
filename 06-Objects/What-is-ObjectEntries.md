# What is Object.entries()?

## Definition

`Object.entries()` returns an **array** of key-value pairs.

## Syntax

```javascript
Object.entries(obj)
```

## Examples

```javascript
const person = { name: "John", age: 30, city: "NYC" };

const entries = Object.entries(person);
console.log(entries); // [["name", "John"], ["age", 30], ["city", "NYC"]]

// Iterate over entries
for (const [key, value] of Object.entries(person)) {
    console.log(`${key}: ${value}`);
}

// Convert to Map
const map = new Map(Object.entries(person));

// Filter entries
const filtered = Object.entries(person)
    .filter(([key, value]) => typeof value === "string");
console.log(filtered); // [["name", "John"], ["city", "NYC"]]

// Transform entries
const transformed = Object.entries(person)
    .map(([key, value]) => `${key}: ${value}`);
console.log(transformed); // ["name: John", "age: 30", "city: NYC"]
```

## Quick Revision

- `Object.entries(obj)` returns `[key, value]` pairs
- Use [[Destructuring]] to access: `[key, value]`
- Can convert to Map
- Use with array methods (filter, map)
- Great for iterating objects

---

## Related Topics

- [[What-is-Object]] - Objects overview
- [[Iterate-Objects]] - Object iteration
- [[What-is-ObjectKeys]] - Object.keys()
- [[What-is-ObjectValues]] - Object.values()
- [[Access-Properties]] - Accessing properties
