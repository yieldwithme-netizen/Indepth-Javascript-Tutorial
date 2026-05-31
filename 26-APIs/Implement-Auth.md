# How to Implement Authentication

Authentication is the process of verifying the identity of a user, device, or system. It ensures that the entity accessing protected resources is who it claims to be.

## Types of Authentication

1. **Basic Authentication** - Username/password sent with each request
2. **Token-based Authentication (JWT)** - Server issues a token after login
3. **OAuth 2.0** - Third-party authentication (Google, GitHub login)
4. **API Key Authentication** - Simple key-based access control
5. **Session-based Authentication** - Server maintains session state

## Token-Based Authentication (JWT)

```javascript
const jwt = require('jsonwebtoken');

const SECRET_KEY = 'your-secret-key';

// Generate JWT token
function generateToken(user) {
  return jwt.sign(
    { userId: user.id, email: user.email },
    SECRET_KEY,
    { expiresIn: '24h' }
  );
}

// Verify JWT token middleware
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }

  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(403).json({ error: 'Invalid or expired token' });
  }
}

// Login endpoint
app.post('/login', async (req, res) => {
  const { email, password } = req.body;
  const user = await findUserByEmail(email);

  if (!user || !await bcrypt.compare(password, user.password)) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  const token = generateToken(user);
  res.json({ token, user: { id: user.id, email: user.email } });
});

// Protected route
app.get('/profile', authenticateToken, (req, res) => {
  res.json({ user: req.user });
});
```

## OAuth 2.0 Implementation

```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  (accessToken, refreshToken, profile, done) => {
    User.findOrCreate({ googleId: profile.id }, (err, user) => {
      return done(err, user);
    });
  }
));

app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    res.redirect('/dashboard');
  }
);
```

## API Key Authentication

```javascript
// Middleware for API key validation
function authenticateApiKey(req, res, next) {
  const apiKey = req.headers['x-api-key'];

  if (!apiKey) {
    return res.status(401).json({ error: 'API key required' });
  }

  const validKey = await validateApiKey(apiKey);

  if (!validKey) {
    return res.status(403).json({ error: 'Invalid API key' });
  }

  req.apiClient = validKey;
  next();
}

// Using API key authentication
app.get('/api/data', authenticateApiKey, (req, res) => {
  res.json({ data: 'Protected data' });
});
```

## Password Hashing with bcrypt

```javascript
const bcrypt = require('bcrypt');

// Hash password before storing
async function hashPassword(password) {
  const saltRounds = 12;
  return await bcrypt.hash(password, saltRounds);
}

// Verify password during login
async function verifyPassword(password, hashedPassword) {
  return await bcrypt.compare(password, hashedPassword);
}
```

## Common Use Cases

- Protecting API endpoints from unauthorized access
- User login and registration systems
- Third-party service integration
- Mobile app backend authentication
- Microservice-to-microservice authentication

## Common Mistakes

1. **Storing passwords in plain text** - Always hash passwords
2. **Using weak secret keys** - Use strong, random keys
3. **Not validating tokens on every request** - Always verify tokens
4. **Exposing tokens in URLs** - Send tokens in headers, not query params
5. **Ignoring token expiration** - Implement proper token refresh mechanisms
6. **Hardcoding secrets** - Use environment variables

## Related Topics

- [[What-is-RateLimit]]
- [[Store-Secrets]]
- [[What-is-OWASP]]
- [[What-is-Documentation]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| JWT | JSON Web Token for stateless authentication |
| OAuth 2.0 | Delegated authorization framework |
| API Key | Simple key-based access control |
| bcrypt | Password hashing library |
| Middleware | Function that runs before route handlers |
| Access Token | Token granting temporary access |
| Refresh Token | Token used to obtain new access tokens |
