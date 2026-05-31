# of Operator

## Definition

`of` is used in `for...of` loops to **iterate over values**.

## Example

```javascript
const arr = [1, 2, 3];
for (const num of arr) {
    console.log(num); // 1, 2, 3
}

const str = "Hello";
for (const char of str) {
    console.log(char); // H, e, l, l, o
}
```

## Quick Revision

- `for...of` iterates values
- Works with arrays, strings, maps, sets
- Use `for...in` for keys

---

## Related Topics

- [[What-is-ForOf]] - [[What-is-ForOf|For...of]]
- [[Use-ForOf]] - [[Use-ForOf|Using for...of]]
- [[of]] - [[of|of operator]]
- [[What-is-ForIn]] - [[What-is-ForIn|For...in]]
