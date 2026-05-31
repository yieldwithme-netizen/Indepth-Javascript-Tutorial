# What is Semantic Versioning?

## Definition

Semantic Versioning (SemVer) is a **version numbering system** that uses a three-part format to indicate major, minor, and patch changes.

## Version Format

```
MAJOR.MINOR.PATCH

Example: 2.4.1
├── Major: 2
├── Minor: 4
└── Patch: 1
```

## Version Types

| Type | Change | Example |
|------|--------|---------|
| MAJOR | Breaking changes | 1.0.0 → 2.0.0 |
| MINOR | New features (backward compatible) | 1.0.0 → 1.1.0 |
| PATCH | Bug fixes (backward compatible) | 1.0.0 → 1.0.1 |

## NPM Version Symbols

```json
{
    "dependencies": {
        "lodash": "^4.17.21",    // Major version: >=4.0.0 <5.0.0
        "axios": "~0.21.4",      // Minor version: >=0.21.4 <0.22.0
        "express": "4.18.2"      // Exact version
    }
}
```

| Symbol | Range | Description |
|--------|-------|-------------|
| `^` | Compatible with version | Allows patches and minors |
| `~` | Approximately version level | Allows patches only |
| (none) | Exact version | No automatic updates |

## Version Ranges

```javascript
// package.json examples
{
    "dependencies": {
        "package-a": "^1.2.3",    // >=1.2.3 <2.0.0
        "package-b": "~1.2.3",    // >=1.2.3 <1.3.0
        "package-c": ">=1.2.3",   // 1.2.3 or higher
        "package-d": "1.2.3"      // Exactly 1.2.3
    }
}
```

## Pre-release Versions

```
1.0.0-alpha.1
1.0.0-beta.2
1.0.0-rc.1
```

| Tag | Description |
|-----|-------------|
| alpha | Early testing, unstable |
| beta | Feature complete, testing |
| rc | Release candidate, near stable |

## Common Use Cases

- Managing [[What-is-NPM|NPM]] dependencies
- Understanding [[Install-Packages|package updates]]
- Planning [[Update-Packages|version upgrades]]
- Writing [[Create-Scripts|version scripts]]
- Communicating changes to users

## Common Mistakes

- Using `^` when you need exact versions
- Not updating versions for breaking changes
- Forgetting to update version before publishing
- Not testing with `npm install` after version changes

## Quick Revision

- MAJOR.MINOR.PATCH format
- MAJOR = breaking changes
- MINOR = new features
- PATCH = bug fixes
- `^` allows minor and patch updates
- `~` allows only patch updates
- Pre-release: alpha, beta, rc

---

## Related Topics

- [[What-is-SemVer]] - This guide
- [[What-is-NPM]] - NPM basics
- [[What-is-PackageJSON]] - package.json
- [[Install-Packages]] - Installing packages
- [[Update-Packages]] - Updating packages
- [[What-is-Lock]] - Lock files