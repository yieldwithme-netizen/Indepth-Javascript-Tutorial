# What-is-DataURL

## Definition

DataURL encodes file data as **Base64 string**.

## Example

```javascript
const reader = new FileReader();
reader.onload = function() {
    const dataURL = reader.result;
};
reader.readAsDataURL(file);
```

## Quick Revision

- DataURL = Base64 encoded
- Format: data:mime;base64,data
- Use for: inline images
- Larger than original

---

## Related Topics

- [[What-is-DataURL]] - [[What-is-DataURL|DataURL]]
- [[What-is-DataURL]] - [[What-is-DataURL|DataURL]]
- [[DataURL]] - [[DataURL|DataURL]]
- [[What-is-File-API]] - [[What-is-File-API|File API]]
