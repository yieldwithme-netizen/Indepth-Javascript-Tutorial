# How to Configure ESLint

## Definition

ESLint configuration is done through `.eslintrc.*` files or `eslint.config.js` to define rules, extends, plugins, and parser options.

## Basic Configuration

```json
// .eslintrc.json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "rules": {
        "no-console": "warn"
    }
}
```

## Configuration Files

| File | Description |
|------|-------------|
| `.eslintrc.js` | JavaScript format |
| `.eslintrc.json` | JSON format |
| `.eslintrc.yaml` | YAML format |
| `eslint.config.js` | Flat config (new) |

## JavaScript Configuration

```javascript
// .eslintrc.js
module.exports = {
    env: {
        browser: true,
        node: true,
        es2021: true
    },
    extends: [
        'eslint:recommended',
        'plugin:react/recommended'
    ],
    parser: '@typescript-eslint/parser',
    parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module',
        ecmaFeatures: {
            jsx: true
        }
    },
    plugins: ['react', '@typescript-eslint'],
    rules: {
        'no-unused-vars': 'error',
        'no-console': 'warn',
        'prefer-const': 'error',
        'no-var': 'error'
    },
    settings: {
        react: {
            version: 'detect'
        }
    }
};
```

## Rule Severity

```javascript
rules: {
    'no-console': 'off',    // Disabled
    'no-unused-vars': 'warn', // Warning
    'no-undef': 'error'      // Error
}
```

## Environment Configuration

```javascript
module.exports = {
    env: {
        browser: true,  // Browser globals
        node: true,     // Node.js globals
        es2021: true,   // ES2021 features
        jest: true      // Jest globals
    }
};
```

## Extending Configurations

```javascript
module.exports = {
    extends: [
        'eslint:recommended',           // ESLint defaults
        'plugin:react/recommended',     // React rules
        'plugin:@typescript-eslint/recommended', // TypeScript
        'prettier'                      // Disables conflicting rules
    ]
};
```

## Common Use Cases

- Setting up [[What-is-ESLint]] for [[What-is-Frameworks|framework]] projects
- Enforcing consistent team coding standards
- Integrating with [[What-is-Prettier|Prettier]]
- Configuring for [[What-is-TypeScript|TypeScript]]
- Setting up CI/CD linting

## Common Mistakes

- Conflicting rules between extends
- Not disabling rules that conflict with [[What-is-Prettier|Prettier]]
- Using wrong parser for TypeScript
- Not setting correct environment globals

## Quick Revision

- Create `.eslintrc.js` or `.eslintrc.json`
- Use `extends` to inherit configurations
- Use `plugins` for framework support
- Use `rules` to customize behavior
- Use `env` to define globals
- Use `prettier` to disable conflicting rules

---

## Related Topics

- [[What-is-ESLint]] - ESLint overview
- [[Configure-ESLint]] - This guide
- [[What-is-Prettier]] - Prettier (code formatter)
- [[Configure-Prettier]] - Prettier configuration
- [[What-is-Babel]] - Babel transpiler
- [[What-is-TypeScript]] - TypeScript