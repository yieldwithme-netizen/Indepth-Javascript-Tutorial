# What is Authorization

## Definition

Authorization is the process of determining whether an authenticated user has permission to access a specific resource or perform a particular action. While [[What-is-Authentication|Authentication]] verifies "who you are," authorization determines "what you can do."

Authorization happens after authentication and is enforced at various levels: application routes, API endpoints, database queries, and UI elements. Proper authorization ensures that users can only access data and functionality appropriate to their role or permissions.

## Authorization vs Authentication

| Aspect | Authentication | Authorization |
|--------|---------------|---------------|
| Question | Who are you? | What can you do? |
| Purpose | Verify identity | Grant permissions |
| Happens | First | After authentication |
| Data | Credentials, tokens | Roles, permissions, policies |
| Example | Login with password | Admin can delete, user cannot |

```javascript
// Authentication: Verify user identity
app.post('/login', (req, res) => {
  const user = findUserByEmail(req.body.email);
  if (user && bcrypt.compare(req.body.password, user.password)) {
    const token = jwt.sign({ userId: user.id }, secret);
    res.json({ token });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});

// Authorization: Check user permissions
app.delete('/posts/:id', authenticateToken, (req, res) => {
  const post = findPostById(req.params.id);
  if (post.authorId !== req.user.id && req.user.role !== 'admin') {
    return res.status(403).json({ error: 'Insufficient permissions' });
  }
  deletePost(post.id);
  res.json({ message: 'Post deleted' });
});
```

## Authorization Models

### 1. Role-Based Access Control (RBAC)

Users are assigned roles, and roles have specific permissions.

```javascript
// Role definitions
const roles = {
  admin: ['read', 'write', 'delete', 'manage_users'],
  editor: ['read', 'write'],
  viewer: ['read']
};

// Middleware to check roles
function authorize(...allowedRoles) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }
    
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    
    next();
  };
}

// Usage
app.get('/admin/dashboard', authorize('admin'), adminController.dashboard);
app.post('/articles', authorize('admin', 'editor'), articlesController.create);
app.get('/articles', authorize('admin', 'editor', 'viewer'), articlesController.list);
```

### 2. Attribute-Based Access Control (ABAC)

Access based on attributes of the user, resource, or environment.

```javascript
function authorizeABAC(policy) {
  return (req, res, next) => {
    const context = {
      user: req.user,
      resource: req.resource,
      environment: {
        time: new Date(),
        ip: req.ip
      }
    };
    
    if (policy.evaluate(context)) {
      return next();
    }
    
    return res.status(403).json({ error: 'Access denied by policy' });
  };
}

// Policy: Users can only edit their own posts
const ownPostPolicy = {
  evaluate: (context) => {
    return context.user.id === context.resource.authorId ||
           context.user.role === 'admin';
  }
};

app.put('/posts/:id', authenticateToken, findPost, authorizeABAC(ownPostPolicy), updatePost);
```

### 3. Permission-Based Access Control

Users are assigned specific permissions directly.

```javascript
const permissions = new Map();

// Set user permissions
function grantPermission(userId, resource, action) {
  const key = `${userId}:${resource}:${action}`;
  permissions.set(key, true);
}

function revokePermission(userId, resource, action) {
  const key = `${userId}:${resource}:${action}`;
  permissions.delete(key);
}

function hasPermission(userId, resource, action) {
  const key = `${userId}:${resource}:${action}`;
  return permissions.has(key);
}

// Middleware
function checkPermission(resource, action) {
  return (req, res, next) => {
    if (!hasPermission(req.user.id, resource, action)) {
      return res.status(403).json({ 
        error: `Permission denied: ${resource}:${action}` 
      });
    }
    next();
  };
}

// Usage
grantPermission('user123', 'posts', 'read');
grantPermission('user123', 'posts', 'write');
revokePermission('user123', 'posts', 'delete');

app.get('/posts', checkPermission('posts', 'read'), getPosts);
app.post('/posts', checkPermission('posts', 'write'), createPost);
```

## Implementation Examples

### Route-Level Authorization

```javascript
const express = require('express');
const router = express.Router();

// Authentication middleware (verify token)
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(401).json({ error: 'Invalid token' });
  }
}

// Authorization middleware
function authorize(roles = []) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }
    
    if (roles.length && !roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    
    next();
  };
}

// Protected routes
router.get('/profile', authenticate, (req, res) => {
  res.json(req.user);
});

router.get('/admin/users', authenticate, authorize(['admin']), (req, res) => {
  res.json(users);
});

router.delete('/posts/:id', authenticate, authorize(['admin', 'editor']), deletePost);
```

### Resource-Level Authorization

```javascript
class Post {
  constructor(id, authorId, content, visibility) {
    this.id = id;
    this.authorId = authorId;
    this.content = content;
    this.visibility = visibility; // 'public', 'private', 'friends'
  }
  
  canRead(user) {
    if (this.visibility === 'public') return true;
    if (!user) return false;
    if (this.authorId === user.id) return true;
    if (user.role === 'admin') return true;
    if (this.visibility === 'friends' && user.friends?.includes(this.authorId)) {
      return true;
    }
    return false;
  }
  
  canEdit(user) {
    if (!user) return false;
    return this.authorId === user.id || user.role === 'admin';
  }
  
  canDelete(user) {
    if (!user) return false;
    return this.authorId === user.id || user.role === 'admin';
  }
}

// Middleware
function authorizePost(action) {
  return (req, res, next) => {
    const post = req.post;
    
    if (!post) {
      return res.status(404).json({ error: 'Post not found' });
    }
    
    const canPerformAction = {
      read: () => post.canRead(req.user),
      edit: () => post.canEdit(req.user),
      delete: () => post.canDelete(req.user)
    };
    
    if (!canPerformAction[action]?.()) {
      return res.status(403).json({ error: `Cannot ${action} this post` });
    }
    
    next();
  };
}
```

### Database-Level Authorization

```javascript
// SQL query with authorization
async function getUserPosts(userId, requestingUser) {
  let query;
  let params;
  
  if (requestingUser.role === 'admin') {
    // Admin can see all posts
    query = 'SELECT * FROM posts WHERE author_id = ?';
    params = [userId];
  } else if (requestingUser.id === userId) {
    // Users can see all their own posts
    query = 'SELECT * FROM posts WHERE author_id = ?';
    params = [userId];
  } else {
    // Others can only see public posts
    query = 'SELECT * FROM posts WHERE author_id = ? AND visibility = ?';
    params = [userId, 'public'];
  }
  
  return await db.query(query, params);
}
```

### Frontend Authorization

```javascript
// React component with authorization
function AdminPanel({ user }) {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    if (user.role !== 'admin') {
      return;
    }
    
    fetch('/api/admin/users', {
      headers: { Authorization: `Bearer ${user.token}` }
    })
      .then(res => res.json())
      .then(data => setUsers(data));
  }, [user]);
  
  // Don't render anything if not admin
  if (user.role !== 'admin') {
    return <Redirect to="/dashboard" />;
  }
  
  return (
    <div>
      <h1>Admin Panel</h1>
      <UserList users={users} />
    </div>
  );
}

// Permission check utility
function hasPermission(user, permission) {
  const permissions = {
    admin: ['read', 'write', 'delete', 'manage_users'],
    editor: ['read', 'write'],
    viewer: ['read']
  };
  
  return permissions[user.role]?.includes(permission) ?? false;
}

// Conditional rendering
function ArticleActions({ user, article }) {
  return (
    <div>
      {hasPermission(user, 'write') && (
        <button onClick={() => editArticle(article.id)}>Edit</button>
      )}
      {(article.authorId === user.id || hasPermission(user, 'delete')) && (
        <button onClick={() => deleteArticle(article.id)}>Delete</button>
      )}
    </div>
  );
}
```

## Common Use Cases

- **User dashboards**: Users see only their own data
- **Admin panels**: Only administrators access management features
- **Content management**: Editors can publish, viewers can only read
- **API protection**: Rate limiting and access control per endpoint
- **Multi-tenant applications**: Tenant isolation and data segregation
- **Payment features**: Only premium users access certain functionality

## Common Mistakes

1. **Only implementing client-side authorization**
   ```javascript
   // Wrong: Client-side only check
   function canEdit(user) {
     return user.role === 'editor' || user.role === 'admin';
   }
   
   // Correct: Always verify on server
   app.put('/posts/:id', authenticate, (req, res) => {
     if (!canEdit(req.user)) {
       return res.status(403).json({ error: 'Insufficient permissions' });
     }
     // Update post
   });
   ```

2. **Ignoring ownership checks**
   ```javascript
   // Wrong: Any authenticated user can delete
   app.delete('/posts/:id', authenticate, deletePost);
   
   // Correct: Check ownership or admin role
   app.delete('/posts/:id', authenticate, (req, res) => {
     const post = findPost(req.params.id);
     if (post.authorId !== req.user.id && req.user.role !== 'admin') {
       return res.status(403).json({ error: 'Insufficient permissions' });
     }
     deletePost(post.id);
   });
   ```

3. **Hardcoding permissions**
   ```javascript
   // Wrong: Permissions scattered throughout code
   if (user.role === 'admin' || user.role === 'editor') { ... }
   
   // Correct: Centralized permission system
   if (user.hasPermission('posts:write')) { ... }
   ```

4. **Not handling role changes**
   ```javascript
   // Wrong: Not invalidating sessions when roles change
   // Fix: Include role in JWT and check on each request
   function authenticate(req, res, next) {
     const token = req.headers.authorization?.split(' ')[1];
     const decoded = jwt.verify(token, secret);
     const user = findUser(decoded.userId); // Fresh role check
     req.user = user;
     next();
   }
   ```

## Quick Revision Summary

- **Authorization** determines what an authenticated user can access
- **RBAC**: Assign roles to users, permissions to roles
- **ABAC**: Base access on attributes of user, resource, or environment
- **Always enforce authorization server-side**: Client-side checks are for UX only
- **Check ownership**: Users should only modify their own resources unless admin
- **Centralize permissions**: Use a permission system rather than scattered checks
- **Audit access logs**: Track authorization decisions for security monitoring

## Related Topics

- [[What-is-Authentication]]
- [[JWT]]
- [[Session-Management]]
- [[Role-Based-Access-Control]]
- [[API-Security]]
- [[Middleware]]
- [[Express]]
- [[Security-Best-Practices]]
