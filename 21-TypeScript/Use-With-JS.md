# How to Use TypeScript with JavaScript

TypeScript can be gradually adopted into existing JavaScript projects. This guide covers different approaches to integrate TypeScript into your JavaScript codebase.

## Definition

Using TypeScript with JavaScript means leveraging TypeScript's type system while still working with JavaScript files. TypeScript can check JavaScript files, provide autocomplete, and help catch errors without requiring a full migration.

## Approaches

### 1. JavaScript Files with JSDoc Comments
TypeScript can check JavaScript files using JSDoc annotations.

```javascript
// @ts-check

/**
 * Adds two numbers together
 * @param {number} a - First number
 * @param {number} b - Second number
 * @returns {number} The sum
 */
function add(a, b) {
    return a + b;
}

// TypeScript will catch type errors
const result = add(5, "10"); // Error: Argument of type string is not assignable to parameter of type number
```

### 2. Mixing .js and .ts Files
Configure your project to allow both file types.

```typescript
// tsconfig.json
{
    "compilerOptions": {
        "allowJs": true,
        "checkJs": true,
        "noEmit": true,
        "strict": true
    },
    "include": ["src/**/*"]
}
```

```javascript
// utils.js - JavaScript file
export function formatDate(date) {
    return date.toISOString();
}
```

```typescript
// app.ts - TypeScript file
import { formatDate } from './utils';

const date = formatDate(new Date()); // Works
const wrong = formatDate("2024-01-01"); // TypeScript catches the error
```

### 3. Gradual Migration
Convert files one by one from .js to .ts.

```bash
# Step 1: Enable strict settings gradually
# Step 2: Rename .js files to .ts
# Step 3: Add type annotations
# Step 4: Fix type errors
```

```typescript
// Before: utils.js
// export function calculateTotal(items) {
//     return items.reduce((sum, item) => sum + item.price, 0);
// }

// After: utils.ts
interface CartItem {
    price: number;
    quantity: number;
}

export function calculateTotal(items: CartItem[]): number {
    return items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
}
```

### 4. Third-Party Type Definitions
Install type definitions for libraries you use.

```bash
# Install type definitions
npm install --save-dev @types/react
npm install --save-dev @types/node
npm install --save-dev @types/lodash
```

```typescript
// With types installed, you get autocomplete and type checking
import _ from 'lodash';

// TypeScript knows this returns a number
const result = _.sum([1, 2, 3]);

// TypeScript catches errors
const wrong = _.sum("not an array"); // Error
```

### 5. Using TypeScript in Build Tools
Configure popular build tools to work with TypeScript.

```javascript
// webpack.config.js
module.exports = {
    resolve: {
        extensions: ['.ts', '.js']
    },
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    }
};
```

```javascript
// babel.config.js
module.exports = {
    presets: [
        '@babel/preset-env',
        '@babel/preset-typescript'
    ]
};
```

## Common Use Cases

### React with TypeScript
```typescript
// Component with TypeScript
interface ButtonProps {
    label: string;
    onClick: () => void;
    disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({ label, onClick, disabled }) => {
    return (
        <button onClick={onClick} disabled={disabled}>
            {label}
        </button>
    );
};
```

### Node.js with TypeScript
```typescript
// server.ts
import express from 'express';

const app = express();

app.get('/api/users', (req, res) => {
    const users: User[] = [
        { id: 1, name: 'John' },
        { id: 2, name: 'Jane' }
    ];
    res.json(users);
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

## Common Mistakes

1. **Not enabling `checkJs`** - TypeScript won't check .js files:
   ```json
   // Wrong
   {
       "compilerOptions": {
           "allowJs": true
           // Missing checkJs: true
       }
   }
   
   // Correct
   {
       "compilerOptions": {
           "allowJs": true,
           "checkJs": true
       }
   }
   ```

2. **Ignoring TypeScript errors** - defeats the purpose:
   ```typescript
   // @ts-ignore - Don't do this!
   const result = add(5, "10");
   
   // Fix the actual error instead
   const result = add(5, 10);
   ```

3. **Mixing too many files without planning** - start small:
   ```bash
   # Don't convert all files at once
   # Start with new files or critical paths
   ```

## Related Topics

- [[What-is-TypeAnnotation]]
- [[What-is-TsConfig]]
- [[Compile-TypeScript]]
- [[What-is-Interface]]

## Quick Revision

- Use `@ts-check` to enable checking in .js files
- Add JSDoc comments for type information
- Use `allowJs` and `checkJs` in tsconfig
- Install `@types/*` packages for third-party libraries
- Migrate gradually - convert files one by one
- Configure build tools to handle TypeScript
- TypeScript can provide benefits without full migration
