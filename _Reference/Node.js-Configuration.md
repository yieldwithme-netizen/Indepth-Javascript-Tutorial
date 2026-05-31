# Node.js Configuration

## Definition

Node.js configuration involves **setting up** the Node.js environment.

## package.json

```json
{
    "name": "my-app",
    "version": "1.0.0",
    "main": "index.js",
    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js"
    }
}
```

## .env File

```bash
PORT=3000
DATABASE_URL=postgresql://...
API_KEY=secret123
```

## Quick Revision

- package.json: project config
- .env: environment variables
- Use `process.env` to access
- Never commit .env files

---

## Related Topics

- [[What-is-Node]] - [[What-is-Node|Node.js]]
- [[Node.js-Configuration]] - [[Node.js-Configuration|Node.js config]]
- [[What-is-PackageJSON]] - [[What-is-PackageJSON|package.json]]
- [[Environment-Variables]] - [[Environment-Variables|Environment variables]]
