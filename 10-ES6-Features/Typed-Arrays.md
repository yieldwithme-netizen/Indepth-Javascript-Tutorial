# Typed Arrays

## Definition

Typed arrays are **array-like objects** for binary data.

## Types

```javascript
const buffer = new ArrayBuffer(8);
const view = new Int32Array(buffer);
view[0] = 42;
```

## Quick Revision

- Typed arrays for binary data
- ArrayBuffer: raw data
- TypedArray: view of buffer
- Use for: WebGL, audio, files

---

## Related Topics

- [[What-is-TypedArrays]] - [[What-is-TypedArrays|Typed arrays]]
- [[Typed-Arrays]] - [[Typed-Arrays|Typed arrays]]
- [[TypedArrays]] - [[TypedArrays|Typed arrays]]
- [[ArrayBuffer]] - [[ArrayBuffer|ArrayBuffer]]
