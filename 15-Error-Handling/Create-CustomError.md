# How to Create Custom Errors

## Definition

Creating custom errors involves defining your own error classes that extend JavaScript's built-in `Error` class. This guide covers practical patterns for creating, configuring, and using custom errors in real applications.

---

## Step-by-Step Creation

### Step 1: Create the Class

```javascript
class AppError extends Error {
  constructor(message) {
    super(message);           // Call parent constructor
    this.name = "AppError";   // Set error name
  }
}
```

### Step 2: Add Custom Properties

```javascript
class ValidationError extends Error {
  constructor(field, message, value) {
    super(message);
    this.name = "ValidationError";
    this.field = field;       // Which field failed
    this.value = value;       // What value was invalid
    this.timestamp = Date.now();
  }
}
```

### Step 3: Add Methods (Optional)

```javascript
class ApiError extends Error {
  constructor(statusCode, message, details) {
    super(message);
    this.name = "ApiError";
    this.statusCode = statusCode;
    this.details = details;
  }

  toJSON() {
    return {
      name: this.name,
      message: this.message,
      statusCode: this.statusCode,
      details: this.details,
    };
  }

  isClientError() {
    return this.statusCode >= 400 && this.statusCode < 500;
  }

  isServerError() {
    return this.statusCode >= 500;
  }
}
```

---

## Complete Examples

### Form Validation Errors

```javascript
class FormValidationError extends Error {
  constructor(errors) {
    super("Form validation failed");
    this.name = "FormValidationError";
    this.errors = errors; // Array of { field, message }
  }

  getFieldError(field) {
    const error = this.errors.find(e => e.field === field);
    return error ? error.message : null;
  }

  hasFieldError(field) {
    return this.errors.some(e => e.field === field);
  }

  static fromZodError(zodError) {
    const errors = zodError.errors.map(e => ({
      field: e.path.join("."),
      message: e.message,
    }));
    return new FormValidationError(errors);
  }
}

// Usage
const errors = [
  { field: "email", message: "Invalid email" },
  { field: "password", message: "Too short" },
];

try {
  throw new FormValidationError(errors);
} catch (error) {
  console.log(error.getFieldError("email")); // "Invalid email"
  console.log(error.hasFieldError("name"));  // false
}
```

### Network/HTTP Errors

```javascript
class NetworkError extends Error {
  constructor(url, options = {}) {
    super(`Network request failed: ${url}`);
    this.name = "NetworkError";
    this.url = url;
    this.status = options.status;
    this.statusText = options.statusText;
    this.retryable = options.retryable ?? true;
  }

  static fromResponse(response) {
    return new NetworkError(response.url, {
      status: response.status,
      statusText: response.statusText,
      retryable: response.status >= 500 || response.status === 429,
    });
  }

  static fromFetchError(error, url) {
    return new NetworkError(url, {
      status: 0,
      statusText: error.message,
      retryable: true,
    });
  }
}
```

### Database Errors

```javascript
class DatabaseError extends Error {
  constructor(message, options = {}) {
    super(message);
    this.name = "DatabaseError";
    this.query = options.query;
    this.params = options.params;
    this.cause = options.cause;
  }

  static connectionFailed(host, port, cause) {
    return new DatabaseError(
      `Cannot connect to database at ${host}:${port}`,
      { cause }
    );
  }

  static queryFailed(query, cause) {
    return new DatabaseError("Query execution failed", {
      query,
      cause,
    });
  }

  static constraintViolation(table, constraint, cause) {
    return new DatabaseError(
      `Constraint violation on ${table}: ${constraint}`,
      { cause }
    );
  }
}
```

---

## Error Factory Pattern

Create errors through a factory for consistency:

```javascript
const ErrorFactory = {
  validation(field, message) {
    const error = new TypeError(message);
    error.name = "ValidationError";
    error.field = field;
    return error;
  },

  notFound(resource, id) {
    const error = new Error(`${resource} with id ${id} not found`);
    error.name = "NotFoundError";
    error.resource = resource;
    error.resourceId = id;
    return error;
  },

  forbidden(message = "Access denied") {
    const error = new Error(message);
    error.name = "ForbiddenError";
    return error;
  },
};

// Usage
throw ErrorFactory.validation("email", "Invalid format");
throw ErrorFactory.notFound("User", 123);
throw ErrorFactory.forbidden("Admin only");
```

---

## Common Mistakes

**Mistake 1: Forgetting to set prototype chain**

```javascript
// Bad - instanceof may not work correctly
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = "MyError";
  }
}

// Good - ensure proper prototype chain
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = "MyError";
    // Fix prototype chain for instanceof
    Object.setPrototypeOf(this, MyError.prototype);
  }
}
```

**Mistake 2: Not providing useful properties**

```javascript
// Bad - no useful context
class AppError extends Error {
  constructor(message) {
    super(message);
    this.name = "AppError";
    // No additional info
  }
}

// Good - include debugging context
class AppError extends Error {
  constructor(message, context = {}) {
    super(message);
    this.name = "AppError";
    this.context = context;
    this.timestamp = Date.now();
    this.id = generateErrorId();
  }
}
```

**Mistake 3: Too many error classes**

```javascript
// Bad - excessive granularity
class InvalidEmailError extends Error {}
class InvalidNameError extends Error {}
class InvalidAgeError extends Error {}
class InvalidPhoneError extends Error {}

// Good - one class with field info
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.field = field;
  }
}
```

---

## Related Topics

- [[What-is-CustomError]]
- [[What-is-ErrorTypes]]
- [[Throw-Errors]]
- [[Use-TryCatch]]
- [[What-is-Propagation]]

---

## Quick Revision

- Extend `Error` class and call `super(message)`
- Always set `this.name` to class name
- Add custom properties for context
- Use static factory methods for common scenarios
- Include `Object.setPrototypeOf()` for reliable instanceof
- Don't over-engineer - use generic errors when appropriate
- Consider error serialization for logging/APIs
