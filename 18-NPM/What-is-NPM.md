# What is NPM?

## Definition

NPM (Node Package Manager) is the **package manager for JavaScript**. It lets you install, share, and manage code packages.

## Basic Commands

```bash
# Initialize a project
npm init -y

# Install a package
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
        "test": "jest"
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

## Quick Revision

- NPM = package manager for JavaScript
- `npm init` creates package.json
- `npm install` installs packages
- `dependencies` vs `devDependencies`
- `package-lock.json` locks versions

---

## Related Topics

- [[What-is-NPM]] - NPM overview
- [[What-is-PackageJSON]] - package.json
- [[Init-Project]] - Initializing project
- [[Install-Packages]] - Installing packages
- [[What-is-SemVer]] - Semantic versioning
