# npm (Node Package Manager)

## Definition

npm is the **default package manager** for Node.js. It manages project dependencies and provides access to code packages.

## Basic Commands

```bash
# Initialize project
npm init -y

# Install package
npm install lodash

# Install dev dependency
npm install --save-dev nodemon

# Install globally
npm install -g typescript

# Remove package
npm uninstall lodash

# Update packages
npm update

# List packages
npm list

# Search packages
npm search lodash
```

## package.json

```json
{
    "name": "my-project",
    "version": "1.0.0",
    "description": "My project",
    "main": "index.js",
    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js",
        "test": "jest",
        "build": "webpack"
    },
    "dependencies": {
        "lodash": "^4.17.21"
    },
    "devDependencies": {
        "nodemon": "^2.0.22"
    }
}
```

## package-lock.json

- Records exact versions of installed packages
- Ensures consistent installs across machines
- Never delete or edit manually

## npx

```bash
# Run package without installing
npx create-react-app my-app

# Run local package
npx nodemon index.js
```

## Quick Revision

- npm = Node Package Manager
- `npm init` creates package.json
- `npm install` installs packages
- `dependencies` vs `devDependencies`
- `package-lock.json` locks versions
- `npx` runs packages without installing

---

## Related Topics

- [[What-is-NPM]] - [[What-is-NPM|NPM]] overview
- [[What-is-PackageJSON]] - [[What-is-PackageJSON|package.json]]
- [[Init-Project]] - [[Init-Project|Initializing project]]
- [[Install-Packages]] - [[Install-Packages|Installing packages]]
- [[What-is-SemVer]] - [[What-is-SemVer|Semantic versioning]]
