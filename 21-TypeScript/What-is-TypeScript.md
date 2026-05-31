# What is TypeScript?

## Definition

TypeScript is a **superset of JavaScript** that adds static type checking.

## Basic Syntax

```typescript
// Type annotations
let name: string = "John";
let age: number = 30;
let active: boolean = true;

// Function types
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Interface
interface User {
    name: string;
    age: number;
    email?: string; // optional
}

// Type
type ID = string | number;
```

## TypeScript vs JavaScript

| Feature | TypeScript | JavaScript |
|---------|------------|------------|
| Types | Static | Dynamic |
| Compilation | Required | Not required |
| IDE Support | Excellent | Good |
| Learning Curve | Steeper | Easier |
| Error Detection | Compile-time | Runtime |

## Benefits

```typescript
// Catch errors early
function add(a: number, b: number): number {
    return a + b;
}

add("1", 2); // Error: string not assignable to number

// Better IDE support
// Autocomplete, refactoring, documentation
```

## Quick Revision

- TypeScript = JavaScript + types
- Adds static type checking
- Catches errors at compile time
- Better IDE support
- Compiles to JavaScript

---

## Related Topics

- [[What-is-TypeScript]] - TypeScript overview
- [[Install-TypeScript]] - Installation
- [[What-is-TypeAnnotation]] - Type annotations
- [[What-is-Interface]] - Interfaces
- [[What-is-Generic]] - Generics
