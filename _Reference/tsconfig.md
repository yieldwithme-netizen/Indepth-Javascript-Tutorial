# tsconfig.json

## Definition

tsconfig.json **configures TypeScript** compiler options.

## Basic Config

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
    "exclude": ["node_modules"]
}
```

## Common Options

| Option | Description |
|--------|-------------|
| target | JS version to compile to |
| module | Module system |
| strict | Enable strict mode |
| outDir | Output directory |
| rootDir | Source directory |

## Quick Revision

- tsconfig.json = TypeScript config
- `compilerOptions`: compiler settings
- `include`/`exclude`: file patterns
- Use `tsc` to compile

---

## Related Topics

- [[What-is-TsConfig]] - [[What-is-TsConfig|tsconfig]]
- [[tsconfig]] - [[tsconfig|tsconfig]]
- [[What-is-TypeScript]] - [[What-is-TypeScript|TypeScript]]
- [[Compile-TypeScript]] - [[Compile-TypeScript|Compiling]]
