# DataView

## Definition

DataView reads/writes **multiple data types** from ArrayBuffer.

## Example

```javascript
const buffer = new ArrayBuffer(16);
const view = new DataView(buffer);

view.setInt8(0, 42);
view.getInt8(0); // 42

view.setFloat32(4, 3.14);
view.getFloat32(4); // 3.14
```

## Quick Revision

- DataView for mixed data types
- Set/get: Int8, Int16, Int32, Float32, Float64
- Works with ArrayBuffer
- Use for: binary protocols

---

## Related Topics

- [[What-is-TypedArrays]] - [[What-is-TypedArrays|Typed arrays]]
- [[DataView]] - [[DataView|DataView]]
- [[ArrayBuffer]] - [[ArrayBuffer|ArrayBuffer]]
- [[TypedArrays]] - [[TypedArrays|Typed arrays]]
