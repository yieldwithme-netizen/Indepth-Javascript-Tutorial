# What are Streams in Node.js?

**Streams** are a powerful way to handle reading and writing data in Node.js. They allow you to process data in chunks rather than loading everything into memory at once, making them ideal for large files and real-time data.

## Definition

Streams are objects that let you read from or write to a source continuously. They handle data piece by piece (chunks) instead of reading or writing the entire data at once.

```javascript
// Streams use the stream module
const stream = require('stream');

// Or specific stream types
const { Readable, Writable, Transform, Duplex } = require('stream');
```

## Four Types of Streams

### 1. Readable Streams

Used to read data from a source.

```javascript
const fs = require('fs');

// File read stream
const readStream = fs.createReadStream('large-file.txt', {
  encoding: 'utf8',
  highWaterMark: 16 * 1024, // 16KB chunks
});

// Event-based reading
readStream.on('data', (chunk) => {
  console.log(`Received ${chunk.length} characters`);
});

readStream.on('end', () => {
  console.log('Finished reading');
});

readStream.on('error', (err) => {
  console.error('Error:', err);
});

// Pipe to writable
readStream.pipe(process.stdout);
```

### 2. Writable Streams

Used to write data to a destination.

```javascript
const fs = require('fs');

// File write stream
const writeStream = fs.createWriteStream('output.txt');

// Write data
writeStream.write('First line\n');
writeStream.write('Second line\n');
writeStream.write('Third line\n');

// End the stream
writeStream.end('Final line\n');

writeStream.on('finish', () => {
  console.log('Write complete');
});

writeStream.on('error', (err) => {
  console.error('Error:', err);
});
```

### 3. Duplex Streams

Both readable and writable.

```javascript
const { Duplex } = require('stream');

const duplexStream = new Duplex({
  read(size) {
    // Implement read logic
    this.push(`Chunk ${this.counter++}\n`);
    if (this.counter > 5) {
      this.push(null); // End stream
    }
  },
  write(chunk, encoding, callback) {
    console.log('Writing:', chunk.toString());
    callback();
  },
});

duplexStream.on('data', (chunk) => {
  console.log('Read:', chunk.toString());
});
```

### 4. Transform Streams

Modify data as it passes through.

```javascript
const { Transform } = require('stream');

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  },
});

// Usage
process.stdin
  .pipe(upperCaseTransform)
  .pipe(process.stdout);
```

## Practical Examples

### File Copying with Streams

```javascript
const fs = require('fs');

function copyFile(source, destination) {
  return new Promise((resolve, reject) => {
    const readStream = fs.createReadStream(source);
    const writeStream = fs.createWriteStream(destination);

    readStream.pipe(writeStream);

    writeStream.on('finish', resolve);
    writeStream.on('error', reject);
    readStream.on('error', reject);
  });
}

// Usage
copyFile('large-video.mp4', 'copy.mp4')
  .then(() => console.log('File copied'))
  .catch(console.error);
```

### HTTP Streaming Response

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  if (req.url === '/video') {
    const videoPath = './video.mp4';
    const stat = fs.statSync(videoPath);

    res.writeHead(200, {
      'Content-Type': 'video/mp4',
      'Content-Length': stat.size,
    });

    const readStream = fs.createReadStream(videoPath);
    readStream.pipe(res);
  }
});

server.listen(3000);
```

### Transform Stream for Data Processing

```javascript
const { Transform } = require('stream');
const fs = require('fs');

class CSVToJSON extends Transform {
  constructor(options) {
    super({ ...options, objectMode: true });
    this.headers = null;
  }

  _transform(chunk, encoding, callback) {
    const lines = chunk.toString().split('\n');

    for (const line of lines) {
      if (!line.trim()) continue;

      const values = line.split(',');

      if (!this.headers) {
        this.headers = values;
        continue;
      }

      const obj = {};
      this.headers.forEach((header, i) => {
        obj[header.trim()] = values[i]?.trim();
      });

      this.push(JSON.stringify(obj) + '\n');
    }

    callback();
  }
}

// Usage
const csvStream = fs.createReadStream('data.csv');
const jsonStream = new CSVToJSON();

csvStream.pipe(jsonStream).pipe(fs.createWriteStream('data.json'));
```

### Piping Multiple Streams

```javascript
const { createGzip } = require('zlib');
const fs = require('fs');

// Compress a file
const readStream = fs.createReadStream('large-file.txt');
const gzipStream = createGzip();
const writeStream = fs.createWriteStream('large-file.txt.gz');

readStream
  .pipe(gzipStream)
  .pipe(writeStream)
  .on('finish', () => console.log('Compression complete'));
```

### Custom Transform Stream

```javascript
const { Transform } = require('stream');

class LineCounter extends Transform {
  constructor(options) {
    super(options);
    this.lineCount = 0;
  }

  _transform(chunk, encoding, callback) {
    const lines = chunk.toString().split('\n');
    this.lineCount += lines.length - 1;
    this.push(chunk); // Pass data through unchanged
    callback();
  }

  _flush(callback) {
    this.push(`\nTotal lines: ${this.lineCount}`);
    callback();
  }
}

// Usage
const counter = new LineCounter();
process.stdin.pipe(counter).pipe(process.stdout);
```

## Stream Events

```javascript
const fs = require('fs');

const stream = fs.createReadStream('file.txt');

// Common events
stream.on('data', (chunk) => {
  console.log('Received chunk');
});

stream.on('end', () => {
  console.log('No more data');
});

stream.on('error', (err) => {
  console.error('Error:', err);
});

stream.on('close', () => {
  console.log('Stream closed');
});

stream.on('pause', () => {
  console.log('Stream paused');
});

stream.on('resume', () => {
  console.log('Stream resumed');
});
```

## Backpressure Handling

```javascript
const fs = require('fs');

const readable = fs.createReadStream('huge-file.txt');
const writable = fs.createWriteStream('output.txt');

readable.on('data', (chunk) => {
  // Check if buffer is full
  const canContinue = writable.write(chunk);

  if (!canContinue) {
    // Wait for drain event
    readable.pause();
    writable.once('drain', () => {
      readable.resume();
    });
  }
});

// Or simply use pipe() which handles backpressure automatically
readable.pipe(writable);
```

## Common Use Cases

- Reading/writing large files
- HTTP streaming (video, audio)
- Real-time data processing
- Log file processing
- File compression/decompression
- Data transformation pipelines
- Network proxying
- Image/video processing

## Common Mistakes

### 1. Not Handling Errors
```javascript
// Bad - Stream may crash
const stream = fs.createReadStream('file.txt');

// Good - Always handle errors
const stream = fs.createReadStream('file.txt');
stream.on('error', (err) => {
  console.error('Stream error:', err);
});
```

### 2. Not Piping Correctly
```javascript
// Bad - May cause issues
readStream.pipe(writeStream);
writeStream.on('error', console.error);

// Good - Handle errors on all streams
readStream
  .on('error', handleError)
  .pipe(writeStream)
  .on('error', handleError);
```

### 3. Forgetting to End Writable Streams
```javascript
// Bad - Stream stays open
const writeStream = fs.createWriteStream('file.txt');
writeStream.write('data');

// Good - End the stream
writeStream.write('data');
writeStream.end();
```

### 4. Using Callbacks Instead of Streams for Large Data
```javascript
// Bad - Loads entire file into memory
fs.readFile('huge-file.txt', (err, data) => {
  // data could be gigabytes!
});

// Good - Stream in chunks
const stream = fs.createReadStream('huge-file.txt');
stream.on('data', (chunk) => {
  // Process chunk by chunk
});
```

## Quick Revision

| Stream Type | Purpose | Methods |
|-------------|---------|---------|
| Readable | Read data | `pipe()`, `read()`, `on('data')` |
| Writable | Write data | `write()`, `end()`, `pipe()` |
| Duplex | Read & write | Both readable and writable |
| Transform | Modify data | `_transform()`, `_flush()` |

| Event | When It Fires |
|-------|---------------|
| `data` | Chunk received |
| `end` | No more data |
| `error` | Error occurred |
| `finish` | All data flushed |
| `close` | Stream closed |

| Method | Description |
|--------|-------------|
| `pipe()` | Connect readable to writable |
| `unpipe()` | Disconnect streams |
| `pause()` | Pause reading |
| `resume()` | Resume reading |
| `write()` | Write chunk to writable |
| `end()` | Signal end of writing |

## Related Topics

- [[What-is-FS]] - File system streams
- [[What-is-Node]] - Node.js fundamentals
- [[What-is-HTTP]] - HTTP streaming
- [[Create-Server]] - Serving streamed responses
- [[What-is-EventLoop]] - How streams work with event loop

---

**Key Takeaway:** Streams are essential for handling large data efficiently in Node.js. Always use `pipe()` for connecting streams and handle errors on all streams to prevent crashes.
