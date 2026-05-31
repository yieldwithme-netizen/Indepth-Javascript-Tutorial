# What is Webpack?

## Definition

Webpack is a **module bundler** that compiles and bundles JavaScript files for the browser.

## Basic Configuration

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

## Core Concepts

| Concept | Description |
|---------|-------------|
| Entry | Starting point (index.js) |
| Output | Where to put bundled file |
| Loaders | Transform non-JS files |
| Plugins | Additional functionality |
| Mode | Development/Production |

## Loaders

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: 'babel-loader'
            }
        ]
    }
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
- Loaders transform files (CSS, images)
- Plugins add features (minify, etc.)
- Use with npm scripts

---

## Related Topics

- [[What-is-Webpack]] - Webpack overview
- [[Configure-Webpack]] - Webpack config
- [[What-is-Vite]] - Vite (alternative)
- [[What-is-Babel]] - Babel
- [[What-is-ESLint]] - ESLint
