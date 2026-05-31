# immutable

## Definition

Immutable means **cannot be changed** after creation.

## Examples

```javascript
// Strings are immutable
const str = "Hello";
str[0] = "J"; // No change
console.log(str); // "Hello"

// Use Object.freeze for objects
const obj = Object.freeze({ name: "John" });
obj.name = "Jane"; // No change
```

## Quick Revision

- Immutable = unchangeable
- Strings are immutable
- Use Object.freeze() for objects
- Use spread for updates

---

## Related Topics

- [[What-is-Immutability]] - [[What-is-Immutability|Immutability]]
- [[immutable]] - [[immutable|immutable]]
- [[mutable]] - [[mutable|mutable]]
- [[Object-Freeze]] - [[Object-Freeze|Object.freeze]]
