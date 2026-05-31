# FileReader

## Definition

FileReader **reads files** from the user's computer.

## Example

```javascript
const reader = new FileReader();

// Read as text
reader.readAsText(file);

// Read as data URL
reader.readAsDataURL(file);

// Read as array buffer
reader.readAsArrayBuffer(file);

// Handle result
reader.onload = function() {
    console.log(reader.result);
};
```

## Quick Revision

- FileReader reads files
- Methods: readAsText, readAsDataURL, readAsArrayBuffer
- Events: onload, onerror
- Use for: file previews, uploads

---

## Related Topics

- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[FileReader]] - [[FileReader|FileReader]]
- [[What-is-Blob]] - [[What-is-Blob|Blob]]
- [[File-Upload]] - [[File-Upload|File upload]]
