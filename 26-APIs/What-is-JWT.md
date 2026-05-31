# What is JWT

## Definition

**JWT (JSON Web Token)** is a compact, URL-safe means of representing claims to be transferred between two parties. JWTs consist of three parts: header, payload, and signature, used for authentication and authorization in web applications.

## JWT Structure

```javascript
// JWT Format: header.payload.signature
// Example: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjEsInJvbGUiOiJ1c2VyIn0.signature

// Base64 decoded:
// Header: { "alg": "HS256", "typ": "JWT" }
// Payload: { "userId": 1, "role": "user", "iat": 1640995200, "exp": 1641081600 }
// Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

## Creating JWTs

```javascript
const jwt = require('jsonwebtoken');

// Secret key (store in environment variable)
const SECRET_KEY = process.env.JWT_SECRET;

// Generate token
function generateToken(user) {
  return jwt.sign(
    {
      userId: user.id,
      email: user.email,
      role: user.role
    },
    SECRET_KEY,
    { expiresIn: '24h' }
  );
}

// Generate refresh token
function generateRefreshToken(user) {
  return jwt.sign(
    { userId: user.id },
    SECRET_KEY,
    { expiresIn: '7d' }
  );
}

// Usage
const user = { id: 1, email: 'john@example.com', role: 'user' };
const token = generateToken(user);
const refreshToken = generateRefreshToken(user);
```

## Verifying JWTs

```javascript
// Verify token
function verifyToken(token) {
  try {
    return jwt.verify(token, SECRET_KEY);
  } catch (error) {
    throw new Error('Invalid token');
  }
}

// Express middleware
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.sendStatus(401); // Unauthorized
  }
  
  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    req.user = decoded;
    next();
  } catch (error) {
    return res.sendStatus(403); // Forbidden
  }
};

// Use middleware
app.get('/api/protected', authenticateToken, (req, res) => {
  res.json({ message: 'Protected data', user: req.user });
});
```

## Authentication Flow

```javascript
// Login endpoint
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body;
  
  // Find user
  const user = await User.findOne({ where: { email } });
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Verify password (using bcrypt)
  const validPassword = await bcrypt.compare(password, user.password);
  if (!validPassword) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Generate tokens
  const token = generateToken(user);
  const refreshToken = generateRefreshToken(user);
  
  res.json({
    token,
    refreshToken,
    user: {
      id: user.id,
      email: user.email,
      role: user.role
    }
  });
});

// Refresh token endpoint
app.post('/api/refresh', (req, res) => {
  const { refreshToken } = req.body;
  
  if (!refreshToken) {
    return res.sendStatus(401);
  }
  
  try {
    const decoded = jwt.verify(refreshToken, SECRET_KEY);
    const user = { id: decoded.userId };
    const newToken = generateToken(user);
    
    res.json({ token: newToken });
  } catch (error) {
    return res.sendStatus(403);
  }
});
```

## Role-Based Access Control

```javascript
// Role-based middleware
const authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
};

// Usage
app.get('/api/admin', 
  authenticateToken, 
  authorize('admin'), 
  (req, res) => {
    res.json({ message: 'Admin only data' });
  }
);

app.get('/api/moderator', 
  authenticateToken, 
  authorize('admin', 'moderator'), 
  (req, res) => {
    res.json({ message: 'Moderator data' });
  }
);
```

## Client-Side Storage

```javascript
// Store token
localStorage.setItem('token', token);

// Send with requests
const token = localStorage.getItem('token');
fetch('/api/data', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});

// Or use axios defaults
axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;

// Clear on logout
localStorage.removeItem('token');
```

## Common Mistakes

1. **Storing sensitive data in payload**: JWT payload is base64, not encrypted
2. **Using weak secrets**: Use strong, random secret keys
3. **Not setting expiration**: Always set `expiresIn`
4. **Storing tokens in localStorage**: Consider httpOnly cookies
5. **Not validating tokens**: Always verify on server

## Related Topics

- [[Handle-CORS]]
- [[What-is-CORS]]
- [[What-is-REST]]

## Quick Revision

- JWT has three parts: header, payload, signature
- Use `jsonwebtoken` library in Node.js
- Always verify tokens on server
- Set expiration time for security
- Use refresh tokens for long-lived sessions
- Never store sensitive data in JWT payload
- Consider httpOnly cookies over localStorage
