# What is the path Module?

The **path** module is a built-in Node.js module that provides utilities for working with file and directory paths. It helps you navigate the file system in a cross-platform way, handling differences between Windows, macOS, and Linux.

## Definition

The `path` module is a core Node.js module that offers methods to manipulate file paths, join directories, resolve absolute paths, and extract file information like extensions and basenames.

```javascript
// Import the path module
const path = require('path');

// Or with ES modules
import path from 'path';
```

## Key Methods

### path.join() - Combining Path Segments

```javascript
const path = require('path');

// Join multiple path segments
const fullPath = path.join('/users', 'documents', 'file.txt');
console.log(fullPath); // /users/documents/file.txt

// Handles redundant separators
path.join('/users//documents///file.txt');
// /users/documents/file.txt

// Resolves parent directory references
path.join('/users', '..', 'downloads', 'file.txt');
// /downloads/file.txt
```

### path.resolve() - Getting Absolute Paths

```javascript
const path = require('path');

// Resolves to an absolute path
const absolutePath = path.resolve('folder', 'file.txt');
// Result depends on current working directory

// Starting from root
const rootPath = path.resolve('/', 'users', 'documents');
// /users/documents

// Practical example
const configPath = path.resolve(__dirname, '..', 'config', 'app.json');
```

### path.basename() - Getting File Name

```javascript
const path = require('path');

// Get base filename
path.basename('/users/documents/report.pdf');
// report.pdf

// Get filename without extension
path.basename('/users/documents/report.pdf', '.pdf');
// report

// Windows-style paths also work
path.basename('C:\\Users\\Documents\\file.txt');
// file.txt
```

### path.dirname() - Getting Directory Name

```javascript
const path = require('path');

// Get directory containing the file
path.dirname('/users/documents/file.txt');
// /users/documents

// Useful for finding relative paths
const currentFile = '/home/user/projects/app.js';
const projectDir = path.dirname(currentFile);
// /home/user/projects
```

### path.extname() - Getting File Extension

```javascript
const path = require('path');

// Get file extension
path.extname('file.txt');
// .txt

path.extname('image.jpg');
// .jpg

// No extension returns empty string
path.extname('Makefile');
// ''
```

### path.parse() - Breaking Down Path

```javascript
const path = require('path');

const parsed = path.parse('/users/documents/report.pdf');
console.log(parsed);
// {
//   root: '/',
//   dir: '/users/documents',
//   base: 'report.pdf',
//   ext: '.pdf',
//   name: 'report'
// }

// Reconstruct path
const reconstructed = path.format(parsed);
// /users/documents/report.pdf
```

### path.relative() - Getting Relative Path

```javascript
const path = require('path');

// Get relative path between two locations
path.relative('/users/documents', '/users/pictures');
// ../pictures

path.relative('/home/user/project', '/home/user/project/src');
// src
```

### path.isAbsolute() - Checking Absolute Paths

```javascript
const path = require('path');

path.isAbsolute('/users/file.txt');   // true
path.isAbsolute('C:\\file.txt');      // true
path.isAbsolute('./file.txt');        // false
path.isAbsolute('file.txt');          // false
```

## Platform-Specific Paths

```javascript
const path = require('path');

// Check platform
console.log(process.platform); // 'win32', 'darwin', 'linux'

// Platform-specific separators
// Windows: C:\users\documents\file.txt
// Unix: /users/documents/file.txt

// Use path.join for cross-platform compatibility
const crossPlatformPath = path.join('folder', 'subfolder', 'file.txt');
```

## Working with __dirname and __filename

```javascript
const path = require('path');

// __dirname - directory of current module
console.log(__dirname);
// /home/user/projects/my-app/src

// __filename - file path of current module
console.log(__filename);
// /home/user/projects/my-app/src/index.js

// Common patterns
const configPath = path.join(__dirname, '..', 'config', 'config.json');
const publicDir = path.join(__dirname, '..', 'public');
```

## Practical Examples

### Loading Configuration Files

```javascript
const path = require('path');
const fs = require('fs');

function loadConfig() {
  const configPath = path.join(__dirname, '..', 'config', 'app.json');

  try {
    const configData = fs.readFileSync(configPath, 'utf8');
    return JSON.parse(configData);
  } catch (err) {
    console.error('Failed to load config:', err.message);
    return {};
  }
}
```

### Building File Paths Dynamically

```javascript
const path = require('path');

function buildUploadPath(userId, filename) {
  const uploadsDir = path.join(__dirname, '..', 'uploads');
  const userDir = path.join(uploadsDir, String(userId));
  const filePath = path.join(userDir, filename);

  return filePath;
}

// Usage
const filePath = buildUploadPath(123, 'profile.jpg');
// /home/user/app/uploads/123/profile.jpg
```

### Checking File Types

```javascript
const path = require('path');

function getFileType(filename) {
  const ext = path.extname(filename).toLowerCase();

  const types = {
    '.jpg': 'image',
    '.jpeg': 'image',
    '.png': 'image',
    '.gif': 'image',
    '.pdf': 'document',
    '.doc': 'document',
    '.docx': 'document',
    '.txt': 'text',
    '.js': 'javascript',
    '.json': 'json',
  };

  return types[ext] || 'unknown';
}

console.log(getFileType('photo.jpg')); // image
console.log(getFileType('readme.txt')); // text
```

## Common Use Cases

- Building file paths dynamically
- Loading configuration files
- Server-side file management
- Creating upload directories
- Finding file locations relative to current module
- Cross-platform file operations

## Common Mistakes

### 1. String Concatenation Instead of path.join
```javascript
// Bad - Platform-specific
const filePath = '/users/' + 'documents/' + 'file.txt';

// Good - Cross-platform
const filePath = path.join('/users', 'documents', 'file.txt');
```

### 2. Hardcoding Path Separators
```javascript
// Bad - Windows uses backslashes
const filePath = 'folder\\file.txt';

// Good - Uses correct separator for platform
const filePath = path.join('folder', 'file.txt');
```

### 3. Confusing resolve vs join
```javascript
// join just concatenates
path.join('folder', 'file.txt');
// folder/file.txt

// resolve resolves to absolute path
path.resolve('folder', 'file.txt');
// /current/working/directory/folder/file.txt
```

## Quick Revision

| Method | Description | Example |
|--------|-------------|---------|
| `join()` | Combine path segments | `path.join('a', 'b')` → `'a/b'` |
| `resolve()` | Get absolute path | `path.resolve('file.txt')` → `'/abs/path/file.txt'` |
| `basename()` | Get filename | `path.basename('/a/b.txt')` → `'b.txt'` |
| `dirname()` | Get directory | `path.dirname('/a/b.txt')` → `'/a'` |
| `extname()` | Get extension | `path.extname('file.txt')` → `'.txt'` |
| `parse()` | Break down path | Returns `{root, dir, base, ext, name}` |
| `format()` | Reconstruct path | From parsed object |
| `relative()` | Get relative path | `path.relative('/a', '/b')` → `'../b'` |
| `isAbsolute()` | Check if absolute | `path.isAbsolute('/a')` → `true` |

## Related Topics

- [[What-is-FS]] - File system operations
- [[What-is-Node]] - Node.js fundamentals
- [[What-is-HTTP]] - HTTP module for web servers
- [[Create-Server]] - Building HTTP servers
- [[What-is-Streams]] - Stream processing

---

**Key Takeaway:** Always use `path.join()` and `path.resolve()` instead of string concatenation to ensure your code works across different operating systems.
