# Binary Operations

## Definition

Binary operations work with **binary (base-2) numbers**.

## Example

```javascript
// Convert to binary
(42).toString(2);  // "101010"

// Convert from binary
parseInt("101010", 2);  // 42

// Bitwise operators
5 & 3;   // 1 (AND)
5 | 3;   // 7 (OR)
5 ^ 3;   // 6 (XOR)
~5;       // -6 (NOT)
5 << 1;  // 10 (left shift)
5 >> 1;  // 2 (right shift)
```

## Quick Revision

- Convert: `toString(2)`, `parseInt(n, 2)`
- Bitwise: &, |, ^, ~, <<, >>
- Use for: flags, permissions

---

## Related Topics

- [[What-is-Operators]] - [[What-is-Operators|Operators]]
- [[Binary-Operations]] - [[Binary-Operations|Binary operations]]
- [[What-is-Number]] - [[What-is-Number|Numbers]]
