# How to Update npm Packages

## Definition

Updating npm packages means installing newer versions of dependencies to get latest features, bug fixes, and security patches. Managing updates properly is crucial for maintaining a secure and up-to-date project.

## Checking for Updates

```bash
# Check all outdated packages
npm outdated

# Check specific package
npm outdated express

# List global outdated packages
npm outdated -g
```

### Output Format

```
Package    Current  Wanted  Latest  Location
express    4.18.2   4.19.2  4.19.2  my-project
lodash     4.17.19  4.17.21 4.17.21 my-project
jest       29.5.0   29.7.0  29.7.0  my-project
```

| Column | Description |
|--------|-------------|
| Current | Currently installed version |
| Wanted | Latest version matching semver range |
| Latest | Latest version on npm registry |

## Update Commands

### Update All Packages

```bash
# Update all packages within semver range
npm update

# Update specific package
npm update express
```

### Install Latest Version

```bash
# Install latest version (ignores semver range)
npm install express@latest

# Install latest minor version
npm install express@^4.19.2

# Install latest patch version
npm install express@~4.18.2
```

### Update package.json

```bash
# Update version in package.json
npm install express@latest --save
```

## Using npx for Updates

```bash
# Use npx to check and update
npx npm-check-updates

# Interactive update
npx npm-check-updates -i

# Update all packages
npx npm-check-updates -u
npm install
```

## Update Strategies

### Conservative (Recommended for Production)

```bash
# Update only patch versions
npm update --save

# Or manually specify versions
npm install express@~4.18.2
```

### Moderate

```bash
# Update minor and patch versions
npm install express@^4.19.2
```

### Aggressive

```bash
# Update to latest major version (may break changes)
npm install express@latest
```

## Lock File Management

```bash
# After updating, commit the lock file
git add package-lock.json
git commit -m "Update dependencies"

# Use npm ci in CI/CD for consistent installs
npm ci
```

## Security Updates

```bash
# Audit for security vulnerabilities
npm audit

# Fix vulnerabilities automatically
npm audit fix

# Fix with breaking changes
npm audit fix --force
```

## Package.json Version Ranges

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "~4.17.19"
  }
}
```

| Range | Behavior |
|-------|----------|
| `^` | Updates minor and patch versions |
| `~` | Updates only patch versions |
| `*` | Updates any version |
| `1.2.3` | Exact version only |

## Common Use Cases

- Applying security patches
- Getting new features
- Bug fixes
- Performance improvements
- Dependency compatibility

## Common Mistakes

- Not testing after updates
- Updating all packages to latest major versions in production
- Not checking changelog for breaking changes
- Ignoring security audit warnings
- Not committing package-lock.json after updates
- Updating packages without reviewing changelogs

## Related Topics

- [[What-is-PackageJSON]]
- [[Install-Packages]]
- [[What-is-Lock]]
- [[What-is-DevDeps]]
- [[NPM]]
- [[Security]]

## Quick Revision

| Command | Purpose |
|---------|---------|
| `npm outdated` | Check for updates |
| `npm update` | Update within semver range |
| `npm install pkg@latest` | Install latest version |
| `npm audit` | Check for vulnerabilities |
| `npm audit fix` | Fix security issues |
| `npx npm-check-updates` | Interactive update tool |
