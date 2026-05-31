# What is the fs Module?

The **fs (File System)** module is a built-in Node.js module that provides an API for interacting with the file system. It allows you to read, write, update, delete, and manipulate files and directories on your machine.

## Definition

The `fs` module is one of the core modules in Node.js that enables file system operations. It offers both **synchronous** and **asynchronous** methods for performing file-related tasks.

```javascript
// Import the fs module
const fs = require('fs');

// Or with ES modules
import fs from 'fs';
import { promises as fsPromises } from 'fs';
```

## Core Operations

### Reading Files

```javascript
const fs = require('fs');

// Synchronous read
try {
  const data = fs.readFileSync('example.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error('Error reading file:', err);
}

// Asynchronous read (callback-based)
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  console.log(data);
});

// Promise-based read
const fsPromises = require('fs').promises;

async function readFile() {
  try {
    const data = await fsPromises.readFile('example.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error('Error:', err);
  }
}
```

### Writing Files

```javascript
const fs = require('fs');

// Synchronous write
fs.writeFileSync('output.txt', 'Hello, World!');

// Asynchronous write
fs.writeFile('output.txt', 'Hello, World!', (err) => {
  if (err) {
    console.error('Error writing file:', err);
    return;
  }
  console.log('File written successfully');
});

// Append to file
fs.appendFile('output.txt', '\nNew line', (err) => {
  if (err) throw err;
  console.log('Content appended');
});
```

### Working with Directories

```javascript
const fs = require('fs');

// Create directory
fs.mkdirSync('new-folder');

// Create nested directories
fs.mkdirSync('parent/child/grandchild', { recursive: true });

// Read directory contents
const files = fs.readdirSync('./');
console.log(files);

// Remove directory (must be empty)
fs.rmdirSync('new-folder');

// Remove directory and contents
fs.rmSync('folder', { recursive: true, force: true });
```

### File Information

```javascript
const fs = require('fs');

// Get file stats
const stats = fs.statSync('example.txt');
console.log('Size:', stats.size);
console.log('Created:', stats.birthtime);
console.log('Modified:', stats.mtime);
console.log('Is file:', stats.isFile());
console.log('Is directory:', stats.isDirectory());

// Check if file exists
if (fs.existsSync('example.txt')) {
  console.log('File exists');
}
```

### Renaming and Deleting

```javascript
const fs = require('fs');

// Rename file
fs.renameSync('old-name.txt', 'new-name.txt');

// Delete file
fs.unlinkSync('file-to-delete.txt');
```

## The Promise API

```javascript
const { promises: fsPromises } = require('fs');

async function fileOperations() {
  // Read
  const content = await fsPromises.readFile('data.json', 'utf8');
  const jsonData = JSON.parse(content);

  // Write
  await fsPromises.writeFile('output.json', JSON.stringify(jsonData, null, 2));

  // Copy
  await fsPromises.copyFile('source.txt', 'destination.txt');

  // Rename
  await fsPromises.rename('old.txt', 'new.txt');

  // Delete
  await fsPromises.unlink('temp.txt');

  // Create directory
  await fsPromises.mkdir('new-dir', { recursive: true });

  // Get stats
  const stats = await fsPromises.stat('file.txt');
  console.log('File size:', stats.size);
}
```

## Streams with fs

```javascript
const fs = require('fs');

// Create read stream
const readStream = fs.createReadStream('large-file.txt', 'utf8');
readStream.on('data', (chunk) => {
  console.log('Received chunk:', chunk.length);
});
readStream.on('end', () => {
  console.log('Finished reading');
});

// Create write stream
const writeStream = fs.createWriteStream('output.txt');
writeStream.write('First line\n');
writeStream.write('Second line\n');
writeStream.end();
```

## Common Use Cases

- Reading configuration files (JSON, YAML)
- Log file management
- File upload handling
- Data processing and transformation
- Creating backup copies
- Building file-based databases
- Static file serving in web servers

## Common Mistakes

### 1. Forgetting Error Handling
```javascript
// Bad - No error handling
fs.readFile('file.txt', (err, data) => {
  console.log(data); // May crash if err occurs
});

// Good - Proper error handling
fs.readFile('file.txt', (err, data) => {
  if (err) {
    console.error('Failed to read file:', err);
    return;
  }
  console.log(data);
});
```

### 2. Using Sync Methods in Production
```javascript
// Bad - Blocks the event loop
const data = fs.readFileSync('large-file.txt');

// Good - Non-blocking
fs.readFile('large-file.txt', (err, data) => {
  // Process data here
});
```

### 3. Not Handling File Paths Correctly
```javascript
const path = require('path');

// Bad - Platform-specific
const filePath = 'folder/file.txt';

// Good - Cross-platform
const filePath = path.join('folder', 'file.txt');
```

## Quick Revision

| Method | Description | Sync Version |
|--------|-------------|--------------|
| `readFile` | Read file contents | `readFileSync` |
| `writeFile` | Write data to file | `writeFileSync` |
| `appendFile` | Append data to file | `appendFileSync` |
| `mkdir` | Create directory | `mkdirSync` |
| `readdir` | List directory contents | `readdirSync` |
| `stat` | Get file information | `statSync` |
| `unlink` | Delete file | `unlinkSync` |
| `rename` | Rename/move file | `renameSync` |
| `copyFile` | Copy file | `copyFileSync` |
| `rm` | Remove file/directory | `rmSync` |

## Related Topics

- [[What-is-Node]] - Introduction to Node.js fundamentals
- [[What-is-Path]] - The path module for working with file paths
- [[What-is-Streams]] - Stream processing for large files
- [[What-is-HTTP]] - HTTP module for web servers
- [[Create-Server]] - Building HTTP servers with Node.js

---

**Key Takeaway:** The `fs` module is essential for any Node.js application that needs to interact with the file system. Always prefer asynchronous methods to avoid blocking the event loop, and handle errors properly to prevent crashes.
