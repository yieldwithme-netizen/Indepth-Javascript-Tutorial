# How to Configure Vite

## Definition

Vite configuration is done through `vite.config.js` (or `.ts`) file with a minimal, sensible-defaults approach.

## Basic Configuration

```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
    root: './src',
    base: '/',
    build: {
        outDir: 'dist'
    }
});
```

## Configuration Options

| Option | Description |
|--------|-------------|
| root | Root directory of the project |
| base | Base public path for production |
| build | Build options |
| server | Dev server options |
| plugins | Vite plugins |
| resolve | Module resolution |

## Server Configuration

```javascript
export default defineConfig({
    server: {
        port: 3000,
        open: true,
        host: true,
        proxy: {
            '/api': {
                target: 'http://localhost:3001',
                changeOrigin: true
            }
        }
    }
});
```

## Build Configuration

```javascript
export default defineConfig({
    build: {
        outDir: 'dist',
        sourcemap: true,
        minify: 'terser',
        rollupOptions: {
            output: {
                manualChunks: {
                    vendor: ['lodash', 'axios']
                }
            }
        }
    }
});
```

## CSS Configuration

```javascript
export default defineConfig({
    css: {
        modules: {
            localsConvention: 'camelCase'
        },
        preprocessorOptions: {
            scss: {
                additionalData: `@import "src/variables.scss";`
            }
        }
    }
});
```

## Environment Variables

```bash
# .env
VITE_API_URL=https://api.example.com

# Access in code
console.log(import.meta.env.VITE_API_URL);
```

## Common Use Cases

- Setting up [[What-is-Vite]] for [[What-is-Frameworks|framework]] projects
- Configuring proxy for API calls
- Setting up [[What-is-Babel|Babel]] or other transpilers
- Optimizing production builds
- Configuring environment variables

## Common Mistakes

- Using `process.env` instead of `import.meta.env`
- Not prefixing env vars with `VITE_`
- Confusing Vite config with Webpack config syntax
- Missing `import { defineConfig }` for TypeScript support

## Quick Revision

- Create `vite.config.js` in project root
- Use `defineConfig()` for type safety
- `server` for dev options
- `build` for production options
- `plugins` for extending functionality
- Environment variables use `import.meta.env`

---

## Related Topics

- [[What-is-Vite]] - Vite overview
- [[Configure-Vite]] - This guide
- [[What-is-Webpack]] - Webpack (alternative)
- [[Configure-Webpack]] - Webpack configuration
- [[What-is-Babel]] - Babel transpiler
- [[Configure-Babel]] - Babel configuration