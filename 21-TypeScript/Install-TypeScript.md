# How to Install TypeScript

## Definition

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Installing TypeScript sets up the TypeScript compiler (tsc) which converts TypeScript code into JavaScript.

## Installation Methods

### Global Installation

```bash
# Install globally with npm
npm install -g typescript

# Or with yarn
yarn global add typescript

# Verify installation
tsc --version
```

### Local Project Installation

```bash
# Install as dev dependency
npm install --save-dev typescript

# Or with yarn
yarn add -D typescript

# Run with npx
npx tsc --version
```

## Project Setup

### Initialize TypeScript Project

```bash
# Create tsconfig.json
npx tsc --init
```

### Basic tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## Project Structure

```
project/
├── src/
│   ├── index.ts
│   └── utils/
│       └── helpers.ts
├── dist/
├── tsconfig.json
├── package.json
└── node_modules/
```

## Basic TypeScript File

```typescript
// src/index.ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}

const message: string = greet('World');
console.log(message);
```

## npm Scripts Setup

```json
{
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "start": "node dist/index.js",
    "typecheck": "tsc --noEmit"
  }
}
```

## Using with Popular Tools

### With React

```bash
npm install --save-dev typescript @types/react @types/react-dom
```

### With Node.js

```bash
npm install --save-dev typescript @types/node
```

### With Express

```bash
npm install express
npm install --save-dev typescript @types/express
```

## Common Compiler Options

```json
{
  "compilerOptions": {
    "target": "ES2020",      // JavaScript version
    "module": "commonjs",     // Module system
    "strict": true,          // Enable all strict checks
    "noImplicitAny": true,   // Error on implicit any
    "noUnusedLocals": true,  // Error on unused variables
    "noUnusedParameters": true,
    "outDir": "./dist",      // Output directory
    "rootDir": "./src",      // Source directory
    "declaration": true,     // Generate .d.ts files
    "sourceMap": true        // Generate source maps
  }
}
```

## Compiling TypeScript

```bash
# Compile all files
npx tsc

# Watch mode
npx tsc --watch

# Check without emitting
npx tsc --noEmit

# Compile specific file
npx tsc src/index.ts
```

## Common Use Cases

- Adding type safety to JavaScript projects
- Large-scale application development
- Team collaboration on codebases
- IDE autocompletion and refactoring
- Documentation through types

## Common Mistakes

- Not using --save-dev for typescript
- Forgetting to initialize tsconfig.json
- Not installing type definitions
- Using wrong target version
- Not setting up proper build scripts

## Related Topics

- [[What-is-Jest]]
- [[Write-UnitTests]]

## Quick Revision

| Command | Purpose |
|---------|---------|
| npm i -D typescript | Install TypeScript |
| tsc --init | Create tsconfig.json |
| tsc | Compile TypeScript |
| tsc --watch | Watch mode |
| @types/* | Type definitions |
