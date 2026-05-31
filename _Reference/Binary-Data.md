# Binary Data

## Definition

Binary data in JavaScript refers to data represented as sequences of bits (0s and 1s). JavaScript provides several ways to work with binary data, including `ArrayBuffer`, `TypedArrays`, `DataView`, and `Blob`. These are essential for handling low-level data operations, file I/O, network protocols, and working with binary formats like images, audio, and video.

## Code Examples

### ArrayBuffer

```javascript
// Create an ArrayBuffer (fixed-size raw memory buffer)
const buffer = new ArrayBuffer(8); // 8 bytes

// Access underlying memory via TypedArray
const int32View = new Int32Array(buffer);
int32View[0] = 42;
int32View[1] = 100;

console.log(int32View); // Int32Array [42, 100]
console.log(buffer.byteLength); // 8
```

### Typed Arrays

```javascript
// Int8Array - signed 8-bit integers
const int8 = new Int8Array([1, -2, 3, -4]);
console.log(int8); // Int8Array [1, -2, 3, -4]

// Uint8Array - unsigned 8-bit integers
const uint8 = new Uint8Array([0, 127, 255]);
console.log(uint8); // Uint8Array [0, 127, 255]

// Float32Array - 32-bit floating point
const float32 = new Float32Array([1.5, 2.7, 3.14]);
console.log(float32); // Float32Array [1.5, 2.700000047683716, 3.140000104904175]

// Float64Array - 64-bit floating point
const float64 = new Float64Array([1.5, 2.7, 3.14]);
console.log(float64); // Float64Array [1.5, 2.7, 3.14]
```

### Creating Typed Arrays

```javascript
// From array
const arr = new Uint8Array([1, 2, 3, 4, 5]);

// From buffer
const buffer = new ArrayBuffer(16);
const view = new Uint8Array(buffer);

// From length
const empty = new Uint8Array(10); // 10 zero-initialized bytes

// From another TypedArray
const original = new Uint16Array([100, 200, 300]);
const copy = new Uint16Array(original);
```

### DataView

```javascript
// DataView for flexible binary data access
const buffer = new ArrayBuffer(16);
const view = new DataView(buffer);

// Write different data types
view.setInt8(0, 127);      // byte 0
view.setUint8(1, 255);     // byte 1
view.setInt16(2, 1000);    // bytes 2-3
view.setFloat32(4, 3.14);  // bytes 4-7

// Read them back
console.log(view.getInt8(0));    // 127
console.log(view.getUint8(1));   // 255
console.log(view.getInt16(2));   // 1000
console.log(view.getFloat32(4)); // 3.140000104904175
```

### ArrayBuffer to/from Base64

```javascript
// Convert ArrayBuffer to Base64
function arrayBufferToBase64(buffer) {
  const bytes = new Uint8Array(buffer);
  let binary = '';
  for (let i = 0; i < bytes.byteLength; i++) {
    binary += String.fromCharCode(bytes[i]);
  }
  return btoa(binary);
}

// Convert Base64 to ArrayBuffer
function base64ToArrayBuffer(base64) {
  const binary = atob(base64);
  const bytes = new Uint8Array(binary.length);
  for (let i = 0; i < binary.length; i++) {
    bytes[i] = binary.charCodeAt(i);
  }
  return bytes.buffer;
}

// Usage
const original = new ArrayBuffer(8);
const base64 = arrayBufferToBase64(original);
const restored = base64ToArrayBuffer(base64);
```

### Blob

```javascript
// Create Blob from array of parts
const blob = new Blob(
  ['Hello, ', 'World!'],
  { type: 'text/plain' }
);

// Read Blob as text
const text = await blob.text();
console.log(text); // "Hello, World!"

// Create Blob from TypedArray
const uint8Array = new Uint8Array([72, 101, 108, 108, 111]);
const blob2 = new Blob([uint8Array], { type: 'text/plain' });

// Get Blob URL
const url = URL.createObjectURL(blob);
console.log(url); // "blob:http://..."
```

### File Reading

```javascript
// Read file as ArrayBuffer
async function readFileAsArrayBuffer(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.onerror = reject;
    reader.readAsArrayBuffer(file);
  });
}

// Read file as text
async function readFileAsText(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.onerror = reject;
    reader.readAsText(file);
  });
}

// Usage
const input = document.querySelector('input[type="file"]');
input.addEventListener('change', async (e) => {
  const file = e.target.files[0];
  const buffer = await readFileAsArrayBuffer(file);
  console.log(`File size: ${buffer.byteLength} bytes`);
});
```

### Binary Operations

```javascript
// Bitwise operations
const a = 0b1010; // 10 in binary
const b = 0b1100; // 12 in binary

console.log(a & b);   // 8  (AND: 1000)
console.log(a | b);   // 14 (OR: 1110)
console.log(a ^ b);   // 6  (XOR: 0110)
console.log(~a);      // -11 (NOT)
console.log(a << 1);  // 20 (left shift)
console.log(a >> 1);  // 5  (right shift)

// Byte manipulation
function getByte(value, byteIndex) {
  return (value >> (byteIndex * 8)) & 0xFF;
}

const number = 0x12345678;
console.log(getByte(number, 0)); // 0x78 (120)
console.log(getByte(number, 1)); // 0x56 (86)
console.log(getByte(number, 2)); // 0x34 (52)
console.log(getByte(number, 3)); // 0x12 (18)
```

### Working with Binary Strings

```javascript
// Binary string to integer
function binaryStringToInt(str) {
  return parseInt(str, 2);
}

// Integer to binary string
function intToBinaryString(num, bits = 8) {
  return num.toString(2).padStart(bits, '0');
}

console.log(binaryStringToInt("1010")); // 10
console.log(intToBinaryString(10));     // "00001010"
console.log(intToBinaryString(255, 8)); // "11111111"
```

### Practical Example: Image Processing

```javascript
// Simple image data manipulation
function createGrayscale(imageData) {
  const data = imageData.data;
  for (let i = 0; i < data.length; i += 4) {
    const avg = (data[i] + data[i + 1] + data[i + 2]) / 3;
    data[i] = avg;     // Red
    data[i + 1] = avg; // Green
    data[i + 2] = avg; // Blue
    // Alpha (data[i + 3]) unchanged
  }
  return imageData;
}

// Usage with canvas
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
const grayscaleData = createGrayscale(imageData);
ctx.putImageData(grayscaleData, 0, 0);
```

## Common Use Cases

1. **File handling** - Reading and processing binary files
2. **Network protocols** - Parsing binary network data
3. **Image/audio/video** - Processing media data
4. **Cryptography** - Working with encrypted data
5. **Data serialization** - Binary formats like Protocol Buffers
6. **WebSockets** - Binary data transmission
7. **WebAssembly** - Interfacing with WASM modules

## Common Mistakes

1. **Mixing up byte order** - Endianness matters for multi-byte values
2. **Incorrect buffer sizes** - Ensure buffers are large enough
3. **Not handling alignment** - Some operations require specific byte alignment
4. **Ignoring data view boundaries** - Stay within buffer bounds

```javascript
// WRONG: Buffer overflow
const buffer = new ArrayBuffer(4);
const view = new DataView(buffer);
view.setInt32(0, 123456789); // Works
view.setInt32(4, 123456789); // RangeError: offset out of bounds

// RIGHT: Check buffer size
const buffer = new ArrayBuffer(8);
const view = new DataView(buffer);
view.setInt32(0, 123456789); // First 4 bytes
view.setInt32(4, 987654321); // Next 4 bytes
```

## Quick Revision Summary

- `ArrayBuffer` is fixed-size raw memory buffer
- `TypedArrays` provide typed views (Int8, Uint8, Float32, etc.)
- `DataView` allows flexible reading/writing of multiple data types
- `Blob` represents immutable binary data
- Use `FileReader` to read files as binary data
- Bitwise operations work directly on binary representations
- Always consider byte order and buffer boundaries

## Related Topics

- [[ArrayBuffer]]
- [[TypedArrays]]
- [[DataView]]
- [[Blob]]
- [[FileReader]]
- [[WebSockets]]
- [[WebAssembly]]
- [[Binary-Operations]]
