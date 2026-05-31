# What is package-lock.json

## Definition

`package-lock.json` is an automatically generated file that records the exact versions of all dependencies installed in your project. It ensures that every installation produces identical results across different environments and machines.

## Purpose

1. **Dependency Locking** - Pins exact versions of all packages
2. **Reproducibility** - Ensures consistent installations
3. **Speed** - Faster installations using cached packages
4. **Security** - Prevents malicious version hijacking

## File Structure

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "lockfileVersion": 3,
  "requires": true,
  "packages": {
    "": {
      "name": "my-project",
      "version": "1.0.0",
      "dependencies": {
        "express": "^4.18.2"
      }
    },
    "node_modules/express": {
      "version": "4.18.2",
      "resolved": "https://registry.npmjs.org/express/-/express-4.18.2.tgz",
      "integrity": "sha512-...",
      "dependencies": {
        "accepts": "~1.3.8",
        "body-parser": "1.20.1"
      }
    }
  }
}
```

## How It Works

### First Installation

```bash
npm install express
# Creates package-lock.json
# Records exact versions of express and all its dependencies
```

### Subsequent Installations

```bash
npm install
# Uses package-lock.json to install exact same versions
# Ensures consistency across environments
```

## Version Resolution

```json
{
  "node_modules/express": {
    "version": "4.18.2",
    "resolved": "https://registry.npmjs.org/express/-/express-4.18.2.tgz",
    "integrity": "sha512-..."
  }
}
```

| Field | Description |
|-------|-------------|
| version | Exact installed version |
| resolved | URL where package was downloaded |
| integrity | Hash for package verification |

## Managing package-lock.json

### Updating the Lock File

```bash
# Update specific package
npm update express

# Update all packages
npm update

# Regenerate lock file
rm package-lock.json
npm install
```

### Installing with Lock File

```bash
# Install using lock file (exact versions)
npm ci

# Clean install (removes node_modules first)
npm ci
```

## npm ci vs npm install

```bash
# npm install - May update versions based on ranges
npm install

# npm ci - Strictly follows lock file
npm ci
```

| Command | Use Case |
|---------|----------|
| `npm install` | Development, may update versions |
| `npm ci` | CI/CD, strict installation |

## Best Practices

### Commit to Version Control

```bash
# Always commit package-lock.json
git add package-lock.json
git commit -m "Update dependencies"
```

### Use in CI/CD

```yaml
# GitHub Actions example
- name: Install dependencies
  run: npm ci

# Never use npm install in CI/CD
```

### Don't Edit Manually

```bash
# Let npm manage the lock file
# Don't manually edit package-lock.json

# Use npm commands instead
npm install package-name@version
npm uninstall package-name
```

## Common Use Cases

- Ensuring consistent builds across team members
- CI/CD pipeline reliability
- Production deployment consistency
- Debugging dependency issues
- Security auditing

## Common Mistakes

- Not committing package-lock.json to version control
- Using `npm install` instead of `npm ci` in CI/CD
- Manually editing the lock file
- Ignoring lock file conflicts in Git
- Not regenerating lock file when changing Node versions
- Deleting lock file to fix installation issues

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[Update-Packages]]
- [[NPM]]
- [[Node-Modules]]
- [[CI-CD]]
- [[Version-Control]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| package-lock.json | Records exact dependency versions |
| lockfileVersion | Lock file format version |
| npm ci | Install using lock file exactly |
| npm install | May update based on version ranges |
| Commit lock file | Always commit to version control |
