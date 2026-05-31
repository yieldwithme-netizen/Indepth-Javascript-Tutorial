# package.json

## Definition

package.json is the **manifest file** for Node.js projects.

## Structure

```json
{
    "name": "my-project",
    "version": "1.0.0",
    "description": "My project",
    "main": "index.js",
    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js",
        "test": "jest"
    },
    "dependencies": {
        "express": "^4.18.2"
    },
    "devDependencies": {
        "nodemon": "^2.0.22"
    }
}
```

## Quick Revision

- package.json = project manifest
- Contains: name, version, scripts, dependencies
- `npm init` creates it
- `npm install` reads it

---

## Related Topics

- [[What-is-PackageJSON]] - [[What-is-PackageJSON|package.json]]
- [[Package.json]] - [[Package.json|package.json]]
- [[What-is-NPM]] - [[What-is-NPM|NPM]]
- [[Init-Project]] - [[Init-Project|Initializing project]]
