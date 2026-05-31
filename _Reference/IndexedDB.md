# IndexedDB - Client-Side Database

## Definition

IndexedDB is a low-level browser API for storing large amounts of structured data client-side. It provides a full database with indexes, transactions, and querying capabilities.

```javascript
const request = indexedDB.open('MyDatabase', 1);
```

## Basic Structure

```
Database
  └── Object Store (like a table)
        └── Records (key-value pairs)
              └── Indexes (for fast queries)
```

## Common Use Cases

### 1. Basic CRUD Operations

```javascript
class TodoDB {
  constructor(dbName = 'TodoDB', version = 1) {
    this.dbName = dbName;
    this.version = version;
    this.db = null;
  }
  
  async open() {
    return new Promise((resolve, reject) => {
      const request = indexedDB.open(this.dbName, this.version);
      
      request.onupgradeneeded = (e) => {
        const db = e.target.result;
        if (!db.objectStoreNames.contains('todos')) {
          const store = db.createObjectStore('todos', { 
            keyPath: 'id',
            autoIncrement: true 
          });
          store.createIndex('title', 'title', { unique: false });
          store.createIndex('completed', 'completed', { unique: false });
        }
      };
      
      request.onsuccess = (e) => {
        this.db = e.target.result;
        resolve(this.db);
      };
      
      request.onerror = (e) => reject(e.target.error);
    });
  }
  
  async add(todo) {
    return new Promise((resolve, reject) => {
      const tx = this.db.transaction('todos', 'readwrite');
      const store = tx.objectStore('todos');
      const request = store.add(todo);
      
      request.onsuccess = () => resolve(request.result);
      request.onerror = () => reject(request.error);
    });
  }
  
  async get(id) {
    return new Promise((resolve, reject) => {
      const tx = this.db.transaction('todos', 'readonly');
      const store = tx.objectStore('todos');
      const request = store.get(id);
      
      request.onsuccess = () => resolve(request.result);
      request.onerror = () => reject(request.error);
    });
  }
  
  async update(todo) {
    return new Promise((resolve, reject) => {
      const tx = this.db.transaction('todos', 'readwrite');
      const store = tx.objectStore('todos');
      const request = store.put(todo);
      
      request.onsuccess = () => resolve(request.result);
      request.onerror = () => reject(request.error);
    });
  }
  
  async delete(id) {
    return new Promise((resolve, reject) => {
      const tx = this.db.transaction('todos', 'readwrite');
      const store = tx.objectStore('todos');
      const request = store.delete(id);
      
      request.onsuccess = () => resolve();
      request.onerror = () => reject(request.error);
    });
  }
  
  async getAll() {
    return new Promise((resolve, reject) => {
      const tx = this.db.transaction('todos', 'readonly');
      const store = tx.objectStore('todos');
      const request = store.getAll();
      
      request.onsuccess = () => resolve(request.result);
      request.onerror = () => reject(request.error);
    });
  }
}

// Usage
const db = new TodoDB();
await db.open();
await db.add({ title: 'Learn IndexedDB', completed: false });
const todos = await db.getAll();
```

### 2. Querying with Indexes

```javascript
async getByIndex(indexName, value) {
  return new Promise((resolve, reject) => {
    const tx = this.db.transaction('todos', 'readonly');
    const store = tx.objectStore('todos');
    const index = store.index(indexName);
    const request = index.getAll(value);
    
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

// Get all completed todos
const completed = await db.getByIndex('completed', true);
```

### 3. Cursor-based Iteration

```javascript
async getAllWithCursor(callback) {
  return new Promise((resolve, reject) => {
    const tx = this.db.transaction('todos', 'readonly');
    const store = tx.objectStore('todos');
    const request = store.openCursor();
    
    request.onsuccess = (e) => {
      const cursor = e.target.result;
      if (cursor) {
        callback(cursor.value);
        cursor.continue();
      } else {
        resolve();
      }
    };
    
    request.onerror = () => reject(request.error);
  });
}

// Process each record
await db.getAllWithCursor(todo => {
  console.log(todo.title);
});
```

### 4. Batch Operations

```javascript
async addBatch(items) {
  return new Promise((resolve, reject) => {
    const tx = this.db.transaction('todos', 'readwrite');
    const store = tx.objectStore('todos');
    
    items.forEach(item => store.add(item));
    
    tx.oncomplete = () => resolve();
    tx.onerror = () => reject(tx.error);
  });
}

await db.addBatch([
  { title: 'Task 1', completed: false },
  { title: 'Task 2', completed: false },
  { title: 'Task 3', completed: true }
]);
```

### 5. Complex Queries with Cursors

```javascript
async query(storeName, queryFn) {
  return new Promise((resolve, reject) => {
    const tx = this.db.transaction(storeName, 'readonly');
    const store = tx.objectStore(storeName);
    const results = [];
    const request = store.openCursor();
    
    request.onsuccess = (e) => {
      const cursor = e.target.result;
      if (cursor) {
        if (queryFn(cursor.value)) {
          results.push(cursor.value);
        }
        cursor.continue();
      } else {
        resolve(results);
      }
    };
    
    request.onerror = () => reject(request.error);
  });
}

// Find todos with "learn" in title
const learning = await db.query('todos', todo => 
  todo.title.toLowerCase().includes('learn')
);
```

### 6. Version Migration

```javascript
const request = indexedDB.open('MyDB', 3);

request.onupgradeneeded = (e) => {
  const db = e.target.result;
  const oldVersion = e.oldVersion;
  
  if (oldVersion < 1) {
    // Create initial schema
    const store = db.createObjectStore('users', { keyPath: 'id' });
    store.createIndex('email', 'email', { unique: true });
  }
  
  if (oldVersion < 2) {
    // Add new store
    db.createObjectStore('posts', { keyPath: 'id' });
  }
  
  if (oldVersion < 3) {
    // Modify existing store
    const tx = e.target.transaction;
    const store = tx.objectStore('users');
    store.createIndex('username', 'username', { unique: true });
  }
};
```

## Common Mistakes

```javascript
// ❌ Wrong: Not handling onupgradeneeded
const request = indexedDB.open('MyDB', 1);
request.onsuccess = (e) => {
  const db = e.target.result;
  // Store might not exist!
};

// ✅ Correct: Always handle upgrade
request.onupgradeneeded = (e) => {
  const db = e.target.result;
  db.createObjectStore('store', { keyPath: 'id' });
};

// ❌ Wrong: Using the wrong transaction mode
const tx = db.transaction('store', 'readwrite');
const store = tx.objectStore('store');
const request = store.get(1); // Should be 'readonly' for get

// ✅ Correct: Use appropriate mode
const tx = db.transaction('store', 'readonly');
const store = tx.objectStore('store');
const request = store.get(1);

// ❌ Wrong: Not waiting for transaction to complete
const tx = db.transaction('store', 'readwrite');
tx.objectStore('store').add(data);
// Data might not be saved yet!

// ✅ Correct: Wait for transaction completion
const tx = db.transaction('store', 'readwrite');
tx.objectStore('store').add(data);
await new Promise((resolve, reject) => {
  tx.oncomplete = resolve;
  tx.onerror = () => reject(tx.error);
});
```

## Promisified Wrapper

```javascript
function promisifyRequest(request) {
  return new Promise((resolve, reject) => {
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

function promisifyTransaction(tx) {
  return new Promise((resolve, reject) => {
    tx.oncomplete = resolve;
    tx.onerror = () => reject(tx.error);
    tx.onabort = () => reject(tx.error);
  });
}

// Usage
const db = await promisifyRequest(indexedDB.open('MyDB', 1));
const tx = db.transaction('store', 'readwrite');
const store = tx.objectStore('store');
await promisifyRequest(store.add(data));
await promisifyTransaction(tx);
```

## Quick Revision Summary

- IndexedDB is async and event-based
- Uses object stores (like tables) with key-value records
- Transactions ensure data consistency
- Use indexes for fast queries
- Handle `onupgradeneeded` for schema creation/updates
- Always use appropriate transaction modes

## Related Topics

- [[LocalStorage]] - Simple key-value storage
- [[SessionStorage]] - Session-specific storage
- [[CacheAPI]] - Network request caching
- [[ServiceWorkers]] - Offline support with IndexedDB
- [[WebSQL]] - Deprecated database API
