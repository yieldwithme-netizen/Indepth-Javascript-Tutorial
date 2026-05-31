# How to Read Files in Node.js

## Definition

Node.js provides the `fs` (File System) module for reading and writing files. It supports both synchronous and asynchronous operations, with asynchronous being the preferred approach for production applications.

## Module Import

```javascript
// CommonJS
const fs = require('fs');

// ES Modules
import fs from 'fs';
import { readFile, readFileSync } from 'fs/promises';
```

## Reading Files

### 1. Synchronous (Blocking)

```javascript
const fs = require('fs');

// Read entire file
const data = fs.readFileSync('file.txt', 'utf8');
console.log(data);

// Read without encoding (returns Buffer)
const buffer = fs.readFileSync('file.txt');
console.log(buffer.toString('utf8'));

// Check if file exists before reading
if (fs.existsSync('file.txt')) {
  const data = fs.readFileSync('file.txt', 'utf8');
  console.log(data);
}
```

### 2. Asynchronous (Non-Blocking) - Callback

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log(data);
});
```

### 3. Asynchronous (Non-Blocking) - Promises

```javascript
const fs = require('fs').promises;

async function readFile() {
  try {
    const data = await fs.readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error('Error reading file:', err);
  }
}

readFile();
```

### 4. fs/promises (Recommended)

```javascript
import { readFile } from 'fs/promises';

async function main() {
  try {
    const data = await readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error('Error:', err);
  }
}

main();
```

## Reading Different File Types

### JSON Files

```javascript
import { readFile } from 'fs/promises';

async function readJSON() {
  try {
    const data = await readFile('config.json', 'utf8');
    const config = JSON.parse(data);
    console.log(config);
  } catch (err) {
    console.error('Error reading JSON:', err);
  }
}

// Synchronous version
const data = readFileSync('config.json', 'utf8');
const config = JSON.parse(data);
```

### CSV Files

```javascript
import { readFile } from 'fs/promises';

async function readCSV() {
  const data = await readFile('data.csv', 'utf8');
  const lines = data.split('\n');
  const headers = lines[0].split(',');

  const rows = lines.slice(1).map(line => {
    const values = line.split(',');
    return headers.reduce((obj, header, index) => {
      obj[header] = values[index];
      return obj;
    }, {});
  });

  return rows;
}
```

### Binary Files

```javascript
import { readFile } from 'fs/promises';

async function readBinary() {
  const buffer = await readFile('image.png');
  console.log(buffer);  // Buffer object
  console.log(buffer.length);  // File size in bytes
}
```

## Reading Streams

For large files, use streams to read in chunks:

```javascript
import { createReadStream } from 'fs';
import { createInterface } from 'readline';

// Read line by line
async function readLines() {
  const fileStream = createReadStream('large-file.txt', 'utf8');
  const rl = createInterface({
    input: fileStream,
    crlfDelay: Infinity
  });

  for await (const line of rl) {
    console.log(line);
  }
}

// Read in chunks
function readChunks() {
  const stream = createReadStream('large-file.txt', {
    encoding: 'utf8',
    highWaterMark: 1024  // 1KB chunks
  });

  stream.on('data', (chunk) => {
    console.log(`Received ${chunk.length} bytes`);
  });

  stream.on('end', () => {
    console.log('Finished reading');
  });

  stream.on('error', (err) => {
    console.error('Error:', err);
  });
}
```

## Reading Directory Contents

```javascript
import { readdir, readdirSync } from 'fs/promises';
import { readdir as readdirCallback } from 'fs';

// Asynchronous
async function listFiles() {
  const files = await readdir('./my-folder');
  console.log(files);
}

// With file type info
async function listFilesDetailed() {
  const entries = await readdir('./my-folder', { withFileTypes: true });
  entries.forEach(entry => {
    console.log(`${entry.name} - ${entry.isDirectory() ? 'Directory' : 'File'}`);
  });
}

// Synchronous
const files = readdirSync('./my-folder');
```

## Reading with Promises Wrapper

```javascript
import { readFile } from 'fs/promises';
import { promisify } from 'util';

// Custom promise wrapper for any callback-based function
const readFileAsync = promisify(readFile);

async function main() {
  const data = await readFileAsync('file.txt', 'utf8');
  console.log(data);
}
```

## Common Use Cases

- **Configuration Files**: Reading JSON configs
- **Log Files**: Parsing and analyzing logs
- **Data Processing**: Reading CSV, XML, or custom formats
- **Template Files**: Loading HTML or text templates
- **Asset Processing**: Reading images, fonts, or binary data

## Common Mistakes

### Not Handling Errors

```javascript
// BAD: No error handling
const data = await readFile('file.txt', 'utf8');

// GOOD: Always handle errors
try {
  const data = await readFile('file.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error('Error reading file:', err.message);
}
```

### Using Sync in Production

```javascript
// BAD: Blocks the event loop
app.get('/user', (req, res) => {
  const config = readFileSync('config.json', 'utf8');  // Blocks!
  res.json(JSON.parse(config));
});

// GOOD: Use async/await
app.get('/user', async (req, res) => {
  try {
    const config = await readFile('config.json', 'utf8');
    res.json(JSON.parse(config));
  } catch (err) {
    res.status(500).json({ error: 'Failed to read config' });
  }
});
```

### Not Specifying Encoding

```javascript
// BAD: Returns Buffer, may cause issues
const data = await readFile('file.txt');

// GOOD: Specify encoding for text files
const data = await readFile('file.txt', 'utf8');
```

### Reading Entire Large Files

```javascript
// BAD: Loads entire file into memory
const hugeData = await readFile('huge-file.txt', 'utf8');  // Memory issues!

// GOOD: Use streams for large files
import { createReadStream } from 'fs';

const stream = createReadStream('huge-file.txt', 'utf8');
stream.on('data', (chunk) => {
  // Process chunks
});
```

### Not Checking File Existence

```javascript
// BAD: Throws error if file doesn't exist
const data = await readFile('maybe-exists.txt', 'utf8');

// GOOD: Check existence first
import { access } from 'fs/promises';

async function safeRead(filePath) {
  try {
    await access(filePath);
    return await readFile(filePath, 'utf8');
  } catch {
    console.log('File does not exist');
    return null;
  }
}
```

## Performance Tips

```javascript
// 1. Use appropriate encoding
await readFile('text.txt', 'utf8');  // Text
await readFile('image.png');          // Binary

// 2. Use streams for large files
import { createReadStream } from 'fs';

// 3. Cache frequently read files
const cache = new Map();

async function cachedRead(filePath) {
  if (cache.has(filePath)) {
    return cache.get(filePath);
  }
  const data = await readFile(filePath, 'utf8');
  cache.set(filePath, data);
  return data;
}

// 4. Use worker threads for CPU-intensive parsing
import { Worker } from 'worker_threads';
```

## Related Topics

- [[Write-Files]]
- [[What-is-NodeModules]]
- [[What-is-State]]

## Quick Revision

- Use `fs` module for file operations
- **Sync**: `readFileSync()` — blocks event loop, use for scripts
- **Async Callback**: `readFile()` — old pattern, avoid
- **Async Promises**: `readFile().promises` — recommended approach
- Always specify `'utf8'` encoding for text files
- Use **streams** for large files to avoid memory issues
- Always handle errors with try/catch
- Use `readdir()` to list directory contents
- Never use sync operations in production servers
