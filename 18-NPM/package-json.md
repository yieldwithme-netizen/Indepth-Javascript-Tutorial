# package.json

## Definition

package.json is the **manifest file** for Node.js projects.

## Structure

```json
{
    "name": "my-project",
    "version": "1.0.0",
    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js"
    },
    "dependencies": {
        "express": "^4.18.2"
    }
}
```

## Quick Revision

- package.json = project config
- Contains: name, version, scripts, dependencies
- `npm init` creates it

---

## Related Topics

- [[What-is-PackageJSON]] - [[What-is-PackageJSON|package.json]]
- [[package-json]] - [[package-json|package.json]]
- [[What-is-NPM]] - [[What-is-NPM|NPM]]
- [[Init-Project]] - [[Init-Project|Initializing project]]
