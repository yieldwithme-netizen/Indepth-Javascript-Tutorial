# What Are Enums in TypeScript?

Enums (Enumerations) are a way to define a set of named constants. They help make code more readable and maintainable by giving meaningful names to a collection of related values.

## Definition

An enum is a special data type that allows you to define a set of named constants. Enums make it easier to document intent and create a set of distinct cases. TypeScript provides both numeric and string-based enums.

## Syntax

```typescript
// Numeric enum
enum Direction {
    Up,        // 0
    Down,      // 1
    Left,      // 2
    Right      // 3
}

// String enum
enum Color {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

// Using enums
let dir: Direction = Direction.Up;
let color: Color = Color.Red;
```

## Common Use Cases

### 1. Numeric Enums
```typescript
// Default numeric values (0, 1, 2, ...)
enum Status {
    Active,    // 0
    Inactive,  // 1
    Pending    // 2
}

// Custom numeric values
enum HttpStatus {
    OK = 200,
    NotFound = 404,
    ServerError = 500
}

// Using
let userStatus = Status.Active;
let statusCode = HttpStatus.NotFound;
console.log(userStatus);  // 0
console.log(statusCode);  // 404
```

### 2. String Enums
```typescript
enum HttpMethod {
    Get = "GET",
    Post = "POST",
    Put = "PUT",
    Delete = "DELETE"
}

enum Environment {
    Development = "development",
    Production = "production",
    Test = "test"
}

// Using - more readable output
let method: HttpMethod = HttpMethod.Post;
console.log(method);  // "POST" instead of 1
```

### 3. Const Enums
```typescript
// Compile-time constant enum - no runtime object
const enum Planet {
    Mercury,
    Venus,
    Earth,
    Mars,
    Jupiter,
    Saturn,
    Uranus,
    Neptune
}

// Values are inlined during compilation
let planet: Planet = Planet.Earth;
// Compiles to: let planet = 2;
```

### 4. Enums with Computed Values
```typescript
enum FileAccess {
    None = 0,
    Read = 1 << 0,    // 1
    Write = 1 << 1,   // 2
    ReadWrite = Read | Write  // 3
}

console.log(FileAccess.ReadWrite);  // 3
```

### 5. Reverse Mapping
```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}

// Forward mapping
let dirName = Direction.Up;  // 0

// Reverse mapping (numeric enums only)
let dirValue = Direction[0];  // "Up"
console.log(Direction);  // { 0: "Up", 1: "Down", 2: "Left", 3: "Right", Up: 0, ... }
```

### 6. Enums in Practice
```typescript
// API response types
enum ApiStatus {
    Loading = "LOADING",
    Success = "SUCCESS",
    Error = "ERROR"
}

interface ApiResponse<T> {
    status: ApiStatus;
    data?: T;
    error?: string;
}

// Using in code
function handleResponse(response: ApiResponse<User>) {
    switch (response.status) {
        case ApiStatus.Loading:
            showLoader();
            break;
        case ApiStatus.Success:
            displayUser(response.data);
            break;
        case ApiStatus.Error:
            showError(response.error);
            break;
    }
}
```

## Common Mistakes

1. **Using numeric enums when string would be better** - Less readable:
   ```typescript
   // Hard to read in logs
   enum Status {
       Active,   // 0
       Inactive  // 1
   }
   console.log(Status.Active);  // 0
   
   // Better - readable output
   enum Status {
       Active = "ACTIVE",
       Inactive = "INACTIVE"
   }
   console.log(Status.Active);  // "ACTIVE"
   ```

2. **Not using const enums when performance matters** - Larger bundle:
   ```typescript
   // Creates runtime object
   enum Direction {
       Up,
       Down,
       Left,
       Right
   }
   
   // Better - inlined, no runtime object
   const enum Direction {
       Up,
       Down,
       Left,
       Right
   }
   ```

3. **Confusing enum values with types** - Enums are values:
   ```typescript
   enum Color {
       Red,
       Green,
       Blue
   }
   
   // Wrong - trying to use enum as type
   function getColor(color: Color) {}  // This works!
   
   // But remember, enum is both a type and a value
   let myColor: Color = Color.Red;  // Type annotation + value
   ```

## Related Topics

- [[What-is-TypeAnnotation]]
- [[What-is-Interface]]
- [[Types-vs-Interface]]
- [[What-is-Generic]]
- [[Compile-TypeScript]]

## Quick Revision

- Enums define sets of named constants
- Numeric enums have auto-incrementing values (0, 1, 2, ...)
- String enums require explicit values for each member
- Const enums are inlined at compile time (better performance)
- Numeric enums support reverse mapping
- Use string enums for better readability in logs/debugging
- Enums are both types and values in TypeScript
- Great for representing fixed sets of options (status codes, directions, etc.)
