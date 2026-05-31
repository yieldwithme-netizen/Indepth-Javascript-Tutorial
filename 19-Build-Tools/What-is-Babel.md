# What is Babel?

## Definition

Babel is a **JavaScript compiler** that transpiles modern JavaScript (ES6+) into backward-compatible versions for older browsers.

## Key Features

| Feature | Description |
|---------|-------------|
| Transpilation | Converts ES6+ to ES5 |
| JSX Support | Transforms JSX syntax |
| TypeScript | Strips type annotations |
| Plugins | Modular transformation system |
| Presets | Collections of plugins |

## Basic Usage

```bash
# Install Babel
npm install --save-dev @babel/core @babel/cli @babel/preset-env

# Transpile a file
npx babel src/index.js --out-file dist/index.js
```

## How It Works

```javascript
// Input (ES6)
const greet = (name) => `Hello, ${name}!`;

// Output (ES5)
var greet = function greet(name) {
    return "Hello, " + name + "!";
};
```

## Presets

```javascript
// .babelrc
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react",
        "@babel/preset-typescript"
    ]
}
```

## Plugins

```javascript
// .babelrc
{
    "plugins": [
        "@babel/plugin-proposal-class-properties",
        "@babel/plugin-proposal-optional-chaining"
    ]
}
```

## Browser Targets

```javascript
// babel.config.js
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                chrome: '58',
                firefox: '57',
                safari: '11'
            }
        }]
    ]
};
```

## Common Use Cases

- Supporting old browsers in [[What-is-Frameworks|framework]] projects
- Using [[What-is-JavaScript|JavaScript]] features not yet widely supported
- Transpiling JSX for [[What-is-React|React]] projects
- Using TypeScript without full compilation
- Processing code with [[What-is-Webpack|Webpack]] or [[What-is-Vite|Vite]]

## Common Mistakes

- Confusing compilation with transpilation
- Not including necessary plugins for features used
- Forgetting to configure browser targets
- Using Babel when [[What-is-TypeScript|TypeScript]] compiler is sufficient

## Quick Revision

- Babel = JavaScript transpiler
- Converts modern JS to older versions
- Uses presets (preset-env, preset-react)
- Uses plugins for specific features
- Essential for browser compatibility
- Integrates with [[What-is-Webpack|Webpack]] and [[What-is-Vite|Vite]]

---

## Related Topics

- [[What-is-Babel]] - This guide
- [[Configure-Babel]] - Babel configuration
- [[What-is-Webpack]] - Webpack (uses Babel)
- [[Configure-Webpack]] - Webpack configuration
- [[What-is-Vite]] - Vite (alternative)
- [[What-is-TypeScript]] - TypeScript