# What is Nullish Coalescing?

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
// || returns right side if LEFT is falsy
const a = 0 || "default";  // "default" (0 is falsy!)
const b = "" || "default"; // "default" ("" is falsy!)

// ?? returns right side if LEFT is null/undefined
const c = 0 ?? "default";  // 0 (0 is not null/undefined)
const d = "" ?? "default"; // "" ("" is not null/undefined)
```

## Common Use Cases

```javascript
// Default values
const port = config.port ?? 3000;

// Function parameters
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

// Short-circuit
const result = value ?? defaultValue;
```

## Quick Revision

- `??` = nullish coalescing
- Returns right if left is null/undefined
- `||` returns right if left is falsy
- `??` is better for defaults (0, "" are valid)
- Use for: default values, config

---

## Related Topics

- [[What-is-Nullish]] - Nullish coalescing overview
- [[Use-Nullish]] - Using nullish coalescing
- [[What-is-OptionalChaining]] - Optional chaining
- [[What-is-Null]] - null
- [[What-is-Undefined]] - undefined
