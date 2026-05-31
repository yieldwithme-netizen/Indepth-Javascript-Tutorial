# What is Error Logging

## Definition

Error logging is the practice of recording error information for debugging, monitoring, and analysis. Proper error logging helps developers understand what went wrong, when, and where, enabling faster troubleshooting and improving application reliability.

---

## Console Logging Methods

### Basic Console Methods

```javascript
// Different severity levels
console.error("Error:", error.message);   // Red text, stack trace
console.warn("Warning:", warning);        // Yellow text
console.info("Info:", info);              // Blue text
console.log("Debug:", debug);             // Standard output
console.debug("Verbose:", details);       // Gray text (may be hidden)
```

### Logging Error Objects

```javascript
try {
  riskyOperation();
} catch (error) {
  // Bad - loses stack trace
  console.error(error.message);

  // Good - logs full error with stack
  console.error(error);

  // Better - structured logging
  console.error({
    message: error.message,
    stack: error.stack,
    timestamp: new Date().toISOString(),
  });
}
```

### Console.table for Structured Data

```javascript
const errors = [
  { type: "TypeError", count: 15, lastSeen: "2024-01-15" },
  { type: "RangeError", count: 3, lastSeen: "2024-01-14" },
  { type: "ReferenceError", count: 8, lastSeen: "2024-01-15" },
];

console.table(errors);
```

---

## Structured Error Logging

### Logger Class

```javascript
class Logger {
  constructor(context = {}) {
    this.context = context;
  }

  error(message, error, extra = {}) {
    const logEntry = {
      level: "ERROR",
      message,
      error: {
        name: error.name,
        message: error.message,
        stack: error.stack,
      },
      context: this.context,
      extra,
      timestamp: new Date().toISOString(),
    };

    console.error(JSON.stringify(logEntry, null, 2));
    return logEntry;
  }

  warn(message, extra = {}) {
    console.warn(JSON.stringify({
      level: "WARN",
      message,
      context: this.context,
      extra,
      timestamp: new Date().toISOString(),
    }));
  }

  info(message, extra = {}) {
    console.info(JSON.stringify({
      level: "INFO",
      message,
      context: this.context,
      extra,
      timestamp: new Date().toISOString(),
    }));
  }
}

// Usage
const logger = new Logger({ service: "auth", version: "1.0" });

try {
  await authenticateUser();
} catch (error) {
  logger.error("Authentication failed", error, { userId: 123 });
}
```

### Request Context Logger

```javascript
function createRequestLogger(req) {
  const requestId = req.headers["x-request-id"] || generateId();

  return {
    error(message, error) {
      console.error(JSON.stringify({
        level: "ERROR",
        message,
        requestId,
        method: req.method,
        path: req.path,
        error: {
          name: error.name,
          message: error.message,
          stack: error.stack,
        },
        timestamp: new Date().toISOString(),
      }));
    },

    info(message) {
      console.info(JSON.stringify({
        level: "INFO",
        message,
        requestId,
        timestamp: new Date().toISOString(),
      }));
    },
  };
}

// Usage in Express middleware
app.use((req, res, next) => {
  req.log = createRequestLogger(req);
  next();
});

app.get("/api/users", async (req, res) => {
  try {
    const users = await getUsers();
    res.json(users);
  } catch (error) {
    req.log.error("Failed to fetch users", error);
    res.status(500).json({ error: "Internal server error" });
  }
});
```

---

## Common Use Cases

### API Error Logging

```javascript
async function apiRequest(url, options = {}) {
  const startTime = Date.now();

  try {
    const response = await fetch(url, options);
    const duration = Date.now() - startTime;

    if (!response.ok) {
      const error = new Error(`HTTP ${response.status}`);
      logError(error, { url, status: response.status, duration });
      throw error;
    }

    logInfo("API request success", { url, duration });
    return await response.json();
  } catch (error) {
    const duration = Date.now() - startTime;
    logError(error, { url, duration });
    throw error;
  }
}
```

### Validation Error Logging

```javascript
function validateAndLog(data, schema) {
  const errors = schema.validate(data);

  if (errors.length > 0) {
    console.error("Validation failed:", {
      errors,
      data: sanitize敏感数据(data),
      timestamp: new Date().toISOString(),
    });

    throw new ValidationError(errors);
  }
}
```

### Database Error Logging

```javascript
async function queryWithLogging(sql, params) {
  const startTime = Date.now();

  try {
    const result = await db.query(sql, params);
    const duration = Date.now() - startTime;

    if (duration > 1000) {
      console.warn("Slow query detected", {
        sql,
        duration,
        rowCount: result.rowCount,
      });
    }

    return result;
  } catch (error) {
    const duration = Date.now() - startTime;
    console.error("Query failed", {
      sql,
      params,
      duration,
      error: error.message,
    });
    throw error;
  }
}
```

---

## Sanitizing Sensitive Data

```javascript
function sanitize(data) {
  const sensitive = ["password", "token", "secret", "creditCard"];

  return Object.keys(data).reduce((clean, key) => {
    if (sensitive.some(s => key.toLowerCase().includes(s))) {
      clean[key] = "[REDACTED]";
    } else {
      clean[key] = data[key];
    }
    return clean;
  }, {});
}

// Usage
const userData = {
  name: "Alice",
  password: "secret123",
  email: "alice@example.com",
};

console.log(sanitize(userData));
// { name: "Alice", password: "[REDACTED]", email: "alice@example.com" }
```

---

## Log Levels

```javascript
const LOG_LEVELS = {
  DEBUG: 0,
  INFO: 1,
  WARN: 2,
  ERROR: 3,
};

class LeveledLogger {
  constructor(minLevel = LOG_LEVELS.INFO) {
    this.minLevel = minLevel;
  }

  log(level, message, data = {}) {
    if (level < this.minLevel) return;

    const entry = {
      level: Object.keys(LOG_LEVELS).find(k => LOG_LEVELS[k] === level),
      message,
      data,
      timestamp: new Date().toISOString(),
    };

    if (level >= LOG_LEVELS.ERROR) {
      console.error(JSON.stringify(entry));
    } else if (level >= LOG_LEVELS.WARN) {
      console.warn(JSON.stringify(entry));
    } else {
      console.log(JSON.stringify(entry));
    }
  }

  debug(msg, data) { this.log(LOG_LEVELS.DEBUG, msg, data); }
  info(msg, data) { this.log(LOG_LEVELS.INFO, msg, data); }
  warn(msg, data) { this.log(LOG_LEVELS.WARN, msg, data); }
  error(msg, data) { this.log(LOG_LEVELS.ERROR, msg, data); }
}
```

---

## Common Mistakes

**Mistake 1: Logging only error message**

```javascript
// Bad - loses stack trace
catch (error) {
  console.error(error.message);
}

// Good - logs full error
catch (error) {
  console.error(error);
}
```

**Mistake 2: Logging sensitive data**

```javascript
// Bad - exposes secrets
console.error("Auth failed:", { password, token });

// Good - sanitize first
console.error("Auth failed:", { userId, error: error.message });
```

**Mistake 3: Inconsistent log format**

```javascript
// Bad - different formats make searching hard
console.error("Error: " + error.message);
console.error({ error: error.message });
console.error(error.message, error.stack);

// Good - consistent structure
console.error(JSON.stringify({
  level: "ERROR",
  message: error.message,
  stack: error.stack,
}));
```

---

## Related Topics

- [[What-is-ErrorTypes]]
- [[What-is-TryCatch]]
- [[Use-TryCatch]]
- [[Handle-AsyncErrors]]
- [[What-is-CustomError]]

---

## Quick Revision

- Use console.error for errors, console.warn for warnings
- Always log full error objects, not just messages
- Create structured log entries with timestamp and context
- Sanitize sensitive data before logging
- Use appropriate log levels (DEBUG, INFO, WARN, ERROR)
- Include request context (requestId, userId) in logs
- Consider external logging services for production
