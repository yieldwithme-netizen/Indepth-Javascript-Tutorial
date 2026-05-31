# Destructuring Assignment

## Definition

Destructuring **extracts values** from arrays/objects into variables.

## Object Destructuring

```javascript
const person = { name: "John", age: 30 };
const { name, age } = person;

// Rename
const { name: userName } = person;

// Default
const { gender = "male" } = person;
```

## Array Destructuring

```javascript
const arr = [1, 2, 3];
const [a, b] = arr;

// Skip
const [a, , c] = arr;

// Rest
const [a, ...rest] = arr;
```

## Quick Revision

- Extract values to variables
- `{}` for objects, `[]` for arrays
- Supports defaults and renaming
- Use for: assignments, parameters

---

## Related Topics

- [[What-is-Destructuring]] - [[What-is-Destructuring|Destructuring]]
- [[Destructuring-Assignment]] - [[Destructuring-Assignment|Destructuring]]
- [[Destructure-Objects]] - [[Destructure-Objects|Object destructuring]]
- [[Destructure-Arrays]] - [[Destructure-Arrays|Array destructuring]]
