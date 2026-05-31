# Webpack

## Definition

Webpack is a **module bundler** that compiles and bundles JavaScript files.

## Basic Config

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    mode: 'development'
};
```

## NPM Scripts

```json
{
    "scripts": {
        "build": "webpack",
        "dev": "webpack serve"
    }
}
```

## Quick Revision

- Webpack = module bundler
- Entry → Output (bundle.js)
- Loaders transform files
- Plugins add features
- Use with npm scripts

---

## Related Topics

- [[What-is-Webpack]] - [[What-is-Webpack|Webpack]]
- [[Webpack]] - [[Webpack|Webpack]]
- [[Configure-Webpack]] - [[Configure-Webpack|Webpack config]]
- [[What-is-Vite]] - [[What-is-Vite|Vite]]
