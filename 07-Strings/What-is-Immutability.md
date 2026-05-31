# What-is-Immutability (Strings)

## Definition

String immutability means strings **cannot be changed** after creation.

## Example

```javascript
const str = "Hello";
str[0] = "J"; // No error, but doesn't change
console.log(str); // "Hello"

// Must create new string
const newStr = "J" + str.slice(1);
console.log(newStr); // "Jello"
```

## Quick Revision

- Strings are immutable
- Methods return new strings
- Original unchanged
- Use concatenation/slice for changes

---

## Related Topics

- [[What-is-Immutability]] - [[What-is-Immutability|Immutability]]
- [[What-is-String]] - [[What-is-String|Strings]]
- [[What-is-Primitive]] - [[What-is-Primitive|Primitives]]
