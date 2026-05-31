# How to Serve Static Files

## Definition

Serving static files means **delivering CSS, JavaScript, images, and other assets** directly to the browser without processing.

## Using Express

```javascript
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from 'public' folder
app.use(express.static('public'));

// Serve from specific path
app.use('/assets', express.static('assets'));

// Serve with virtual path
app.use('/static', express.static(path.join(__dirname, 'public')));
```

## Folder Structure

```
project/
├── server.js
├── public/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── app.js
│   └── images/
│       └── logo.png
```

## Accessing Files

```html
<!-- In index.html -->
<link rel="stylesheet" href="/css/style.css">
<script src="/js/app.js"></script>
<img src="/images/logo.png" alt="Logo">
```

## MIME Types

| Extension | MIME Type |
|-----------|-----------|
| .html | text/html |
| .css | text/css |
| .js | application/javascript |
| .json | application/json |
| .png | image/png |
| .jpg | image/jpeg |
| .pdf | application/pdf |

## Security Considerations

```javascript
// ❌ Bad: Serve entire file system
app.use(express.static('/'));

// ✅ Good: Serve specific folder
app.use(express.static('public'));

// Disable directory listing
app.use(express.static('public', { dotfiles: 'deny' }));
```

## Quick Revision

- Use `express.static()` for static files
- Place files in `public` folder
- Access with `/filename` in HTML
- Never serve root directory
- MIME types set automatically

---

## Related Topics

- [[What-is-Express]] - Express overview
- [[Create-App]] - Creating Express app
- [[What-is-Middleware]] - Middleware
- [[Serve-Static]] - Serving static files
- [[What-is-Path]] - Path module
