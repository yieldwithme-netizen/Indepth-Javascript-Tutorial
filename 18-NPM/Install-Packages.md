# How to Install npm Packages

## Definition

npm packages are reusable code modules published to the npm registry. Installing packages adds external functionality to your project without writing everything from scratch.

## Basic Installation

### Install a Package

```bash
npm install package-name
# or
npm i package-name
```

### Install Specific Version

```bash
npm install package-name@1.2.3
```

### Install Multiple Packages

```bash
npm install express lodash axios
```

## Installation Modes

### Production Dependencies

```bash
npm install express
npm install express --save
# or
npm install express -S
```

Adds to `dependencies` in package.json.

### Development Dependencies

```bash
npm install nodemon --save-dev
# or
npm install nodemon -D
```

Adds to `devDependencies` in package.json.

### Optional Dependencies

```bash
npm install optional-package --save-optional
# or
npm install optional-package -O
```

Adds to `optionalDependencies` in package.json.

## Version Ranges

```json
{
  "dependencies": {
    "exact": "1.2.3",
    "caret": "^1.2.3",
    "tilde": "~1.2.3",
    "greater": ">=1.2.3",
    "less": "<2.0.0",
    "range": ">=1.0.0 <2.0.0",
    "pipe": "1.2.x || 2.0.0",
    "latest": "*"
  }
}
```

| Symbol | Range | Example |
|--------|-------|---------|
| `^` | Compatible with version | ^1.2.3 → >=1.2.3 <2.0.0 |
| `~` | Approximately version | ~1.2.3 → >=1.2.3 <1.3.0 |
| `>` | Greater than | >1.2.3 |
| `>=` | Greater or equal | >=1.2.3 |
| `<` | Less than | <2.0.0 |
| `*` | Any version | * |

## Installing from Different Sources

### Install from Git

```bash
npm install github:user/repo
npm install https://github.com/user/repo.git
```

### Install from Tarball

```bash
npm install ./package.tgz
```

### Install from URL

```bash
npm install https://registry.npmjs.org/package/-/package-1.0.0.tgz
```

## Global Installation

```bash
npm install -g package-name
npm install --global package-name
```

Installed globally, available system-wide.

## Common npm install Commands

```bash
# Install all dependencies from package.json
npm install

# Install and save exact version
npm install package-name --save-exact

# Install and save to specific section
npm install package-name --save-dev

# Dry run (show what would be installed)
npm install --dry-run

# Install with legacy peer deps
npm install --legacy-peer-deps
```

## Managing node_modules

```bash
# Remove a package
npm uninstall package-name
npm remove package-name

# List installed packages
npm list
npm ls

# List globally installed
npm list -g

# Check for outdated packages
npm outdated

# Update a package
npm update package-name
```

## Common Use Cases

- Adding web frameworks (Express, Koa)
- Including utility libraries (Lodash, Moment)
- Setting up development tools (ESLint, Prettier)
- Adding testing frameworks (Jest, Mocha)
- Installing build tools (Webpack, Babel)

## Common Mistakes

- Installing dev dependencies as production dependencies
- Not checking package security before installing
- Using `npm install` without a lock file
- Installing packages globally when project-local is better
- Not reading package documentation before use
- Ignoring security warnings during installation

## Related Topics

- [[What-is-PackageJSON]]
- [[What-is-DevDeps]]
- [[What-is-Lock]]
- [[Update-Packages]]
- [[What-is-NPX]]
- [[NPM]]
- [[Node-Modules]]

## Quick Revision

| Command | Purpose |
|---------|---------|
| `npm install` | Install all dependencies |
| `npm install pkg` | Install production dependency |
| `npm install pkg -D` | Install dev dependency |
| `npm install -g pkg` | Install globally |
| `npm uninstall pkg` | Remove package |
| `npm outdated` | Check for updates |
| `npm list` | List installed packages |
