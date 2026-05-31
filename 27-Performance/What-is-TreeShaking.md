# What is Tree Shaking

## Definition

**Tree shaking** is a dead code elimination technique that removes unused exports from your bundle during the build process, resulting in smaller output.

## How It Works

Tree shaking relies on **ES Module static structure**:

```javascript
// math.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }

// app.js
import { add } from "./math.js";
console.log(add(1, 2));
// Tree shaking removes subtract and multiply from final bundle
```

## Enabling Tree Shaking

### Webpack

```javascript
// webpack.config.js
module.exports = {
  mode: "production", // Enables tree shaking
  optimization: {
    usedExports: true,
  },
};
```

### Rollup

```javascript
// rollup.config.js (tree shaking enabled by default)
export default {
  input: "src/index.js",
  output: { file: "dist/bundle.js", format: "es" },
};
```

### Vite

```javascript
// vite.config.js (tree shaking enabled by default)
export default {
  build: {
    rollupOptions: {
      output: {
        manualChunks: undefined,
      },
    },
  },
};
```

## Side Effects Configuration

```json
// package.json
{
  "sideEffects": false
}
```

```javascript
// If some files have side effects
{
  "sideEffects": ["*.css", "./src/polyfill.js"]
}
```

## Common Use Cases

- **Library usage**: Import only needed functions
- **Utility modules**: Remove unused helpers
- **Component libraries**: Import specific components

## Common Mistakes

```javascript
// Mistake: CommonJS modules (not tree-shakeable)
const _ = require("lodash"); // Entire library included
_.map([1, 2, 3], (n) => n * 2);

// Fix: Use ES modules or named imports
import { map } from "lodash-es";
map([1, 2, 3], (n) => n * 2);

// Mistake: Side effects prevent shaking
// math.js
export const add = (a, b) => a + b;
console.log("Module loaded!"); // Side effect prevents tree shaking
```

## Related Topics

- [[What-is-CodeSplitting]]
- [[What-is-LazyLoading]]
- [[Optimize-Rendering]]
- [[Measure-Performance]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| What | Remove unused code |
| Requirement | ES Modules (static imports) |
| Config | `sideEffects: false` in package.json |
| Best with | Code splitting + lazy loading |
| Enemy | CommonJS `require()`, side effects |
