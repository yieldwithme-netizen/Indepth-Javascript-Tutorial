# Nullish Coalescing

## Definition

Nullish coalescing (`??`) returns the **right-hand value** when left is `null` or `undefined`.

## Basic Usage

```javascript
const value = null;
const default1 = value ?? "default"; // "default"

const value2 = 0;
const default2 = value2 ?? "default"; // 0 (not nullish!)

const value3 = "";
const default3 = value3 ?? "default"; // "" (not nullish!)
```

## ?? vs ||

```javascript
// || returns right if LEFT is falsy
const a = 0 || "default";  // "default"
const b = "" || "default"; // "default"

// ?? returns right if LEFT is null/undefined
const c = 0 ?? "default";  // 0
const d = "" ?? "default"; // ""
```

## Quick Revision

- `??` = nullish coalescing
- Returns right if left is null/undefined
- `||` returns right if left is falsy
- `??` is better for defaults

---

## Related Topics

- [[What-is-Nullish]] - [[What-is-Nullish|Nullish coalescing]]
- [[Nullish-Coalescing]] - [[Nullish-Coalescing|Nullish coalescing]]
- [[Use-Nullish]] - [[Use-Nullish|Using nullish coalescing]]
- [[What-is-OptionalChaining]] - [[What-is-OptionalChaining|Optional chaining]]
