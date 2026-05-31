# Bundlers

## Definition

Bundlers **combine JavaScript modules** into optimized files for browsers.

## Popular Bundlers

| Bundler | Description |
|---------|-------------|
| Webpack | Most popular, highly configurable |
| Vite | Fast, modern, uses ES modules |
| Rollup | Best for libraries |
| Parcel | Zero-config |
| esbuild | Extremely fast |

## Why Bundle

```javascript
// Without bundler (multiple requests)
<script src="utils.js"></script>
<script src="api.js"></script>
<script src="app.js"></script>

// With bundler (single request)
<script src="bundle.js"></script>
```

## Quick Revision

- Bundlers combine modules into one file
- Reduces HTTP requests
- Enables tree shaking
- Minifies code
- Transpiles modern JS

---

## Related Topics

- [[What-is-Webpack]] - [[What-is-Webpack|Webpack]]
- [[What-is-Vite]] - [[What-is-Vite|Vite]]
- [[What-is-TreeShaking]] - [[What-is-TreeShaking|Tree shaking]]
- [[Build-Tools]] - [[Build-Tools|Build tools]]
