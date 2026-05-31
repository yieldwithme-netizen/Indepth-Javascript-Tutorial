# Singleton Pattern in JavaScript

## Definition

The **Singleton Pattern** ensures a class has only **one instance** and provides a global point of access to it. It's useful for managing shared resources like database connections, configuration settings, or logging services where having multiple instances would cause issues.

---

## Basic Implementation

### Class-based Singleton

```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    
    this.data = {};
    Singleton.instance = this;
  }
  
  setData(key, value) {
    this.data[key] = value;
  }
  
  getData(key) {
    return this.data[key];
  }
}

// Usage
const instance1 = new Singleton();
const instance2 = new Singleton();

console.log(instance1 === instance2); // true

instance1.setData("name", "Alice");
console.log(instance2.getData("name")); // "Alice" (same instance)
```

### IIFE Singleton

```javascript
const Singleton = (function() {
  let instance;
  
  function createInstance() {
    return {
      data: {},
      setData(key, value) {
        this.data[key] = value;
      },
      getData(key) {
        return this.data[key];
      }
    };
  }
  
  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

// Usage
const s1 = Singleton.getInstance();
const s2 = Singleton.getInstance();

console.log(s1 === s2); // true
```

---

## Modern JavaScript Singleton

### Module Singleton

```javascript
// config.js - Module is naturally a singleton
const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  retries: 3
};

export default config;

// Any file importing this gets the same instance
import config from "./config.js";
```

### Closure-based Singleton

```javascript
const Logger = (function() {
  let instance;
  
  function createInstance() {
    const logs = [];
    
    return {
      log(message) {
        const timestamp = new Date().toISOString();
        logs.push({ timestamp, message });
        console.log(`[${timestamp}] ${message}`);
      },
      
      getLogs() {
        return [...logs];
      },
      
      clearLogs() {
        logs.length = 0;
      }
    };
  }
  
  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

// Usage
const logger1 = Logger.getInstance();
const logger2 = Logger.getInstance();

logger1.log("Application started");
console.log(logger2.getLogs()); // [{ timestamp: "...", message: "Application started" }]
```

---

## Common Use Cases

### Database Connection

```javascript
class Database {
  constructor() {
    if (Database.instance) {
      return Database.instance;
    }
    
    this.connection = null;
    Database.instance = this;
  }
  
  async connect() {
    if (!this.connection) {
      this.connection = await createConnection({
        host: "localhost",
        port: 5432,
        database: "myapp"
      });
    }
    return this.connection;
  }
  
  async query(sql, params) {
    const conn = await this.connect();
    return conn.query(sql, params);
  }
  
  async disconnect() {
    if (this.connection) {
      await this.connection.close();
      this.connection = null;
    }
  }
}

// Usage
const db1 = new Database();
const db2 = new Database();

console.log(db1 === db2); // true

await db1.query("SELECT * FROM users");
// Both use same connection
```

### Configuration Manager

```javascript
class Config {
  constructor() {
    if (Config.instance) {
      return Config.instance;
    }
    
    this.settings = {};
    Config.instance = this;
  }
  
  set(key, value) {
    this.settings[key] = value;
    return this; // Enable chaining
  }
  
  get(key, defaultValue = null) {
    return this.settings[key] ?? defaultValue;
  }
  
  getAll() {
    return { ...this.settings };
  }
  
  reset() {
    this.settings = {};
    return this;
  }
}

// Usage
const config = new Config();
config.set("apiUrl", "https://api.example.com")
      .set("timeout", 5000)
      .set("debug", true);

console.log(config.get("apiUrl")); // "https://api.example.com"
```

### State Manager

```javascript
class Store {
  constructor() {
    if (Store.instance) {
      return Store.instance;
    }
    
    this.state = {};
    this.listeners = new Map();
    Store.instance = this;
  }
  
  getState() {
    return { ...this.state };
  }
  
  setState(updates) {
    this.state = { ...this.state, ...updates };
    this.notifyListeners();
  }
  
  subscribe(key, callback) {
    if (!this.listeners.has(key)) {
      this.listeners.set(key, new Set());
    }
    this.listeners.get(key).add(callback);
    
    // Return unsubscribe function
    return () => this.listeners.get(key).delete(callback);
  }
  
  notifyListeners() {
    this.listeners.forEach((callbacks, key) => {
      if (key in this.state) {
        callbacks.forEach(cb => cb(this.state[key]));
      }
    });
  }
}

// Usage
const store = new Store();

const unsubscribe = store.subscribe("count", (value) => {
  console.log("Count changed:", value);
});

store.setState({ count: 0 });
store.setState({ count: 1 }); // Logs: "Count changed: 1"

unsubscribe();
```

### Cache Manager

```javascript
class Cache {
  constructor(ttl = 60000) {
    if (Cache.instance) {
      return Cache.instance;
    }
    
    this.store = new Map();
    this.ttl = ttl;
    Cache.instance = this;
  }
  
  set(key, value, ttl = this.ttl) {
    this.store.set(key, {
      value,
      expires: Date.now() + ttl
    });
  }
  
  get(key) {
    const item = this.store.get(key);
    
    if (!item) return null;
    
    if (Date.now() > item.expires) {
      this.store.delete(key);
      return null;
    }
    
    return item.value;
  }
  
  has(key) {
    return this.get(key) !== null;
  }
  
  delete(key) {
    this.store.delete(key);
  }
  
  clear() {
    this.store.clear();
  }
}

// Usage
const cache = new Cache(300000); // 5 minutes TTL

cache.set("user:123", { name: "Alice" });
const user = cache.get("user:123"); // { name: "Alice" }
```

---

## Common Mistakes

### Mistake 1: Not Checking for Existing Instance

```javascript
// Wrong: always creates new instance
class Singleton {
  constructor() {
    this.data = {};
  }
}

// Correct: check and return existing
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    this.data = {};
    Singleton.instance = this;
  }
}
```

### Mistake 2: Forgetting to Store Instance

```javascript
// Wrong: instance not stored
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    // Forgot: Singleton.instance = this;
  }
}

// Correct
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
  }
}
```

### Mistake 3: Using Singleton When Not Needed

```javascript
// Wrong: overusing singleton
class UserService {
  constructor() {
    if (UserService.instance) {
      return UserService.instance;
    }
    UserService.instance = this;
  }
  
  async getUser(id) {
    return fetch(`/api/users/${id}`);
  }
}

// Better: just use a regular class or module
class UserService {
  async getUser(id) {
    return fetch(`/api/users/${id}`);
  }
}

export default new UserService();
```

### Mistake 4: Testing Difficulties

```javascript
// Problem: singleton state persists between tests
class Config {
  constructor() {
    if (Config.instance) {
      return Config.instance;
    }
    this.settings = {};
    Config.instance = this;
  }
}

// Test pollution
test("test 1", () => {
  const config = new Config();
  config.set("key", "value");
});

test("test 2", () => {
  const config = new Config();
  // config.settings still has "key" from test 1!
});

// Solution: provide reset method
class Config {
  // ... existing code ...
  
  static resetInstance() {
    Config.instance = null;
  }
}
```

---

## Singleton vs Module

```javascript
// Singleton (runtime single instance)
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
  }
}

// Module (load-time single instance)
const module = {
  data: {},
  setData(key, value) {
    this.data[key] = value;
  },
  getData(key) {
    return this.data[key];
  }
};

export default module;
```

---

## Quick Revision Summary

| Aspect | Description |
|--------|-------------|
| Purpose | One instance only |
| Access | Global point of access |
| Use cases | Config, DB, Cache, Logger |
| Pros | Controlled access, lazy init |
| Cons | Testing difficulties, tight coupling |
| Alternative | Module pattern |

---

## Related Topics

- [[Design-Patterns]] - Other design patterns
- [[class]] - ES6 classes
- [[Object]] - Object creation
- [[Closures]] - Closure-based singletons
- [[Modules]] - Module pattern
- [[Singleton-Pattern]] - This file
- [[Clean-Code]] - When to use singletons