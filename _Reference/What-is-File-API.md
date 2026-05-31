# What-is-File-API

## Definition

File API provides **client-side file access**.

## Example

```javascript
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

## Quick Revision

- File API for file access
- FileReader reads files
- Methods: readAsText, readAsDataURL
- Properties: name, size, type

---

## Related Topics

- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[File-Upload]] - [[File-Upload|File upload]]
- [[What-is-Blob]] - [[What-is-Blob|Blob]]
