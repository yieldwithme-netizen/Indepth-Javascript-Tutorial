# What-is-Slice (Strings)

## Definition

`slice()` extracts a **portion of a string** without modifying it.

## Syntax

```javascript
string.slice(start, end)
```

## Examples

```javascript
const str = "Hello World";
str.slice(0, 5);    // "Hello"
str.slice(6);       // "World"
str.slice(-5);      // "World"
```

## Quick Revision

- `slice(start, end)` extracts portion
- Doesn't modify original
- `end` is exclusive
- Negative index from end

---

## Related Topics

- [[What-is-Slice]] - [[What-is-Slice|Slice overview]]
- [[Use-Slice]] - [[Use-Slice|Using slice]]
- [[What-is-String]] - [[What-is-String|Strings]]
