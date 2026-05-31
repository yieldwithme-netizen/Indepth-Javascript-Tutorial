# Slice

## Definition

`slice()` extracts a **portion of an array or string** without modifying the original.

## Array Slice

```javascript
const arr = [1, 2, 3, 4, 5];

arr.slice(1, 3);    // [2, 3]
arr.slice(2);       // [3, 4, 5]
arr.slice(-2);      // [4, 5]
arr.slice();        // [1, 2, 3, 4, 5] (copy)
```

## String Slice

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
- Negative index: from end

---

## Related Topics

- [[What-is-Slice]] - [[What-is-Slice|Slice]]
- [[slice]] - [[slice|slice]]
- [[Use-Slice]] - [[Use-Slice|Using slice]]
- [[What-is-Splice]] - [[What-is-Splice|Splice]]
- [[What-is-String]] - [[What-is-String|Strings]]
