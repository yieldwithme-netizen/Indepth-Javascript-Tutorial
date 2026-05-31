# What Are Error Types

## Definition

Error types in JavaScript are built-in error classes that categorize different kinds of runtime errors. Each error type provides specific information about what went wrong, making debugging easier.

JavaScript has several built-in error types that all inherit from the base `Error` class.

---

## Built-in Error Types

### 1. Error (Base Type)

The generic error type used as a base for all other error types.

```javascript
try {
  throw new Error("Something went wrong");
} catch (e) {
  console.log(e.name);    // "Error"
  console.log(e.message); // "Something went wrong"
  console.log(e.stack);   // Stack trace
}
```

### 2. TypeError

Thrown when an operation is performed on an incorrect data type or when a variable is not of the expected type.

```javascript
// Accessing property of undefined
const obj = undefined;
console.log(obj.name); // TypeError: Cannot read properties of undefined

// Calling non-function
const num = 42;
num(); // TypeError: num is not a function

// Wrong argument type
const result = null.toFixed(2); // TypeError: Cannot read properties of null
```

### 3. ReferenceError

Thrown when trying to use a variable that has not been declared.

```javascript
console.log(x); // ReferenceError: x is not defined

function example() {
  console.log(myVar); // ReferenceError: myVar is not defined
  let myVar = 10;
}
```

### 4. SyntaxError

Thrown when the JavaScript parser encounters invalid syntax during parsing.

```javascript
// Invalid syntax - won't even run
// eval("function(");     // SyntaxError: Unexpected end of input
// eval("if (]");         // SyntaxError: Unexpected token ']'
// const = 5;             // SyntaxError: Missing initializer in const declaration
```

### 5. RangeError

Thrown when a numeric value is outside the allowed range.

```javascript
// Invalid array length
const arr = new Array(-1); // RangeError: Invalid array length

// Number out of bounds
const num = Number.MAX_SAFE_INTEGER + 1;

// Recursion without base case
function infinite() {
  return infinite(); // RangeError: Maximum call stack size exceeded
}
```

### 6. URIError

Thrown when a global URI handling function is used incorrectly.

```javascript
decodeURI("%"); // URIError: URI malformed
decodeURIComponent("%"); // URIError: URI malformed
encodeURI("\n"); // URIError: URI malformed
```

### 7. EvalError

Thrown when the `eval()` function is used incorrectly (rarely used in modern code).

```javascript
new eval(); // EvalError: (not thrown in current engines, but historical)
eval = eval; // EvalError: (historical behavior)
```

### 8. AggregateError (ES2021)

Represents an error when there are multiple errors, such as from `Promise.all()`.

```javascript
try {
  await Promise.all([
    Promise.reject(new Error("First")),
    Promise.reject(new Error("Second")),
    Promise.reject(new Error("Third"))
  ]);
} catch (e) {
  console.log(e instanceof AggregateError); // true
  console.log(e.errors); // [Error: First, Error: Second, Error: Third]
  e.errors.forEach(err => console.log(err.message));
}
```

---

## Error Properties

All error types share these common properties:

```javascript
const error = new TypeError("Type mismatch");

console.log(error.name);    // "TypeError"
console.log(error.message); // "Type mismatch"
console.log(error.stack);   // Full stack trace
console.log(error instanceof Error);    // true
console.log(error instanceof TypeError); // true
```

---

## Common Use Cases

| Error Type | When It Occurs |
|------------|----------------|
| TypeError | Wrong data type, null/undefined access |
| ReferenceError | Using undeclared variables |
| SyntaxError | Invalid code syntax (before runtime) |
| RangeError | Number exceeds allowed range |
| URIError | Malformed URI string |
| AggregateError | Multiple errors from Promise.all |

---

## Common Mistakes

**Mistake 1: Not checking error type**

```javascript
// Bad - catches everything equally
try {
  riskyOperation();
} catch (e) {
  console.log(e.message); // May not handle appropriately
}

// Better - check error type
try {
  riskyOperation();
} catch (e) {
  if (e instanceof TypeError) {
    console.log("Type issue:", e.message);
  } else if (e instanceof ReferenceError) {
    console.log("Reference issue:", e.message);
  } else {
    console.log("Unknown error:", e.message);
  }
}
```

**Mistake 2: Using Error subtypes incorrectly**

```javascript
// Bad - using RangeError for non-range issues
throw new RangeError("Invalid email"); // Wrong type

// Good - use TypeError for type validation
throw new TypeError("Invalid email format");
```

**Mistake 3: Assuming all errors are thrown**

```javascript
// SyntaxError is thrown BEFORE code runs
// eval("function{"); // Parser error - stops execution

// TypeError and ReferenceError are thrown DURING execution
// These CAN be caught with try-catch
```

---

## Related Topics

- [[What-is-TryCatch]]
- [[Use-TryCatch]]
- [[Throw-Errors]]
- [[What-is-CustomError]]
- [[Create-CustomError]]
- [[Handle-AsyncErrors]]

---

## Quick Revision

- **Error**: Base class for all errors
- **TypeError**: Wrong data type or null/undefined access
- **ReferenceError**: Variable not declared
- **SyntaxError**: Invalid syntax (before runtime)
- **RangeError**: Number outside allowed range
- **URIError**: Malformed URI
- **AggregateError**: Multiple errors (Promise.all)
- All error types have `name`, `message`, and `stack` properties
- Use `instanceof` to check specific error types
