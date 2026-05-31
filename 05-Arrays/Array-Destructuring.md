# Array Destructuring

## Definition

Array destructuring **extracts values** from arrays into variables.

## Basic Syntax

```javascript
const arr = [1, 2, 3];
const [a, b] = arr;
console.log(a, b); // 1, 2
```

## Skip Elements

```javascript
const [a, , c] = [1, 2, 3];
console.log(a, c); // 1, 3
```

## Rest Pattern

```javascript
const [first, ...rest] = [1, 2, 3, 4];
console.log(rest); // [2, 3, 4]
```

## Swap Variables

```javascript
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2, 1
```

## Quick Revision

- Extract array values to variables
- Use `[]` syntax
- Skip with commas
- Rest pattern for remaining

---

## Related Topics

- [[What-is-Destructuring]] - [[What-is-Destructuring|Destructuring]]
- [[Array-Destructuring]] - [[Array-Destructuring|Array destructuring]]
- [[Destructure-Arrays]] - [[Destructure-Arrays|Array destructuring]]
- [[Destructuring-Assignment]] - [[Destructuring-Assignment|Destructuring]]
