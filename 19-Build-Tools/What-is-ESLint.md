# What is ESLint?

## Definition

ESLint is a **pluggable linting utility** for JavaScript that finds and reports code patterns, helping maintain code quality and consistency.

## Key Features

| Feature | Description |
|---------|-------------|
| Code Analysis | Detects problematic patterns |
| Auto-fix | Automatically fixes many issues |
| Custom Rules | Create your own linting rules |
| Framework Support | Works with React, Vue, etc. |
| Integrations | Works with editors and CI/CD |

## Basic Usage

```bash
# Install ESLint
npm install --save-dev eslint

# Initialize ESLint
npx eslint --init

# Lint a file
npx eslint src/index.js

# Fix issues
npx eslint src/index.js --fix
```

## Sample Rules

```javascript
// .eslintrc.js
module.exports = {
    rules: {
        'no-console': 'warn',
        'no-unused-vars': 'error',
        'eqeqeq': 'error',
        'no-var': 'error',
        'prefer-const': 'error'
    }
};
```

## Extends and Plugins

```javascript
module.exports = {
    extends: [
        'eslint:recommended',
        'plugin:react/recommended',
        'plugin:@typescript-eslint/recommended'
    ],
    plugins: ['react', '@typescript-eslint'],
    parser: '@typescript-eslint/parser'
};
```

## Common Use Cases

- Enforcing consistent [[What-is-JavaScript|JavaScript]] style
- Catching bugs before runtime
- Maintaining team coding standards
- Integrating with [[What-is-Frameworks|framework]] projects
- CI/CD code quality gates

## Common Mistakes

- Using too many rules (start simple)
- Not understanding rule severity (off, warn, error)
- Forgetting to install parser/plugins
- Confusing ESLint with [[What-is-Prettier|Prettier]]

## Quick Revision

- ESLint = JavaScript linter
- Finds and fixes code issues
- Uses `.eslintrc` configuration
- Extends recommended configs
- Plugins for frameworks (React, Vue)
- Integrates with editors and CI/CD

---

## Related Topics

- [[What-is-ESLint]] - This guide
- [[Configure-ESLint]] - ESLint configuration
- [[What-is-Prettier]] - Code formatter
- [[Configure-Prettier]] - Prettier configuration
- [[What-is-JavaScript]] - JavaScript basics
- [[What-is-NodeJS]] - Node.js