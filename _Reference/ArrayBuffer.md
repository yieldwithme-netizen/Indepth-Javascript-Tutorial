# ArrayBuffer

## Definition
An `ArrayBuffer` is a fixed-length binary data buffer used to represent a generic raw binary data buffer. It's used for working with binary data, often in combination with typed arrays and data views.

## Syntax
```javascript
const buffer = new ArrayBuffer(byteLength);
```

## Code Examples

### Creating ArrayBuffer
```javascript
const buffer = new ArrayBuffer(8);
console.log(buffer.byteLength); // 8
```

### Using Typed Arrays
```javascript
const buffer = new ArrayBuffer(16);
const int32View = new Int32Array(buffer);

int32View[0] = 42;
int32View[1] = 100;

console.log(int32View[0]); // 42
console.log(int32View[1]); // 100
```

### Multiple Views on Same Buffer
```javascript
const buffer = new ArrayBuffer(8);
const int8View = new Int8Array(buffer);
const float64View = new Float64Array(buffer);

int8View[0] = 10;
console.log(float64View[0]); // 2.5e-323 (bits interpreted as float64)
```

### DataView for Flexible Access
```javascript
const buffer = new ArrayBuffer(16);
const view = new DataView(buffer);

view.setInt8(0, 42);
view.setInt16(2, 1000);
view.setFloat32(4, 3.14);

console.log(view.getInt8(0));     // 42
console.log(view.getInt16(2));    // 1000
console.log(view.getFloat32(4));  // 3.140000104904175
```

### Copying ArrayBuffer
```javascript
const buffer1 = new ArrayBuffer(8);
const view1 = new Int32Array(buffer1);
view1[0] = 42;

const buffer2 = buffer1.slice(0);
const view2 = new Int32Array(buffer2);

view2[0] = 100;
console.log(view1[0]); // 42 (original unchanged)
```

### Converting to/from Other Formats
```javascript
// From base64
const binaryString = atob('SGVsbG8=');
const bytes = new Uint8Array(binaryString.length);
for (let i = 0; i < binaryString.length; i++) {
  bytes[i] = binaryString.charCodeAt(i);
}
const buffer = bytes.buffer;

// To base64
const base64 = btoa(String.fromCharCode(...new Uint8Array(buffer)));
```

## Typed Array Types
- `Int8Array`, `Uint8Array`, `Uint8ClampedArray`
- `Int16Array`, `Uint16Array`
- `Int32Array`, `Uint32Array`
- `Float32Array`, `Float64Array`
- `BigInt64Array`, `BigUint64Array`

## Common Use Cases
- Processing binary data from APIs
- Image manipulation
- Audio/video processing
- WebSocket binary data
- File I/O operations
- WebGL data

## Common Mistakes
- **Not using correct view type**: Ensure the typed array matches your data type
- **Byte order issues**: Consider endianness with DataView
- **Buffer detachment**: ArrayBuffer becomes detached after `transfer()`
- **Memory management**: Large buffers should be manually freed

## Related Topics
- [[Typed-Arrays]]
- [[DataView]]
- [[Blob]]
- [[File-API]]
- [[Binary-Data]]

## Quick Revision
- `ArrayBuffer` is a fixed-length binary data buffer
- Use typed arrays (`Int32Array`, etc.) for typed access
- `DataView` provides flexible multi-type access
- Essential for binary data processing and Web APIs
