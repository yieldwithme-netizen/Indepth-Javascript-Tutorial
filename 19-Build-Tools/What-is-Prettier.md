# What is Prettier?

## Definition

Prettier is an **opinionated code formatter** that enforces consistent style by parsing and reformatting code.

## Key Features

| Feature | Description |
|---------|-------------|
| Opinionated | No configuration debates |
| Multi-language | JS, TS, CSS, HTML, JSON, MD |
| Auto-fix | Formats on save |
| Integrations | Works with ESLint, editors |
| Consistent | Team-wide code style |

## Basic Usage

```bash
# Install Prettier
npm install --save-dev prettier

# Format a file
npx prettier --write src/index.js

# Format all files
npx prettier --write "src/**/*.{js,ts,jsx,tsx}"

# Check without writing
npx prettier --check src/
```

## Sample Output

```javascript
// Input
const   foo={bar:1,baz:2};

// Output
const foo = { bar: 1, baz: 2 };
```

## Supported Languages

- JavaScript (ES6+)
- TypeScript
- JSX/TSX
- CSS/SCSS/Less
- HTML
- JSON/JSON5
- Markdown
- YAML

## Common Use Cases

- Enforcing consistent [[What-is-JavaScript|JavaScript]] style
- Eliminating style debates in teams
- Working alongside [[What-is-ESLint|ESLint]]
- Formatting [[What-is-Frameworks|framework]] code (React, Vue)
- Auto-formatting on save in editors

## Common Mistakes

- Confusing Prettier with [[What-is-ESLint|ESLint]] (formatting vs linting)
- Using both tools with conflicting rules
- Not configuring [[What-is-ESLint|ESLint]] to work with Prettier
- Expecting Prettier to fix logical errors

## Quick Revision

- Prettier = code formatter (opinionated)
- Formats code consistently
- Supports many languages
- Works with [[What-is-ESLint|ESLint]] (disable conflicting rules)
- Use editor integration for auto-format
- No configuration needed (defaults are good)

---

## Related Topics

- [[What-is-Prettier]] - This guide
- [[Configure-Prettier]] - Prettier configuration
- [[What-is-ESLint]] - ESLint (linter)
- [[Configure-ESLint]] - ESLint configuration
- [[What-is-JavaScript]] - JavaScript basics
- [[What-is-Frameworks]] - Frameworks