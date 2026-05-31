# Object.defineProperty

## Definition

Object.defineProperty **defines a property** with control over its behavior.

## Example

```javascript
const obj = {};

Object.defineProperty(obj, 'name', {
    value: 'John',
    writable: false,     // can't change
    enumerable: true,    // shows in loops
    configurable: false  // can't delete
});

obj.name = "Jane"; // Error in strict mode
```

## Quick Revision

- Define property with descriptors
- writable: can change value
- enumerable: shows in for...in
- configurable: can delete/reconfigure

---

## Related Topics

- [[What-is-Property]] - [[What-is-Property|Properties]]
- [[ObjectDefineProperty]] - [[ObjectDefineProperty|Object.defineProperty]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
- [[What-is-Descriptor]] - [[What-is-Descriptor|Descriptors]]
