# Unicode

## Definition

Unicode is a **standard for encoding characters** from all writing systems.

## JavaScript and Unicode

```javascript
// String length (UTF-16 units)
"hello".length; // 5
"😀".length;    // 2 (surrogate pair)

// Code point
"😀".codePointAt(0); // 128512

// From code point
String.fromCodePoint(128512); // "😀"

// Iterate properly
[..."hello"]; // ["h", "e", "l", "l", "o"]
[..."😀"];    // ["😀"]
```

## Quick Revision

- Unicode = character encoding standard
- JavaScript uses UTF-16
- `codePointAt()` for code point
- `fromCodePoint()` from code point
- Spread for proper iteration

---

## Related Topics

- [[What-is-Unicode]] - [[What-is-Unicode|Unicode]]
- [[Unicode]] - [[Unicode|Unicode]]
- [[What-is-String]] - [[What-is-String|Strings]]
- [[Encoding]] - [[Encoding|Encoding]]
