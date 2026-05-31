# What Are Generics in TypeScript?

Generics allow you to write flexible, reusable code that works with multiple types while maintaining type safety. They are like type variables that get resolved when the code is used.

## Definition

Generics enable you to create reusable components that work with any data type. Instead of using specific types or `any`, you can define placeholders (type parameters) that get replaced with actual types when the function, class, or interface is used.

## Syntax

```typescript
// Generic function
function identity<T>(value: T): T {
    return value;
}

// Using generic
let result = identity<string>("hello"); // Type: string
let number = identity<number>(42);     // Type: number
```

## Common Use Cases

### 1. Generic Functions
```typescript
// Swap two values
function swap<T, U>(a: T, b: U): [U, T] {
    return [b, a];
}

// Get first element of array
function first<T>(arr: T[]): T | undefined {
    return arr[0];
}

// Merge objects
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}
```

### 2. Generic Interfaces
```typescript
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

// User response
interface User {
    name: string;
    age: number;
}

const userResponse: ApiResponse<User> = {
    data: { name: "John", age: 30 },
    status: 200,
    message: "Success"
};

// Product response
interface Product {
    id: number;
    name: string;
}

const productResponse: ApiResponse<Product[]> = {
    data: [{ id: 1, name: "Laptop" }],
    status: 200,
    message: "Success"
};
```

### 3. Generic Classes
```typescript
class Stack<T> {
    private items: T[] = [];
    
    push(item: T): void {
        this.items.push(item);
    }
    
    pop(): T | undefined {
        return this.items.pop();
    }
    
    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }
    
    isEmpty(): boolean {
        return this.items.length === 0;
    }
}

const numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
const value = numberStack.pop(); // Type: number

const stringStack = new Stack<string>();
stringStack.push("hello");
stringStack.push("world");
```

### 4. Generic Constraints
```typescript
// Constrain to objects with specific property
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(value: T): void {
    console.log(`Length: ${value.length}`);
}

logLength("hello");        // Works
logLength([1, 2, 3]);      // Works
logLength({ length: 10 }); // Works
// logLength(123);         // Error! number doesn't have length

// Constrain to specific keys
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const user = { name: "John", age: 30 };
getProperty(user, "name"); // Works
// getProperty(user, "email"); // Error! "email" not in keyof User
```

### 5. Generic Utility Types
```typescript
// Partial - all properties optional
interface User {
    name: string;
    age: number;
    email: string;
}

function updateUser(id: string, updates: Partial<User>): void {
    // updates can have any subset of User properties
}

updateUser("1", { name: "Jane" }); // Works
updateUser("1", { name: "Jane", age: 25 }); // Works

// Pick - select specific properties
type UserPreview = Pick<User, "name" | "email">;

// Omit - exclude specific properties
type UserWithoutEmail = Omit<User, "email">;
```

## Common Mistakes

1. **Overusing `any` in generics** - Defeats the purpose:
   ```typescript
   // Wrong
   function bad<T>(value: T): any {
       return value.toString(); // Loses type information
   }
   
   // Better
   function better<T extends { toString(): string }>(value: T): string {
       return value.toString();
   }
   ```

2. **Not constraining generic types** - Can lead to runtime errors:
   ```typescript
   // Wrong - could be any type
   function wrong<T>(obj: T): number {
       return obj.length; // Error if T doesn't have length
   }
   
   // Better - constrain to types with length
   function better<T extends { length: number }>(obj: T): number {
       return obj.length;
   }
   ```

3. **Overcomplicating simple cases** - Use simpler types when possible:
   ```typescript
   // Over-engineered
   function identity<T>(value: T): T { return value; }
   
   // Simpler for specific use case
   function getUserName(user: User): string { return user.name; }
   ```

## Related Topics

- [[What-is-TypeAnnotation]]
- [[What-is-Interface]]
- [[Types-vs-Interface]]
- [[What-is-Enum]]
- [[Compile-TypeScript]]

## Quick Revision

- Generics use type parameters (`<T>`) for reusable code
- They maintain type safety while being flexible
- Use constraints (`extends`) to limit generic types
- Common uses: functions, interfaces, classes, utility types
- Built-in utility types: Partial, Pick, Omit, Required
- Type inference can often determine generic types automatically
- Great for creating reusable data structures and APIs
