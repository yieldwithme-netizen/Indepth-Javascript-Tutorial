# How to Compile TypeScript

TypeScript needs to be compiled (transpiled) to JavaScript before it can run in browsers or Node.js. This guide covers different ways to compile TypeScript code.

## Definition

Compiling TypeScript means converting `.ts` files into `.js` files using the TypeScript compiler (`tsc`). The compiler performs type checking and transforms TypeScript-specific syntax into standard JavaScript.

## Compilation Methods

### 1. Using tsc Directly
The TypeScript compiler (`tsc`) is the most basic way to compile TypeScript.

```bash
# Install TypeScript
npm install -g typescript

# Compile a single file
tsc filename.ts

# Compile with tsconfig.json
tsc

# Watch mode - recompile on changes
tsc --watch

# Check types without emitting files
tsc --noEmit
```

### 2. npm Scripts
Add compilation scripts to your `package.json`.

```json
{
    "scripts": {
        "build": "tsc",
        "watch": "tsc --watch",
        "typecheck": "tsc --noEmit",
        "clean": "rm -rf dist"
    }
}
```

```bash
# Run scripts
npm run build
npm run watch
npm run typecheck
```

### 3. Using ts-node
Run TypeScript directly without manual compilation.

```bash
# Install ts-node
npm install -D ts-node

# Run a TypeScript file directly
npx ts-node filename.ts

# Run with tsconfig
npx ts-node --project tsconfig.json filename.ts
```

### 4. With Build Tools

#### Webpack
```javascript
// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.ts',
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    },
    resolve: {
        extensions: ['.ts', '.js']
    },
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
};
```

#### Vite
```typescript
// vite.config.ts
import { defineConfig } from 'vite';

export default defineConfig({
    // Vite has built-in TypeScript support
    // No special configuration needed
});
```

### 5. Babel with TypeScript
```bash
# Install dependencies
npm install --save-dev @babel/core @babel/preset-env @babel/preset-typescript
```

```javascript
// babel.config.js
module.exports = {
    presets: [
        ['@babel/preset-env', { targets: { node: 'current' } }],
        '@babel/preset-typescript'
    ]
};
```

## Common Use Cases

### 1. Production Build
```bash
# Build for production
tsc --project tsconfig.prod.json

# With minification (using webpack)
npm run build:prod
```

### 2. Development Workflow
```bash
# Start development with watch mode
npm run watch

# In another terminal
npm run dev
```

### 3. CI/CD Pipeline
```yaml
# .github/workflows/build.yml
steps:
  - name: Install dependencies
    run: npm ci
  
  - name: Type check
    run: npm run typecheck
  
  - name: Build
    run: npm run build
  
  - name: Test
    run: npm test
```

### 4. Monorepo Setup
```json
{
    "compilerOptions": {
        "composite": true,
        "declaration": true,
        "declarationMap": true
    },
    "references": [
        { "path": "./packages/core" },
        { "path": "./packages/utils" }
    ]
}
```

```bash
# Build all packages
tsc --build

# Build specific package
tsc --build packages/core
```

## Compiler Options for Compilation

```json
{
    "compilerOptions": {
        "outDir": "./dist",        // Output directory
        "rootDir": "./src",        // Source directory
        "declaration": true,       // Generate .d.ts files
        "sourceMap": true,         // Generate source maps
        "removeComments": true,    // Remove comments from output
        "target": "ES2020",        // JavaScript version
        "module": "commonjs"       // Module system
    }
}
```

## Common Mistakes

1. **Not setting `outDir`** - Compiles to source directory:
   ```json
   // Wrong
   {
       "compilerOptions": {
           "rootDir": "./src"
       }
   }
   
   // Correct
   {
       "compilerOptions": {
           "rootDir": "./src",
           "outDir": "./dist"
       }
   }
   ```

2. **Forgetting to run `tsc` after changes** - Using old compiled files:
   ```bash
   # Wrong - forgetting to recompile
   tsc
   # ... make changes ...
   node dist/index.js  # Running old code!
   
   # Better - use watch mode
   tsc --watch
   ```

3. **Not including source maps** - Hard to debug:
   ```json
   // Wrong - no source maps
   {
       "compilerOptions": {
           "sourceMap": false
       }
   }
   
   // Better - enable source maps for debugging
   {
       "compilerOptions": {
           "sourceMap": true
       }
   }
   ```

## Related Topics

- [[What-is-TsConfig]]
- [[Use-With-JS]]
- [[What-is-TypeAnnotation]]
- [[What-is-Interface]]

## Quick Revision

- Use `tsc` to compile TypeScript to JavaScript
- `tsc --watch` watches for changes and recompiles
- `ts-node` runs TypeScript directly without compilation
- Build tools (Webpack, Vite) have TypeScript support
- `outDir` specifies where compiled files go
- `declaration` generates `.d.ts` type definition files
- `sourceMap` enables debugging with original TypeScript source
- Always run type checking in CI/CD pipelines
