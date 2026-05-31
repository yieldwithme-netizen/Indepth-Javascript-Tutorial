# Streams

## Definition

Streams are **sequences of data** processed over time.

## Node.js Streams

```javascript
const fs = require('fs');

// Readable stream
const readStream = fs.createReadStream('file.txt');

// Writable stream
const writeStream = fs.createWriteStream('output.txt');

// Pipe
readStream.pipe(writeStream);
```

## Browser Streams

```javascript
// Fetch with streaming
const response = await fetch(url);
const reader = response.body.getReader();

while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    console.log(value);
}
```

## Quick Revision

- Streams process data over time
- Readable, Writable, Transform
- Use `pipe()` to connect streams
- Memory efficient for large data

---

## Related Topics

- [[What-is-Streams]] - [[What-is-Streams|Streams]]
- [[Streams]] - [[Streams|Streams]]
- [[What-is-Node]] - [[What-is-Node|Node.js]]
- [[What-is-FS]] - [[What-is-FS|File system]]
