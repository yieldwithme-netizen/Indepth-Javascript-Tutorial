# File Upload

## Definition

File upload allows users to **send files to the server**.

## Basic Example

```html
<input type="file" id="fileInput" />
<button onclick="upload()">Upload</button>

<script>
async function upload() {
    const file = document.getElementById('fileInput').files[0];
    const formData = new FormData();
    formData.append('file', file);
    
    const response = await fetch('/upload', {
        method: 'POST',
        body: formData
    });
    
    const data = await response.json();
    console.log(data);
}
</script>
```

## Quick Revision

- Use `<input type="file">`
- Create FormData object
- Append file to FormData
- Send with fetch POST
- Server receives file

---

## Related Topics

- [[What-is-File-API]] - [[What-is-File-API|File API]]
- [[File-Upload]] - [[File-Upload|File upload]]
- [[What-is-Blob]] - [[What-is-Blob|Blob]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
