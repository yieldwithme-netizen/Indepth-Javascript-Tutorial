# CRUD Operations

CRUD stands for Create, Read, Update, and Delete - the four basic operations of persistent storage. These operations form the foundation of most web applications and database interactions.

## What is CRUD?

CRUD represents the fundamental operations performed on data:

- **Create**: Add new records
- **Read**: Retrieve existing records
- **Update**: Modify existing records
- **Delete**: Remove records

## CRUD with Arrays (In-Memory)

```javascript
class UserDatabase {
  constructor() {
    this.users = [];
    this.nextId = 1;
  }

  // CREATE
  create(user) {
    const newUser = {
      id: this.nextId++,
      ...user,
      createdAt: new Date()
    };
    this.users.push(newUser);
    return newUser;
  }

  // READ
  read(id) {
    return this.users.find(user => user.id === id);
  }

  readAll() {
    return this.users;
  }

  readByCondition(callback) {
    return this.users.filter(callback);
  }

  // UPDATE
  update(id, updates) {
    const index = this.users.findIndex(user => user.id === id);
    if (index !== -1) {
      this.users[index] = { ...this.users[index], ...updates };
      return this.users[index];
    }
    return null;
  }

  // DELETE
  delete(id) {
    const index = this.users.findIndex(user => user.id === id);
    if (index !== -1) {
      return this.users.splice(index, 1)[0];
    }
    return null;
  }
}

// Usage
const db = new UserDatabase();
db.create({ name: 'Alice', email: 'alice@example.com' });
db.read(1);
db.update(1, { name: 'Alice Smith' });
db.delete(1);
```

## CRUD with Objects (Key-Value Store)

```javascript
class KeyValueStore {
  constructor() {
    this.store = {};
  }

  create(key, value) {
    if (this.store[key]) {
      throw new Error('Key already exists');
    }
    this.store[key] = value;
    return true;
  }

  read(key) {
    return this.store[key] || null;
  }

  readAll() {
    return { ...this.store };
  }

  update(key, value) {
    if (!this.store[key]) {
      throw new Error('Key not found');
    }
    this.store[key] = value;
    return true;
  }

  delete(key) {
    if (!this.store[key]) {
      return false;
    }
    delete this.store[key];
    return true;
  }
}
```

## CRUD with localStorage

```javascript
class LocalStorageCRUD {
  static create(key, value) {
    const existing = localStorage.getItem(key);
    if (existing) {
      throw new Error('Key already exists');
    }
    localStorage.setItem(key, JSON.stringify(value));
    return true;
  }

  static read(key) {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : null;
  }

  static update(key, value) {
    const existing = localStorage.getItem(key);
    if (!existing) {
      throw new Error('Key not found');
    }
    localStorage.setItem(key, JSON.stringify(value));
    return true;
  }

  static delete(key) {
    if (!localStorage.getItem(key)) {
      return false;
    }
    localStorage.removeItem(key);
    return true;
  }

  static readAll() {
    const items = {};
    for (let i = 0; i < localStorage.length; i++) {
      const key = localStorage.key(i);
      items[key] = JSON.parse(localStorage.getItem(key));
    }
    return items;
  }
}
```

## CRUD with API (Fetch)

```javascript
class ApiCRUD {
  constructor(baseUrl) {
    this.baseUrl = baseUrl;
  }

  async create(data) {
    const response = await fetch(this.baseUrl, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    return response.json();
  }

  async read(id) {
    const response = await fetch(`${this.baseUrl}/${id}`);
    return response.json();
  }

  async readAll() {
    const response = await fetch(this.baseUrl);
    return response.json();
  }

  async update(id, data) {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    return response.json();
  }

  async delete(id) {
    const response = await fetch(`${this.baseUrl}/${id}`, {
      method: 'DELETE'
    });
    return response.json();
  }
}

// Usage
const api = new ApiCRUD('https://api.example.com/users');
await api.create({ name: 'Bob' });
await api.read(1);
await api.update(1, { name: 'Bob Smith' });
await api.delete(1);
```

## Common Use Cases

- User management systems
- Product catalogs
- Blog post management
- Task/todo applications
- Data dashboards

## Common Mistakes

1. **Not handling errors** - Always wrap operations in try/catch
2. **Missing validation** - Validate data before CRUD operations
3. **Not checking existence** - Verify record exists before update/delete
4. **Forgetting indexes** - Use proper data structures for quick lookups
5. **Race conditions** - Handle concurrent operations carefully

## Related Topics

- [[Database-Design]]
- [[API-Development]]
- [[Data-Validation]]
- [[Error-Handling]]
- [[Async-Programming]]

## Quick Revision

| Operation | Method | HTTP Equivalent |
|-----------|--------|-----------------|
| Create | `create()` | POST |
| Read | `read()` | GET |
| Update | `update()` | PUT/PATCH |
| Delete | `delete()` | DELETE |

CRUD operations are the building blocks of data management in web applications, providing a consistent pattern for interacting with persistent data.