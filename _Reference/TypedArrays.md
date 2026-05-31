# Typed Arrays

## Definition

Typed arrays are **array-like objects** for working with binary data.

## Types

```javascript
const buffer = new ArrayBuffer(8);
const view = new Int32Array(buffer);
view[0] = 42;
console.log(view[0]); // 42
```

## Common Types

| Type | Description |
|------|-------------|
| Int8Array | 8-bit signed integer |
| Uint8Array | 8-bit unsigned integer |
| Int16Array | 16-bit signed integer |
| Float32Array | 32-bit float |

## Quick Revision

- Typed arrays for binary data
- ArrayBuffer: raw binary data
- TypedArray: view of buffer
- Use for: WebGL, audio, files

---

## Related Topics

- [[What-is-TypedArrays]] - [[What-is-TypedArrays|Typed arrays]]
- [[TypedArrays]] - [[TypedArrays|Typed arrays]]
- [[ArrayBuffer]] - [[ArrayBuffer|ArrayBuffer]]
- [[Binary-Data]] - [[Binary-Data|Binary data]]
