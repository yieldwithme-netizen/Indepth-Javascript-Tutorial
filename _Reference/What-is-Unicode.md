# What-is-Unicode

## Definition

Unicode is a **standard for encoding characters** from all writing systems.

## JavaScript and Unicode

```javascript
"hello".length; // 5
"😀".length;    // 2 (surrogate pair)

"😀".codePointAt(0); // 128512
String.fromCodePoint(128512); // "😀"
```

## Quick Revision

- Unicode = character encoding standard
- JavaScript uses UTF-16
- `codePointAt()` for code point
- `fromCodePoint()` from code point

---

## Related Topics

- [[What-is-Unicode]] - [[What-is-Unicode|Unicode]]
- [[What-is-Unicode]] - [[What-is-Unicode|Unicode]]
- [[Unicode]] - [[Unicode|Unicode]]
- [[What-is-String]] - [[What-is-String|Strings]]
