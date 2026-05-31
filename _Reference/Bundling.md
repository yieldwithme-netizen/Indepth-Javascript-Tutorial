# Bundling

## Definition

Bundling is the process of **combining multiple JavaScript files** into a single optimized file.

## Why Bundle

```javascript
// Without bundler (multiple requests)
<script src="utils.js"></script>
<script src="api.js"></script>
<script src="app.js"></script>

// With bundler (single request)
<script src="bundle.js"></script>
```

## Benefits

- Reduces HTTP requests
- Enables tree shaking
- Minifies code
- Transpiles modern JS
- Handles dependencies

## Tools

- Webpack
- Vite
- Rollup
- esbuild

## Quick Revision

- Bundling = combining files
- Reduces HTTP requests
- Enables optimization
- Use Webpack or Vite

---

## Related Topics

- [[Bundlers]] - [[Bundlers|Bundlers]]
- [[What-is-Webpack]] - [[What-is-Webpack|Webpack]]
- [[What-is-Vite]] - [[What-is-Vite|Vite]]
- [[What-is-TreeShaking]] - [[What-is-TreeShaking|Tree shaking]]
