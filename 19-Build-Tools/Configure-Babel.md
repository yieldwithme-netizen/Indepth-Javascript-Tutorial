# How to Configure Babel

## Definition

Babel configuration is done through `.babelrc`, `babel.config.js`, or `package.json` to define presets, plugins, and compilation options.

## Basic Configuration

```json
// .babelrc
{
    "presets": ["@babel/preset-env"]
}
```

## Configuration Files

| File | Description |
|------|-------------|
| `.babelrc` | Project-specific config |
| `babel.config.js` | Programmatic config |
| `babel.config.json` | JSON format config |
| `package.json` | Using `babel` key |

## JavaScript Configuration

```javascript
// babel.config.js
module.exports = {
    presets: [
        ['@babel/preset-env', {
            useBuiltIns: 'usage',
            corejs: 3
        }]
    ],
    plugins: [
        '@babel/plugin-proposal-class-properties'
    ]
};
```

## Preset Configuration

```javascript
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                browsers: ['last 2 versions', 'not dead']
            },
            modules: false,
            useBuiltIns: 'usage'
        }],
        '@babel/preset-react',
        '@babel/preset-typescript'
    ]
};
```

## Plugin Configuration

```javascript
module.exports = {
    plugins: [
        ['@babel/plugin-proposal-decorators', { legacy: true }],
        ['@babel/plugin-proposal-class-properties', { loose: true }],
        '@babel/plugin-syntax-dynamic-import'
    ]
};
```

## Environment-Specific Config

```javascript
module.exports = function(api) {
    api.cache(true);

    const presets = ['@babel/preset-env'];
    const plugins = ['@babel/plugin-proposal-class-properties'];

    if (process.env.NODE_ENV === 'development') {
        plugins.push('react-hot-loader/babel');
    }

    return { presets, plugins };
};
```

## Common Use Cases

- Setting up [[What-is-Babel]] for [[What-is-Webpack|Webpack]] projects
- Configuring for [[What-is-Vite|Vite]] projects
- Supporting specific browser versions
- Using experimental JavaScript features
- Integrating with [[What-is-TypeScript|TypeScript]]

## Common Mistakes

- Mixing up `.babelrc` and `babel.config.js` scopes
- Not installing `@babel/core` dependency
- Using `useBuiltIns: 'entry'` without core-js
- Forgetting to set `modules: false` for tree-shaking

## Quick Revision

- Create `.babelrc` or `babel.config.js`
- Use `presets` for feature sets (env, react, typescript)
- Use `plugins` for specific transforms
- Configure browser targets in preset-env
- Use `useBuiltIns: 'usage'` for polyfills
- Test configuration with `npx babel src --out-dir dist`

---

## Related Topics

- [[What-is-Babel]] - Babel overview
- [[Configure-Babel]] - This guide
- [[What-is-Webpack]] - Webpack (uses Babel)
- [[Configure-Webpack]] - Webpack configuration
- [[What-is-Vite]] - Vite
- [[What-is-TypeScript]] - TypeScript