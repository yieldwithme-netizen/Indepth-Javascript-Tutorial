# How to Write Files in Node.js

## Definition

Node.js provides the `fs` (File System) module for writing files. It supports synchronous and asynchronous operations, with options for creating, overwriting, and appending to files.

## Module Import

```javascript
// CommonJS
const fs = require('fs');

// ES Modules
import fs from 'fs';
import { writeFile, writeFileSync, appendFile } from 'fs/promises';
```

## Writing Files

### 1. Synchronous (Blocking)

```javascript
const fs = require('fs');

// Write text file (creates or overwrites)
fs.writeFileSync('output.txt', 'Hello, World!');

// Write with encoding
fs.writeFileSync('output.txt', 'Hello, World!', 'utf8');

// Write JSON
const data = { name: 'John', age: 30 };
fs.writeFileSync('data.json', JSON.stringify(data, null, 2));
```

### 2. Asynchronous (Non-Blocking) - Callback

```javascript
const fs = require('fs');

fs.writeFile('output.txt', 'Hello, World!', 'utf8', (err) => {
  if (err) {
    console.error('Error writing file:', err);
    return;
  }
  console.log('File written successfully');
});
```

### 3. Asynchronous (Non-Blocking) - Promises (Recommended)

```javascript
import { writeFile } from 'fs/promises';

async function writeData() {
  try {
    await writeFile('output.txt', 'Hello, World!', 'utf8');
    console.log('File written successfully');
  } catch (err) {
    console.error('Error writing file:', err);
  }
}

writeData();
```

## Writing Different File Types

### JSON Files

```javascript
import { writeFile } from 'fs/promises';

async function writeJSON() {
  const config = {
    name: 'my-app',
    version: '1.0.0',
    settings: {
      theme: 'dark',
      language: 'en'
    }
  };

  await writeFile('config.json', JSON.stringify(config, null, 2));
  console.log('Config saved');
}
```

### CSV Files

```javascript
import { writeFile } from 'fs/promises';

async function writeCSV() {
  const headers = ['Name', 'Email', 'Age'];
  const rows = [
    ['John', 'john@example.com', '30'],
    ['Jane', 'jane@example.com', '25']
  ];

  const csv = [
    headers.join(','),
    ...rows.map(row => row.join(','))
  ].join('\n');

  await writeFile('contacts.csv', csv);
}
```

### Binary Files

```javascript
import { writeFile } from 'fs/promises';

async function writeBinary() {
  const buffer = Buffer.from([0x89, 0x50, 0x4E, 0x47]);
  await writeFile('image.png', buffer);
}
```

## Appending to Files

```javascript
import { appendFile } from 'fs/promises';

async function appendToFile() {
  await appendFile('log.txt', 'New log entry\n');
  await appendFile('log.txt', 'Another entry\n');
}

// Synchronous
fs.appendFileSync('log.txt', 'New entry\n');
```

## Writing Streams

For large files or continuous writing:

```javascript
import { createWriteStream } from 'fs';

// Write line by line
function writeStream() {
  const stream = createWriteStream('output.txt');

  stream.write('First line\n');
  stream.write('Second line\n');
  stream.write('Third line\n');

  stream.end();
  stream.on('finish', () => {
    console.log('Writing complete');
  });
}

// Pipe readable stream to writable stream
import { createReadStream, createWriteStream } from 'fs';

function copyFile() {
  const readStream = createReadStream('source.txt');
  const writeStream = createWriteStream('destination.txt');

  readStream.pipe(writeStream);

  writeStream.on('finish', () => {
    console.log('File copied');
  });
}
```

## Writing with Promises Wrapper

```javascript
import { writeFile } from 'fs/promises';
import { promisify } from 'util';

// Custom promise wrapper
const writeFileAsync = promisify(writeFile);

async function main() {
  await writeFileAsync('output.txt', 'Hello!');
}
```

## Common Use Cases

- **Log Files**: Writing application logs
- **Data Export**: Creating CSV, JSON, or text exports
- **Configuration**: Saving user settings
- **Cache Files**: Storing cached data
- **Generated Content**: Creating reports, documents

## Common Mistakes

### Not Handling Errors

```javascript
// BAD: No error handling
await writeFile('output.txt', 'data');

// GOOD: Always handle errors
try {
  await writeFile('output.txt', 'data');
  console.log('File written successfully');
} catch (err) {
  console.error('Error writing file:', err.message);
}
```

### Using Sync in Production

```javascript
// BAD: Blocks the event loop
app.post('/export', (req, res) => {
  fs.writeFileSync('export.csv', csvData);  // Blocks!
  res.json({ success: true });
});

// GOOD: Use async/await
app.post('/export', async (req, res) => {
  try {
    await writeFile('export.csv', csvData);
    res.json({ success: true });
  } catch (err) {
    res.status(500).json({ error: 'Export failed' });
  }
});
```

### Not Creating Directory First

```javascript
// BAD: Directory doesn't exist
await writeFile('logs/app.log', 'data');  // Error: ENOENT

// GOOD: Ensure directory exists
import { mkdir } from 'fs/promises';

async function writeToFile(filePath, data) {
  const dir = require('path').dirname(filePath);
  await mkdir(dir, { recursive: true });
  await writeFile(filePath, data);
}

await writeToFile('logs/app.log', 'log data');
```

### Not Handling Encoding

```javascript
// BAD: May cause encoding issues
await writeFile('output.txt', 'Hello');

// GOOD: Specify encoding
await writeFile('output.txt', 'Hello', 'utf8');
```

### Not Using Atomic Writes

```javascript
// BAD: File may be corrupted if process crashes
await writeFile('important.txt', 'critical data');

// GOOD: Write to temp file, then rename (atomic)
import { rename, unlink } from 'fs/promises';
import { tmpdir } from 'os';
import { join } from 'path';

async function atomicWrite(filePath, data) {
  const tempPath = join(tmpdir(), `temp-${Date.now()}`);
  try {
    await writeFile(tempPath, data, 'utf8');
    await rename(tempPath, filePath);
  } catch (err) {
    await unlink(tempPath).catch(() => {});
    throw err;
  }
}
```

### Not Truncating File

```javascript
// BAD: File may have leftover data
// If new content is shorter than old content
await writeFile('output.txt', 'Hi');  // Old: 'Hello World'

// GOOD: Use flag to truncate
await writeFile('output.txt', 'Hi', { flag: 'w' });  // 'w' is default, truncates
```

## File Flags

```javascript
// Common flags
const flags = {
  'w': 'Open file for writing. Creates if not exists. Truncates if exists.',
  'wx': 'Open for writing. Fails if file exists.',
  'a': 'Open for appending. Creates if not exists.',
  'ax': 'Open for appending. Fails if file exists.',
  'r+': 'Open for reading and writing.',
  'w+': 'Open for reading and writing. Creates if not exists.'
};

// Example: Append mode
await writeFile('log.txt', 'entry\n', { flag: 'a' });
```

## Performance Tips

```javascript
// 1. Buffer data before writing
const chunks = [];
for (const item of largeArray) {
  chunks.push(formatItem(item));
}
await writeFile('output.txt', chunks.join(''));

// 2. Use streams for large files
import { createWriteStream } from 'fs';

const stream = createWriteStream('large-output.txt');
for (const chunk of data) {
  stream.write(chunk);
}
stream.end();

// 3. Use worker threads for CPU-intensive formatting
import { Worker } from 'worker_threads';

const worker = new Worker('./format-worker.js');
worker.postMessage(largeArray);
worker.on('message', async (formatted) => {
  await writeFile('output.txt', formatted);
});
```

## Writing Multiple Files

```javascript
import { writeFile } from 'fs/promises';

// Sequential (slower)
async function writeSequential() {
  await writeFile('file1.txt', 'data1');
  await writeFile('file2.txt', 'data2');
  await writeFile('file3.txt', 'data3');
}

// Parallel (faster)
async function writeParallel() {
  await Promise.all([
    writeFile('file1.txt', 'data1'),
    writeFile('file2.txt', 'data2'),
    writeFile('file3.txt', 'data3')
  ]);
}
```

## Related Topics

- [[Read-Files]]
- [[What-is-NodeModules]]

## Quick Revision

- Use `fs` module for file writing operations
- **Sync**: `writeFileSync()` — blocks event loop, use for scripts only
- **Async Callback**: `writeFile()` — old pattern, avoid
- **Async Promises**: `writeFile().promises` — recommended approach
- Always handle errors with try/catch
- Create directories before writing with `mkdir({ recursive: true })`
- Use **atomic writes** (write temp → rename) for critical files
- Use **streams** for large files or continuous writing
- Specify `'utf8'` encoding for text files
- Use `{ flag: 'a' }` for appending instead of overwriting
