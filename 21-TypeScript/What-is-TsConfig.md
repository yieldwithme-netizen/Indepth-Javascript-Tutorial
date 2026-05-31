# What is tsconfig.json?

The `tsconfig.json` file is the configuration file for TypeScript. It tells the TypeScript compiler how to compile your code and which files to include.

## Definition

`tsconfig.json` is a JSON file in the root of your project that specifies compiler options, file patterns, and project-level settings for TypeScript. It defines how TypeScript should treat your codebase.

## Basic Structure

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "commonjs",
        "strict": true,
        "esModuleInterop": true,
        "outDir": "./dist",
        "rootDir": "./src"
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist"]
}
```

## Common Options

### 1. Compiler Options
```json
{
    "compilerOptions": {
        // Language and Environment
        "target": "ES2020",        // JavaScript output version
        "lib": ["ES2020", "DOM"],  // Type definition files
        
        // Modules
        "module": "commonjs",      // Module system
        "moduleResolution": "node",
        "baseUrl": "./",           // Base directory for module resolution
        "paths": {                 // Path aliases
            "@/*": ["src/*"]
        },
        
        // Strict Type-Checking
        "strict": true,            // Enable all strict type checks
        "noImplicitAny": true,     // Error on implicit any
        "strictNullChecks": true,  // Enable strict null checks
        
        // Output
        "outDir": "./dist",        // Output directory
        "rootDir": "./src",        // Root directory of source
        "declaration": true,       // Generate .d.ts files
        "sourceMap": true,         // Generate source maps
        
        // Interop
        "esModuleInterop": true,   // CommonJS/ES module interop
        "allowSyntheticDefaultImports": true
    }
}
```

### 2. Include/Exclude Patterns
```json
{
    "include": [
        "src/**/*",              // All files in src
        "types/**/*",            // All files in types
        "*.config.ts"           // Config files in root
    ],
    "exclude": [
        "node_modules",
        "dist",
        "**/*.test.ts",         // Exclude test files
        "**/*.spec.ts"
    ]
}
```

### 3. Project References
```json
{
    "references": [
        { "path": "./packages/core" },
        { "path": "./packages/utils" }
    ]
}
```

## Common Use Cases

### 1. React Project Configuration
```json
{
    "compilerOptions": {
        "target": "ES2020",
        "lib": ["DOM", "DOM.Iterable", "ES2020"],
        "module": "ESNext",
        "moduleResolution": "node",
        "jsx": "react-jsx",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true,
        "resolveJsonModule": true,
        "isolatedModules": true,
        "noEmit": true
    },
    "include": ["src"]
}
```

### 2. Node.js Project Configuration
```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "commonjs",
        "lib": ["ES2020"],
        "outDir": "./dist",
        "rootDir": "./src",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true,
        "resolveJsonModule": true,
        "declaration": true,
        "declarationMap": true,
        "sourceMap": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

### 3. Library Development
```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "ESNext",
        "lib": ["ES2020"],
        "declaration": true,
        "declarationMap": true,
        "sourceMap": true,
        "outDir": "./dist",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true
    },
    "include": ["src"],
    "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

## Common Mistakes

1. **Missing `strict` mode** - Less type safety:
   ```json
   // Wrong - missing strict
   {
       "compilerOptions": {
           "target": "ES2020"
       }
   }
   
   // Better
   {
       "compilerOptions": {
           "target": "ES2020",
           "strict": true
       }
   }
   ```

2. **Wrong `module` setting** - Module compatibility issues:
   ```json
   // Wrong for Node.js
   {
       "compilerOptions": {
           "module": "ESNext"  // Wrong for CommonJS
       }
   }
   
   // Correct for Node.js
   {
       "compilerOptions": {
           "module": "commonjs"
       }
   }
   ```

3. **Not setting `outDir`** - Compiles to source directory:
   ```json
   // Wrong - no output directory
   {
       "compilerOptions": {
           "rootDir": "./src"
       }
   }
   
   // Better
   {
       "compilerOptions": {
           "rootDir": "./src",
           "outDir": "./dist"
       }
   }
   ```

## Related Topics

- [[Compile-TypeScript]]
- [[Use-With-JS]]
- [[What-is-TypeAnnotation]]
- [[What-is-Interface]]

## Quick Revision

- `tsconfig.json` configures TypeScript compiler options
- `compilerOptions` defines how TypeScript processes code
- `include` and `exclude` control which files are processed
- `strict` mode enables all type-checking options
- `target` specifies the JavaScript output version
- `module` defines the module system (commonjs, ESNext, etc.)
- `outDir` and `rootDir` control source and output directories
- Use `--init` flag to generate a default tsconfig.json
