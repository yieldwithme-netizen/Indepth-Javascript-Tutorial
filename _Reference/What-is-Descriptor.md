# What-is-Descriptor

## Definition

Descriptors define **property behavior**.

## Example

```javascript
const obj = {};

Object.defineProperty(obj, 'name', {
    value: 'John',
    writable: false,
    enumerable: true,
    configurable: false
});
```

## Quick Revision

- Descriptors control property behavior
- writable: can change value
- enumerable: shows in loops
- configurable: can delete/reconfigure

---

## Related Topics

- [[What-is-Descriptor]] - [[What-is-Descriptor|Descriptors]]
- [[What-is-Descriptor]] - [[What-is-Descriptor|Descriptors]]
- [[ObjectDefineProperty]] - [[ObjectDefineProperty|Object.defineProperty]]
- [[What-is-Property]] - [[What-is-Property|Properties]]
