# For-In Loop

## Definition

`for...in` **iterates over object keys/indices**.

## Example

```javascript
const person = { name: "John", age: 30 };
for (const key in person) {
    console.log(`${key}: ${person[key]}`);
}
```

## Quick Revision

- `for...in` iterates keys
- Use `obj[key]` for values
- Use `hasOwnProperty()` to filter
- Don't use for arrays

---

## Related Topics

- [[What-is-ForIn]] - [[What-is-ForIn|For...in]]
- [[Use-ForIn]] - [[Use-ForIn|Using for...in]]
- [[For-In-Loop]] - [[For-In-Loop|For-in loop]]
- [[What-is-ForOf]] - [[What-is-ForOf|For...of]]
