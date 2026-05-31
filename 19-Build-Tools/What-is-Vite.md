# What is Vite?

## Definition

Vite is a **modern frontend build tool** that provides fast development server and optimized production builds using native ES modules.

## Key Features

| Feature | Description |
|---------|-------------|
| Lightning Fast | Instant server start via native ES modules |
| Hot Module Replacement | Fast HMR without full page reload |
| Optimized Build | Uses Rollup for production bundling |
| TypeScript | Native TypeScript support |
| JSX | Built-in JSX support |
| CSS | PostCSS and CSS Modules support |

## Basic Usage

```bash
# Create new Vite project
npm create vite@latest my-app

# Install dependencies
cd my-app
npm install

# Start development server
npm run dev

# Build for production
npm run build
```

## How It Works

```javascript
// During development - no bundling!
// Vite serves files directly using native ES modules
import { createApp } from 'vue'; // Browser fetches this directly
```

## Vite vs Webpack

| Aspect | Vite | Webpack |
|--------|------|---------|
| Dev Server | Instant start | Slow startup |
| HMR | Very fast | Slower |
| Bundling | Rollup | Custom |
| Config | Minimal | Complex |
| Ecosystem | Growing | Mature |

## Plugin System

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    plugins: [vue()]
});
```

## Common Use Cases

- Modern [[What-is-Frameworks|framework]] projects (Vue, React, Svelte)
- Fast prototyping and development
- Projects needing quick startup times
- Modern [[What-is-JavaScript|JavaScript]] applications

## Common Mistakes

- Confusing Vite with Webpack (different approach)
- Not understanding Vite uses native ES modules in dev
- Expecting Webpack plugins to work with Vite

## Quick Revision

- Vite = fast modern build tool
- Uses native ES modules in development
- Rollup for production bundling
- Minimal configuration needed
- Faster than Webpack for dev server
- Supports TypeScript, JSX, CSS out of the box

---

## Related Topics

- [[What-is-Vite]] - This guide
- [[Configure-Vite]] - Vite configuration
- [[What-is-Webpack]] - Webpack (alternative)
- [[Configure-Webpack]] - Webpack configuration
- [[What-is-NPM]] - Package management
- [[What-is-Babel]] - Babel transpiler