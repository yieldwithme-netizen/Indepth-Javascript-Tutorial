# How to Configure Prettier

## Definition

Prettier configuration is done through `.prettierrc` or `prettier.config.js` to customize formatting options like tabs, semicolons, and quotes.

## Basic Configuration

```json
// .prettierrc
{
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2,
    "trailingComma": "es5"
}
```

## Configuration Files

| File | Description |
|------|-------------|
| `.prettierrc` | JSON format |
| `.prettierrc.js` | JavaScript format |
| `prettier.config.js` | JavaScript format |
| `.prettierrc.json` | JSON format |
| `.prettierrc.yaml` | YAML format |

## JavaScript Configuration

```javascript
// prettier.config.js
module.exports = {
    semi: true,
    singleQuote: true,
    tabWidth: 2,
    useTabs: false,
    trailingComma: 'es5',
    printWidth: 80,
    bracketSpacing: true,
    arrowParens: 'avoid',
    endOfLine: 'lf'
};
```

## Common Options

```javascript
module.exports = {
    // Semicolons
    semi: true,                    // true: ;  false: (none)

    // Quotes
    singleQuote: true,             // true: '  false: "
    quoteProps: 'as-needed',       // as-needed, consistent, consistent-as-needed

    // Indentation
    tabWidth: 2,                   // Number of spaces
    useTabs: false,                // true: tabs  false: spaces

    // Line Length
    printWidth: 80,                // Max line length

    // Trailing Commas
    trailingComma: 'es5',          // none, es5, all

    // Spacing
    bracketSpacing: true,          // { foo: true } vs {foo: true}

    // Arrow Functions
    arrowParens: 'avoid',          // (x) => {} vs x => {}

    // Line Endings
    endOfLine: 'lf'                // lf, crlf, cr
};
```

## Ignore Files

```markdown
// .prettierignore
node_modules
dist
build
*.min.js
package-lock.json
```

## Editor Integration

```json
// .vscode/settings.json
{
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
}
```

## Common Use Cases

- Standardizing [[What-is-JavaScript|JavaScript]] code style
- Working with [[What-is-ESLint|ESLint]] (disable conflicting rules)
- Formatting [[What-is-Frameworks|framework]] code
- Auto-formatting on save
- Enforcing team style guide

## Common Mistakes

- Not using `.prettierignore` for generated files
- Conflicting rules with [[What-is-ESLint|ESLint]]
- Setting `arrowParens: 'avoid'` with multi-line params
- Not configuring editor integration

## Quick Revision

- Create `.prettierrc` in project root
- Key options: `semi`, `singleQuote`, `trailingComma`
- Use `.prettierignore` for files to skip
- Configure editor for auto-format on save
- Works alongside [[What-is-ESLint|ESLint]] (use `eslint-config-prettier`)
- No config needed for defaults

---

## Related Topics

- [[What-is-Prettier]] - Prettier overview
- [[Configure-Prettier]] - This guide
- [[What-is-ESLint]] - ESLint (linter)
- [[Configure-ESLint]] - ESLint configuration
- [[What-is-JavaScript]] - JavaScript basics
- [[What-is-NodeJS]] - Node.js