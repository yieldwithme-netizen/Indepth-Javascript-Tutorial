# How to Configure Webpack

## Definition

Configuring Webpack involves creating a `webpack.config.js` file that defines entry points, output settings, loaders, plugins, and other build options.

## Basic Configuration

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
        clean: true
    },
    mode: 'development',
    devtool: 'source-map'
};
```

## Configuration Options

| Option | Description |
|--------|-------------|
| entry | Starting point(s) for the bundle |
| output | Where and how to output the bundle |
| module | Rules for transforming files |
| plugins | Additional build functionality |
| mode | `development` or `production` |
| devtool | Source map type |

## Loaders Configuration

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/i,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.(png|svg|jpg|jpeg|gif)$/i,
                type: 'asset/resource'
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            }
        ]
    }
};
```

## Plugins Configuration

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new MiniCssExtractPlugin({
            filename: 'styles.css'
        })
    ]
};
```

## Dev Server Configuration

```javascript
module.exports = {
    devServer: {
        static: './dist',
        port: 3000,
        hot: true,
        open: true,
        historyApiFallback: true
    }
};
```

## Multi-Entry Configuration

```javascript
module.exports = {
    entry: {
        main: './src/index.js',
        vendor: './src/vendor.js'
    },
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
};
```

## Common Use Cases

- Setting up [[What-is-Webpack]] for a new project
- Configuring [[What-is-Babel]] transpilation
- Adding CSS processing with loaders
- Optimizing production builds
- Setting up development server with hot reload

## Common Mistakes

- Incorrect `path.resolve()` usage for output
- Missing `exclude: /node_modules/` in loader rules
- Not setting proper `mode` (development/production)
- Forgetting to install loader dependencies

## Quick Revision

- Create `webpack.config.js` in project root
- Define entry, output, mode at minimum
- Use `module.rules` for loaders
- Use `plugins` array for plugins
- Use `devServer` for local development
- `path.resolve(__dirname, 'dist')` for absolute paths

---

## Related Topics

- [[What-is-Webpack]] - Webpack overview
- [[Configure-Webpack]] - This guide
- [[What-is-Vite]] - Vite (modern alternative)
- [[Configure-Vite]] - Vite configuration
- [[What-is-Babel]] - Babel transpiler
- [[Configure-Babel]] - Babel configuration
- [[Install-Packages]] - Installing dependencies