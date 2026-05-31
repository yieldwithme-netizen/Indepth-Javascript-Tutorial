# What-is-Substring

## Definition

Substring extracts a **portion of a string**.

## Examples

```javascript
const str = "Hello World";

// slice
str.slice(0, 5);    // "Hello"
str.slice(6);       // "World"
str.slice(-5);      // "World"

// substring
str.substring(0, 5); // "Hello"

// substr (deprecated)
str.substr(0, 5);   // "Hello"
```

## Quick Revision

- `slice(start, end)` - extracts portion
- `substring(start, end)` - similar to slice
- `substr(start, length)` - deprecated
- Negative index in slice from end

---

## Related Topics

- [[What-is-String]] - [[What-is-String|Strings]]
- [[Extract-Substring]] - [[Extract-Substring|Extracting substrings]]
- [[What-is-Substring]] - [[What-is-Substring|Substring]]
- [[What-is-Slice]] - [[What-is-Slice|Slice]]
