# Types vs Interfaces in TypeScript

Both `type` and `interface` can be used to define object shapes in TypeScript. While they're similar in many ways, there are important differences to understand.

## Definition

- **Type aliases (`type`)**: Create a new name for any type (primitives, unions, intersections, tuples, etc.)
- **Interfaces (`interface`)**: Define the shape of objects and can be extended and implemented by classes

## Syntax Comparison

```typescript
// Type alias
type User = {
    name: string;
    age: number;
};

// Interface
interface UserInterface {
    name: string;
    age: number;
}
```

## Key Differences

### 1. Declaration and Extension

```typescript
// Type - uses intersection (&)
type Animal = {
    name: string;
};

type Dog = Animal & {
    breed: string;
};

// Interface - uses extends
interface AnimalInterface {
    name: string;
}

interface DogInterface extends AnimalInterface {
    breed: string;
}
```

### 2. Declaration Merging

```typescript
// Interface - supports declaration merging
interface User {
    name: string;
}

interface User {
    age: number;
}

// Result: User has both name and age
const user: User = { name: "John", age: 30 }; // Works!

// Type - does NOT support declaration merging
type UserType = {
    name: string;
};

type UserType = {  // Error! Duplicate identifier
    age: number;
};
```

### 3. Implementing in Classes

```typescript
// Interface - can be implemented
interface Shape {
    area(): number;
}

class Circle implements Shape {
    constructor(private radius: number) {}
    
    area(): number {
        return Math.PI * this.radius ** 2;
    }
}

// Type - can be implemented (with some limitations)
type ShapeType = {
    area(): number;
};

class Square implements ShapeType {
    constructor(private side: number) {}
    
    area(): number {
        return this.side ** 2;
    }
}
```

### 4. Type Flexibility

```typescript
// Type - more flexible, can define any type
type StringOrNumber = string | number;
type Tuple = [string, number];
type Callback = (data: string) => void;
type Primitive = string;
type Union = { a: string } | { b: number };

// Interface - limited to object shapes
// interface StringOrNumber = string | number; // Error!
```

### 5. Computed Properties

```typescript
// Type - supports computed properties
type Keys = "name" | "age" | "email";
type User = {
    [K in Keys]: string;
};

// Interface - doesn't support computed properties
// interface User {
//     [K in Keys]: string; // Error!
// }
```

### 6. Generic Constraints

```typescript
// Both work similarly with generics
type Container<T> = {
    value: T;
};

interface ContainerInterface<T> {
    value: T;
}
```

## When to Use Each

### Use `interface` when:
```typescript
// 1. Defining object shapes for classes
interface Repository<T> {
    findById(id: string): Promise<T>;
    findAll(): Promise<T[]>;
    create(item: T): Promise<T>;
}

// 2. Need declaration merging (e.g., extending third-party types)
interface Window {
    myCustomProperty: string;
}

// 3. Creating public APIs
interface UserService {
    getUser(id: string): User;
    updateUser(id: string, data: Partial<User>): User;
}
```

### Use `type` when:
```typescript
// 1. Union types
type Status = "active" | "inactive" | "pending";

// 2. Tuple types
type Point = [number, number];

// 3. Function types
type EventHandler = (event: Event) => void;

// 4. Complex utility types
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

// 5. Mapped types
type Optional<T> = {
    [P in keyof T]?: T[P];
};
```

## Common Mistakes

1. **Using type for everything** - Missing interface benefits:
   ```typescript
   // Wrong - using type for class implementation
   type Shape = {
       area(): number;
   };
   
   // Better - use interface for class contracts
   interface Shape {
       area(): number;
   }
   ```

2. **Using interface for unions** - Not possible:
   ```typescript
   // Wrong
   interface Status = "active" | "inactive"; // Error!
   
   // Correct
   type Status = "active" | "inactive";
   ```

3. **Mixing styles inconsistently** - Pick one for similar cases:
   ```typescript
   // Inconsistent
   type User = { name: string };
   interface Product { name: string }
   
   // Better - consistent
   type User = { name: string };
   type Product = { name: string };
   ```

## Related Topics

- [[What-is-TypeAnnotation]]
- [[What-is-Interface]]
- [[What-is-Generic]]
- [[What-is-Enum]]
- [[Compile-TypeScript]]

## Quick Revision

- **Interfaces**: Best for object shapes, class contracts, declaration merging
- **Types**: Best for unions, tuples, function types, complex type manipulation
- Both can define object shapes similarly
- Interfaces support declaration merging; types don't
- Both work with generics
- Use interfaces for public APIs and class implementations
- Use types for complex type transformations
- Choose one style consistently for similar cases
