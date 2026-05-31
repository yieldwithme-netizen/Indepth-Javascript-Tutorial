# Tree Shaking

## Definition

Tree shaking **removes unused code** from bundles.

## How It Works

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// app.js
import { add } from './math.js';
// Only add() is included in bundle
// subtract() is removed
```

## Quick Revision

- Tree shaking = dead code elimination
- Requires ES modules
- Webpack/Rollup support
- Import only what you use

---

## Related Topics

- [[What-is-TreeShaking]] - [[What-is-TreeShaking|Tree shaking]]
- [[Tree-Shaking]] - [[Tree-Shaking|Tree shaking]]
- [[What-is-Webpack]] - [[What-is-Webpack|Webpack]]
- [[What-is-Vite]] - [[What-is-Vite|Vite]]
