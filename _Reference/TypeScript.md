# TypeScript Overview

## Definition
TypeScript is a statically typed superset of JavaScript developed by Microsoft. It adds optional static typing, interfaces, and other features to JavaScript, compiling to plain JavaScript for runtime execution.

## Key Features

### 1. Static Type Checking
```typescript
// Basic types
let name: string = "John";
let age: number = 25;
let isActive: boolean = true;
let items: string[] = ["a", "b", "c"];

// Type inference
let x = 10;  // TypeScript infers as number

// Function with types
function add(a: number, b: number): number {
  return a + b;
}
```

### 2. Interfaces
```typescript
// Defining interfaces
interface User {
  id: number;
  name: string;
  email: string;
  age?: number;  // Optional property
}

// Using interfaces
const user: User = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

// Extending interfaces
interface Admin extends User {
  role: string;
}
```

### 3. Type Aliases
```typescript
// Type aliases
type ID = string | number;
type Point = { x: number; y: number };
type Status = "active" | "inactive" | "pending";

// Using type aliases
let userId: ID = 123;
let point: Point = { x: 10, y: 20 };
let status: Status = "active";
```

### 4. Generics
```typescript
// Generic function
function identity<T>(arg: T): T {
  return arg;
}

// Generic interface
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

// Using generics
const response: ApiResponse<User> = {
  data: user,
  status: 200,
  message: "Success"
};
```

### 5. Classes
```typescript
class Animal {
  constructor(
    public name: string,
    private age: number
  ) {}

  speak(): string {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  speak(): string {
    return `${this.name} barks`;
  }
}
```

### 6. Enums
```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}

const move: Direction = Direction.Up;
```

## TypeScript vs JavaScript

| Feature | JavaScript | TypeScript |
|---------|------------|------------|
| Typing | Dynamic | Static |
| Compilation | Interpreted | Compiled |
| Error Detection | Runtime | Compile-time |
| IDE Support | Basic | Advanced |
| Learning Curve | Easier | Steeper |

## Installation & Setup

```bash
# Install TypeScript globally
npm install -g typescript

# Initialize TypeScript project
tsc --init

# Compile TypeScript file
tsc filename.ts

# Watch mode
tsc --watch
```

## tsconfig.json Example
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

## Common Use Cases
- Large-scale applications
- Enterprise-level projects
- Angular applications
- Library development
- Any project requiring better code maintainability

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Using `any` type too often | Use specific types or `unknown` |
| Ignoring strict mode | Enable strict in tsconfig |
| Not using interfaces | Define shapes with interfaces |
| Over-complicating types | Keep types simple and readable |

## Quick Revision Summary
- TypeScript adds static typing to JavaScript
- Compiles to JavaScript for runtime
- Provides interfaces, generics, enums, and more
- Catches errors at compile-time
- Improves IDE support and code maintainability
- Ideal for large projects and teams

## Related Topics
- [[JavaScript-Types]]
- [[Interfaces]]
- [[Generics]]
- [[Type-Annotation]]
- [[tsconfig]]
- [[Modules]]
- [[Classes]]
