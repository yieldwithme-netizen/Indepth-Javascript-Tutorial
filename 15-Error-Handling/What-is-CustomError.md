# What is Custom Error

## Definition

Custom errors are user-defined error classes that extend JavaScript's built-in `Error` class. They allow you to create error types specific to your application's domain, providing better context and enabling more precise error handling.

---

## Why Custom Errors?

Built-in errors like `TypeError` and `RangeError` are generic. Custom errors let you:

- Represent domain-specific problems (e.g., `PaymentError`, `ValidationError`)
- Add custom properties for debugging
- Enable targeted catch blocks
- Create meaningful error hierarchies

---

## Basic Custom Error

```javascript
class AppError extends Error {
  constructor(message) {
    super(message);
    this.name = "AppError";
  }
}

throw new AppError("Application failed");
// AppError: Application failed
```

---

## Custom Error with Additional Properties

```javascript
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
    this.timestamp = Date.now();
  }
}

try {
  throw new ValidationError("email", "Invalid email format");
} catch (error) {
  console.log(error.name);      // "ValidationError"
  console.log(error.field);     // "email"
  console.log(error.message);   // "Invalid email format"
  console.log(error.timestamp); // 1699900000000
}
```

---

## Error Hierarchy

You can create a hierarchy of related errors:

```javascript
class DatabaseError extends Error {
  constructor(message) {
    super(message);
    this.name = "DatabaseError";
  }
}

class ConnectionError extends DatabaseError {
  constructor(host, port) {
    super(`Cannot connect to ${host}:${port}`);
    this.name = "ConnectionError";
    this.host = host;
    this.port = port;
  }
}

class QueryError extends DatabaseError {
  constructor(query, reason) {
    super(`Query failed: ${reason}`);
    this.name = "QueryError";
    this.query = query;
    this.reason = reason;
  }
}

// All are instances of DatabaseError
try {
  throw new ConnectionError("localhost", 5432);
} catch (error) {
  if (error instanceof DatabaseError) {
    console.log("Database issue:", error.message);
  }
}
```

---

## Common Use Cases

### API Errors

```javascript
class ApiError extends Error {
  constructor(statusCode, message, details = null) {
    super(message);
    this.name = "ApiError";
    this.statusCode = statusCode;
    this.details = details;
  }

  static notFound(resource) {
    return new ApiError(404, `${resource} not found`);
  }

  static unauthorized(message = "Unauthorized") {
    return new ApiError(401, message);
  }

  static validation(details) {
    return new ApiError(400, "Validation failed", details);
  }
}

// Usage
throw ApiError.notFound("User");
throw ApiError.unauthorized();
throw ApiError.validation({ email: "Invalid format" });
```

### Authentication Errors

```javascript
class AuthError extends Error {
  constructor(message, code) {
    super(message);
    this.name = "AuthError";
    this.code = code;
  }
}

class TokenExpiredError extends AuthError {
  constructor(expiryDate) {
    super("Token has expired");
    this.name = "TokenExpiredError";
    this.expiryDate = expiryDate;
  }
}

class InvalidCredentialsError extends AuthError {
  constructor() {
    super("Invalid username or password");
    this.name = "InvalidCredentialsError";
    this.code = "INVALID_CREDENTIALS";
  }
}
```

### Configuration Errors

```javascript
class ConfigError extends Error {
  constructor(key, message) {
    super(`Config error for "${key}": ${message}`);
    this.name = "ConfigError";
    this.key = key;
  }
}

// Usage
function getConfig(key) {
  const value = process.env[key];
  if (value === undefined) {
    throw new ConfigError(key, "Missing required configuration");
  }
  return value;
}
```

---

## Custom Error with Cause (ES2022)

Modern JavaScript supports error chaining:

```javascript
class DatabaseError extends Error {
  constructor(message, options = {}) {
    super(message, options);
    this.name = "DatabaseError";
  }
}

async function query(sql) {
  try {
    await db.execute(sql);
  } catch (error) {
    throw new DatabaseError("Query failed", { cause: error });
  }
}

try {
  await query("SELECT * FROM users");
} catch (error) {
  console.log(error.message);    // "Query failed"
  console.log(error.cause);      // Original database error
  console.log(error.cause.stack); // Original stack trace
}
```

---

## Common Mistakes

**Mistake 1: Not calling super()**

```javascript
// Bad - missing super call
class MyError extends Error {
  constructor(message) {
    // Missing: super(message)
    this.message = message; // Won't work correctly
  }
}

// Good
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = "MyError";
  }
}
```

**Mistake 2: Not setting the name property**

```javascript
// Bad - name stays as "Error"
class AppError extends Error {
  constructor(message) {
    super(message);
    // Missing: this.name = "AppError"
  }
}

// Good
class AppError extends Error {
  constructor(message) {
    super(message);
    this.name = "AppError";
  }
}
```

**Mistake 3: Overcomplicating simple errors**

```javascript
// Bad - over-engineered
class MissingParameterError extends Error {
  constructor(paramName) {
    super(`Missing parameter: ${paramName}`);
    this.name = "MissingParameterError";
    this.param = paramName;
    this.severity = "high";
    this.category = "validation";
  }
}

// Often sufficient
throw new TypeError(`Missing required parameter: ${paramName}`);
```

---

## Related Topics

- [[Create-CustomError]]
- [[What-is-ErrorTypes]]
- [[Throw-Errors]]
- [[What-is-TryCatch]]
- [[What-is-Propagation]]

---

## Quick Revision

- Custom errors extend the built-in `Error` class
- Always call `super(message)` in constructor
- Set `this.name` to your error class name
- Add custom properties for domain-specific context
- Use static factory methods for common scenarios
- Error hierarchy enables targeted catch blocks
- ES2022 supports error chaining with `{ cause }`
