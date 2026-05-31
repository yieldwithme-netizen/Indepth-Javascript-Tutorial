# DataURL

## Definition

DataURL encodes file data as **Base64 string** in URL.

## Example

```javascript
const reader = new FileReader();
reader.onload = function() {
    const dataURL = reader.result;
    console.log(dataURL); // "data:image/png;base64,iVBORw0KGgo..."
};
reader.readAsDataURL(file);
```

## Quick Revision

- DataURL = Base64 encoded data
- Format: `data:mime;base64,data`
- Use for: inline images, small files
- Larger than original file

---

## Related Topics

- [[What-is-DataURL]] - [[What-is-DataURL|DataURL]]
- [[DataURL]] - [[DataURL|DataURL]]
- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[ArrayBuffer]] - [[ArrayBuffer|ArrayBuffer]]
