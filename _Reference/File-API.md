# File API

## Definition

The File API provides **client-side file access** for reading and processing files in the browser.

## Reading Files

```javascript
// Input element
<input type="file" id="fileInput" />

const input = document.getElementById('fileInput');
input.addEventListener('change', (e) => {
    const file = e.target.files[0];
    
    const reader = new FileReader();
    reader.onload = (event) => {
        console.log(event.target.result);
    };
    reader.readAsText(file);
});
```

## FileReader Methods

```javascript
const reader = new FileReader();

// Read as text
reader.readAsText(file);

// Read as data URL
reader.readAsDataURL(file);

// Read as array buffer
reader.readAsArrayBuffer(file);

// Read as binary string
reader.readAsBinaryString(file);
```

## File Properties

```javascript
input.addEventListener('change', (e) => {
    const file = e.target.files[0];
    
    console.log(file.name);     // "document.pdf"
    console.log(file.size);     // 123456 (bytes)
    console.log(file.type);     // "application/pdf"
    console.log(file.lastModified); // 1620000000000
});
```

## Quick Revision

- File API for client-side file access
- `FileReader` reads file contents
- Methods: readAsText, readAsDataURL, readAsArrayBuffer
- File properties: name, size, type
- Use for: file uploads, previews, processing

---

## Related Topics

- [[What-is-File-API]] - [[What-is-File-API|File API]] overview
- [[Read-Files]] - [[Read-Files|Reading files]]
- [[What-is-Blob]] - [[What-is-Blob|Blob]]
- [[ArrayBuffer]] - [[ArrayBuffer|ArrayBuffer]]
