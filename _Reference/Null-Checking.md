# Null Checking

## Definition

Null checking verifies if a value is **null or undefined**.

## Methods

```javascript
// Strict equality
if (value === null) { }
if (value === undefined) { }

// Loose equality (catches both)
if (value == null) { }

// Optional chaining
const name = user?.name;

// Nullish coalescing
const name = user?.name ?? "Guest";
```

## Quick Revision

- `=== null` for null only
- `=== undefined` for undefined only
- `== null` for both
- Optional chaining for safe access
- Nullish coalescing for defaults

---

## Related Topics

- [[What-is-Null]] - [[What-is-Null|null]]
- [[What-is-Undefined]] - [[What-is-Undefined|undefined]]
- [[Null-Checking]] - [[Null-Checking|Null checking]]
- [[What-is-OptionalChaining]] - [[What-is-OptionalChaining|Optional chaining]]
- [[What-is-Nullish]] - [[What-is-Nullish|Nullish coalescing]]
