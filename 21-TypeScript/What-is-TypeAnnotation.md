# What Are Type Annotations in TypeScript?

Type annotations are a way to explicitly declare the type of a variable, function parameter, or return value in TypeScript. They help catch errors at compile time rather than runtime.

## Definition

Type annotations allow you to specify the expected type of a variable, function, or expression. TypeScript uses these annotations to perform static type checking and ensure type safety throughout your code.

## Syntax

```typescript
// Variable annotations
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;

// Function parameter and return type annotations
function add(a: number, b: number): number {
    return a + b;
}

// Arrow function annotations
const multiply = (x: number, y: number): number => x * y;
```

## Common Use Cases

### 1. Basic Types
```typescript
// Primitive types
let username: string = "Alice";
let score: number = 95;
let isLoggedIn: boolean = false;

// Array types
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];

// Tuple
let person: [string, number] = ["Alice", 30];
```

### 2. Object Types
```typescript
// Inline object annotation
let user: { name: string; age: number; email: string } = {
    name: "John",
    age: 30,
    email: "john@example.com"
};

// Optional properties
let config: { host: string; port?: number } = {
    host: "localhost"
};
```

### 3. Function Annotations
```typescript
// Function with explicit types
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Function with optional parameters
function createAdmin(name: string, role: string = "admin"): void {
    console.log(`${name} is now an admin`);
}

// Async function return type
async function fetchData(url: string): Promise<any> {
    const response = await fetch(url);
    return response.json();
}
```

### 4. Union and Intersection Types
```typescript
// Union type
let id: string | number;
id = "abc123";
id = 123;

// Intersection type
type Person = { name: string };
type Employee = { employeeId: number };
type PersonEmployee = Person & Employee;

let emp: PersonEmployee = { name: "John", employeeId: 1001 };
```

## Common Mistakes

1. **Over-annotating** - TypeScript can infer types automatically:
   ```typescript
   // Unnecessary annotation
   let name: string = "Alice";
   
   // Better - let TypeScript infer
   let name = "Alice";
   ```

2. **Using `any` type** - Defeats the purpose of TypeScript:
   ```typescript
   // Avoid
   let data: any = "hello";
   
   // Better
   let data: string = "hello";
   ```

3. **Inconsistent type usage** - Mixing types in collections:
   ```typescript
   // Wrong
   let items: number[] = [1, "two", 3]; // Error!
   
   // Correct
   let items: number[] = [1, 2, 3];
   ```

## Related Topics

- [[What-is-Interface]]
- [[Types-vs-Interface]]
- [[What-is-Generic]]
- [[What-is-TsConfig]]
- [[Compile-TypeScript]]

## Quick Revision

- Type annotations declare expected types for variables, parameters, and return values
- Use `: type` syntax to add annotations
- TypeScript checks types at compile time
- Type annotations help catch errors early
- TypeScript can often infer types automatically
- Use union types (`|`) for multiple possible types
- Use intersection types (`&`) to combine types
