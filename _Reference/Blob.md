# Blob

## Definition
A `Blob` (Binary Large Object) represents immutable raw binary data. Blobs are used for handling binary data like files, images, and media streams.

## Syntax
```javascript
const blob = new Blob(array, options);
```

- **array**: Array of parts ( ArrayBuffer, ArrayBufferView, Blob, string )
- **options**: Optional. `{ type: 'mime/type', endings: 'transparent' | 'native' }`

## Code Examples

### Creating a Blob
```javascript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });
console.log(blob.size); // 13
console.log(blob.type); // "text/plain"
```

### Blob from Multiple Parts
```javascript
const textPart = 'Hello, ';
const blobPart = new Blob(['World!']);
const arrayBufferPart = new ArrayBuffer(8);

const blob = new Blob([textPart, blobPart, arrayBufferPart], {
  type: 'text/plain'
});
```

### Reading Blob as Text
```javascript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });

blob.text().then(text => {
  console.log(text); // "Hello, World!"
});
```

### Reading Blob as ArrayBuffer
```javascript
const blob = new Blob([1, 2, 3, 4], { type: 'application/octet-stream' });

blob.arrayBuffer().then(buffer => {
  console.log(new Uint8Array(buffer)); // [1, 2, 3, 4]
});
```

### Blob to Data URL
```javascript
const blob = new Blob(['Hello'], { type: 'text/plain' });
const reader = new FileReader();

reader.onloadend = () => {
  console.log(reader.result); // "data:text/plain,SGVsbG8="
};

reader.readAsDataURL(blob);
```

### Blob to Object URL
```javascript
const blob = new Blob(['Hello'], { type: 'text/plain' });
const url = URL.createObjectURL(blob);

// Create downloadable link
const a = document.createElement('a');
a.href = url;
a.download = 'hello.txt';
a.click();

// Clean up
URL.revokeObjectURL(url);
```

### Slice a Blob
```javascript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });
const slice = blob.slice(0, 5, { type: 'text/plain' });

slice.text().then(text => {
  console.log(text); // "Hello"
});
```

### Blob from Fetch
```javascript
const response = await fetch('https://example.com/image.png');
const blob = await response.blob();

console.log(blob.type); // "image/png"
console.log(blob.size); // File size in bytes
```

### Uploading Blob
```javascript
const formData = new FormData();
formData.append('file', blob, 'filename.txt');

fetch('https://example.com/upload', {
  method: 'POST',
  body: formData
});
```

## Common Use Cases
- File uploads
- Image processing
- Download generation
- Media streaming
- Data caching
- Converting between formats

## Common Mistakes
- **Memory leaks**: Always revoke object URLs after use
- **Wrong MIME type**: Ensure correct type for content
- **Async operations**: Blob reading methods return Promises
- **File size limits**: Blobs have memory constraints

## Related Topics
- [[File-API]]
- [[ArrayBuffer]]
- [[Fetch-API]]
- [[FormData]]
- [[DataURL]]

## Quick Revision
- `Blob` represents immutable binary data
- Use `blob.text()`, `blob.arrayBuffer()` to read
- `URL.createObjectURL()` creates downloadable URLs
- Always revoke object URLs to free memory
- Essential for file uploads and downloads
