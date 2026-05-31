# What Are Interfaces in TypeScript?

Interfaces define the structure of objects by specifying what properties and methods they should have. They are a powerful way to enforce type contracts in your code.

## Definition

An interface is a named type that describes the shape of an object. It defines what properties and methods an object should have, without providing implementation details. Interfaces are purely a TypeScript compile-time construct and are erased during compilation.

## Syntax

```typescript
// Basic interface definition
interface User {
    name: string;
    age: number;
    email: string;
}

// Using interface
const user: User = {
    name: "John",
    age: 30,
    email: "john@example.com"
};
```

## Common Use Cases

### 1. Object Shape Definition
```typescript
interface Product {
    id: number;
    name: string;
    price: number;
    description?: string; // Optional property
}

const laptop: Product = {
    id: 1,
    name: "MacBook Pro",
    price: 1999
};
```

### 2. Function Type Signatures
```typescript
interface MathOperation {
    (a: number, b: number): number;
}

const add: MathOperation = (a, b) => a + b;
const subtract: MathOperation = (a, b) => a - b;
```

### 3. Extending Interfaces
```typescript
interface Animal {
    name: string;
    sound(): string;
}

interface Dog extends Animal {
    breed: string;
    fetch(): void;
}

const myDog: Dog = {
    name: "Rex",
    breed: "German Shepherd",
    sound() { return "Woof!"; },
    fetch() { console.log("Fetching ball..."); }
};
```

### 4. Index Signatures
```typescript
interface StringMap {
    [key: string]: string;
}

const colors: StringMap = {
    red: "#FF0000",
    green: "#00FF00",
    blue: "#0000FF"
};
```

### 5. Implementing Interfaces in Classes
```typescript
interface Serializable {
    serialize(): string;
    deserialize(data: string): void;
}

class UserData implements Serializable {
    constructor(public name: string, public age: number) {}
    
    serialize(): string {
        return JSON.stringify({ name: this.name, age: this.age });
    }
    
    deserialize(data: string): void {
        const parsed = JSON.parse(data);
        this.name = parsed.name;
        this.age = parsed.age;
    }
}
```

## Common Mistakes

1. **Confusing interfaces with classes** - Interfaces don't provide implementation:
   ```typescript
   // Wrong - trying to add implementation
   interface Bad {
       greet() { // Error! Interfaces can't have implementations
           return "Hello";
       }
   }
   
   // Correct
   interface Good {
       greet(): string;
   }
   ```

2. **Optional properties in initialization** - Required properties must be provided:
   ```typescript
   interface Config {
       host: string;
       port?: number;
   }
   
   // Wrong - missing required property
   const bad: Config = {}; // Error!
   
   // Correct
   const good: Config = { host: "localhost" };
   ```

3. **Using interfaces for primitive types** - Interfaces are for objects:
   ```typescript
   // Wrong
   interface Number {
       value: number;
   }
   
   // Correct - use type aliases for primitives
   type NumberAlias = number;
   ```

## Related Topics

- [[What-is-TypeAnnotation]]
- [[Types-vs-Interface]]
- [[What-is-Enum]]
- [[What-is-Generic]]
- [[Compile-TypeScript]]

## Quick Revision

- Interfaces define object shapes and contracts
- Use `interface` keyword to declare them
- Properties can be optional using `?`
- Interfaces can extend other interfaces using `extends`
- Classes implement interfaces using `implements`
- Interfaces are erased at compile time
- Great for defining APIs and object structures
