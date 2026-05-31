# Build Tools

Build tools automate tasks like bundling, transpiling, minifying, and optimizing code for production. They are essential for modern JavaScript development.

## Definition

Build tools process source code to create optimized bundles for browsers. They handle module bundling, transpilation (ES6+ to ES5), minification, code splitting, and development server functionality.

## Webpack

```javascript
// webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
        clean: true
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                type: 'asset/resource'
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ],
    devServer: {
        static: './dist',
        hot: true
    }
};
```

## Vite

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
    plugins: [react()],
    server: {
        port: 3000,
        open: true
    },
    build: {
        outDir: 'dist',
        sourcemap: true
    },
    resolve: {
        alias: {
            '@': path.resolve(__dirname, './src')
        }
    }
});
```

## Babel

```javascript
// babel.config.js
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: '> 0.25%, not dead',
            useBuiltIns: 'usage',
            corejs: 3
        }],
        '@babel/preset-react'
    ],
    plugins: [
        '@babel/plugin-proposal-class-properties',
        '@babel/plugin-syntax-dynamic-import'
    ]
};
```

## Rollup

```javascript
// rollup.config.js
export default {
    input: 'src/index.js',
    output: {
        file: 'dist/bundle.js',
        format: 'cjs'
    },
    plugins: [
        resolve(),
        commonjs(),
        terser()
    ]
};
```

## ESLint & Prettier

```javascript
// .eslintrc.js
module.exports = {
    env: {
        browser: true,
        es2021: true,
        node: true
    },
    extends: ['eslint:recommended', 'plugin:react/recommended'],
    parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module'
    },
    rules: {
        'no-unused-vars': 'warn',
        'no-console': 'warn'
    }
};

// .prettierrc
{
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2,
    "trailingComma": "es5"
}
```

## Package.json Scripts

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview",
        "lint": "eslint src --ext .js,.jsx",
        "lint:fix": "eslint src --ext .js,.jsx --fix",
        "format": "prettier --write \"src/**/*.{js,jsx}\"",
        "test": "vitest",
        "test:coverage": "vitest --coverage"
    }
}
```

## Common Build Tasks

```javascript
// npm scripts for common tasks
// package.json
{
    "scripts": {
        "clean": "rm -rf dist",
        "build:dev": "webpack --mode development",
        "build:prod": "webpack --mode production",
        "watch": "webpack --watch",
        "start": "webpack serve --open"
    }
}
```

## Common Use Cases

- Bundling modules for browser
- Transpiling modern JavaScript
- Minifying and compressing assets
- Code splitting for lazy loading
- Hot module replacement in development
- Linting and code formatting

## Common Mistakes

1. **Not using production mode** - Missing optimizations
2. **Ignoring source maps** - Hard to debug production issues
3. **Large bundle sizes** - Not code splitting or tree shaking
4. **Missing polyfills** - Browser compatibility issues
5. **Not caching builds** - Slow development workflow

## Related Topics

- [[Module System]]
- [[npm]]
- [[Bundling]]
- [[Transpilation]]
- [[Development Server]]

## Quick Revision

| Tool | Purpose |
|------|---------|
| Webpack | Module bundler with plugins |
| Vite | Fast dev server and bundler |
| Babel | JavaScript transpiler |
| Rollup | Module bundler for libraries |
| ESLint | Code linting |
| Prettier | Code formatting |
| Parcel | Zero-config bundler |
