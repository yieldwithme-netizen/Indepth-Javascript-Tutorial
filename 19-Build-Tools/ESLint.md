# ESLint

## Definition

ESLint **analyzes code** for errors and enforces standards.

## Setup

```bash
npm install eslint --save-dev
npx eslint --init
npx src/
```

## Config

```json
{
    "env": { "browser": true, "node": true },
    "extends": "eslint:recommended",
    "rules": {
        "no-unused-vars": "warn",
        "no-console": "off"
    }
}
```

## Quick Revision

- ESLint = JavaScript linter
- Catches errors early
- Enforces coding standards
- Highly configurable

---

## Related Topics

- [[What-is-ESLint]] - [[What-is-ESLint|ESLint]]
- [[ESLint]] - [[ESLint|ESLint]]
- [[Configure-ESLint]] - [[Configure-ESLint|ESLint config]]
- [[What-is-Prettier]] - [[What-is-Prettier|Prettier]]
