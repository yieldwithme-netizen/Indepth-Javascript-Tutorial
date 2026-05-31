# valueOf

## Definition

valueOf returns the **primitive value** of an object.

## Example

```javascript
const obj = {
    valueOf() {
        return 42;
    }
};

console.log(obj + 8); // 50 (uses valueOf)
console.log(obj.valueOf()); // 42
```

## Quick Revision

- valueOf returns primitive value
- Called for type conversion
- Can be overridden
- Used with operators

---

## Related Topics

- [[toString]] - [[toString|toString]]
- [[valueOf]] - [[valueOf|valueOf]]
- [[What-is-Operator]] - [[What-is-Operator|Operators]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
