# What-is-Blob

## Definition

Blob represents **binary large objects** for files.

## Example

```javascript
const blob = new Blob(["Hello"], { type: "text/plain" });
const url = URL.createObjectURL(blob);

// Read as text
const text = await blob.text();
```

## Quick Revision

- Blob = binary data
- Create with `new Blob()`
- Use for: files, downloads
- Convert to URL with `createObjectURL`

---

## Related Topics

- [[What-is-Blob]] - [[What-is-Blob|Blob]]
- [[What-is-Blob]] - [[What-is-Blob|Blob]]
- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[DataURL]] - [[DataURL|DataURL]]
