# Test Regex

## Definition

Testing regular expressions checks if a **pattern matches** a string.

## Methods

```javascript
// test() - returns boolean
const pattern = /hello/i;
pattern.test("Hello World"); // true
pattern.test("Hi World"); // false

// match() - returns match array or null
"Hello World".match(/hello/i); // ["Hello"]
"Hi World".match(/hello/i); // null

// matchAll() - returns iterator
"test1test2".matchAll(/test/g); // Iterator with matches
```

## Common Patterns

```javascript
// Email validation
/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test("user@example.com"); // true

// Phone number
 /^\d{3}-\d{3}-\d{4}$/.test("123-456-7890"); // true

// URL
/^https?:\/\/.+\..+$/.test("https://example.com"); // true

// Strong password
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test("Pass1234"); // true
```

## Quick Revision

- `test()` returns true/false
- `match()` returns array or null
- `matchAll()` returns iterator
- Use for validation and search

---

## Related Topics

- [[What-is-Regex]] - [[What-is-Regex|Regex]] overview
- [[Create-Regex]] - [[Create-Regex|Creating regex]]
- [[Use-RegexFlags]] - [[Use-RegexFlags|Regex flags]]
- [[Match-Patterns]] - [[Match-Patterns|Matching patterns]]
