# What is Authentication?

## Definition

Authentication is the process of **verifying the identity** of a user, device, or system - confirming "who you are" through credentials like passwords, tokens, or biometrics.

## Authentication Methods

| Method | Description | Security Level |
|--------|-------------|----------------|
| Password | Something you know | Medium |
| Token/JWT | Something you have | High |
| OAuth | Third-party verification | High |
| Biometric | Something you are | Very High |
| MFA | Multiple methods combined | Very High |

## Code Examples

### 1. Basic Password Authentication

```javascript
// Registration
import bcrypt from 'bcrypt';

async function registerUser(email, password) {
  // Hash password before storing
  const saltRounds = 12;
  const hashedPassword = await bcrypt.hash(password, saltRounds);

  // Store in database
  await db.users.create({
    email,
    password: hashedPassword
  });
}

// Login
async function loginUser(email, password) {
  const user = await db.users.findByEmail(email);

  if (!user) {
    throw new Error('User not found');
  }

  const isValid = await bcrypt.compare(password, user.password);

  if (!isValid) {
    throw new Error('Invalid password');
  }

  return user;
}
```

### 2. JWT (JSON Web Tokens)

```javascript
import jwt from 'jsonwebtoken';

const SECRET_KEY = process.env.JWT_SECRET;

// Generate token
function generateToken(user) {
  return jwt.sign(
    {
      userId: user.id,
      email: user.email
    },
    SECRET_KEY,
    { expiresIn: '24h' }
  );
}

// Verify token middleware
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Access denied' });
  }

  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(403).json({ error: 'Invalid token' });
  }
}

// Usage in routes
app.get('/protected', authenticateToken, (req, res) => {
  res.json({ message: 'Protected content', user: req.user });
});
```

### 3. OAuth 2.0 with Passport.js

```javascript
import passport from 'passport';
import { Strategy as GoogleStrategy } from 'passport-google-oauth20';

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  async (accessToken, refreshToken, profile, done) => {
    let user = await db.users.findByGoogleId(profile.id);

    if (!user) {
      user = await db.users.create({
        googleId: profile.id,
        email: profile.emails[0].value,
        name: profile.displayName
      });
    }

    done(null, user);
  }
));

// Routes
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

### 4. Session-Based Authentication

```javascript
import session from 'express-session';
import RedisStore from 'connect-redis';
import { createClient } from 'redis';

const redisClient = createClient();
redisClient.connect();

app.use(session({
  store: new RedisStore({ client: redisClient }),
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,      // HTTPS only
    httpOnly: true,    // No JavaScript access
    maxAge: 1000 * 60 * 60 * 24 // 24 hours
  }
}));

// Login route
app.post('/login', async (req, res) => {
  const user = await loginUser(req.body.email, req.body.password);

  if (user) {
    req.session.userId = user.id;
    res.json({ success: true });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});

// Logout route
app.post('/logout', (req, res) => {
  req.session.destroy();
  res.clearCookie('connect.sid');
  res.json({ success: true });
});
```

### 5. MFA (Multi-Factor Authentication)

```javascript
import speakeasy from 'speakeasy';
import QRCode from 'qrcode';

// Setup MFA
async function setupMFA(user) {
  const secret = speakeasy.generateSecret({
    name: `MyApp:${user.email}`
  });

  // Store secret in database
  await db.users.updateMFA(user.id, secret.base32);

  // Generate QR code
  const qrCodeUrl = await QRCode.toDataURL(secret.otpauth_url);

  return { qrCodeUrl, secret: secret.base32 };
}

// Verify MFA
function verifyMFA(token, userSecret) {
  return speakeasy.totp.verify({
    secret: userSecret,
    encoding: 'base32',
    token,
    window: 1  // Allow 30 seconds window
  });
}
```

### 6. Password Reset Flow

```javascript
import crypto from 'crypto';
import nodemailer from 'nodemailer';

async function requestPasswordReset(email) {
  const user = await db.users.findByEmail(email);
  if (!user) return; // Don't reveal if user exists

  // Generate reset token
  const resetToken = crypto.randomBytes(32).toString('hex');
  const resetTokenExpiry = Date.now() + 3600000; // 1 hour

  // Store hashed token
  await db.users.updateResetToken(user.id, resetToken, resetTokenExpiry);

  // Send email
  const resetUrl = `https://example.com/reset?token=${resetToken}`;
  await sendEmail(user.email, 'Password Reset', `Click here: ${resetUrl}`);
}

async function resetPassword(token, newPassword) {
  const user = await db.users.findByResetToken(token);

  if (!user || user.resetTokenExpiry < Date.now()) {
    throw new Error('Invalid or expired token');
  }

  const hashedPassword = await bcrypt.hash(newPassword, 12);
  await db.users.updatePassword(user.id, hashedPassword);
  await db.users.clearResetToken(user.id);
}
```

## Common Use Cases

```javascript
// Protect API routes
app.use('/api', authenticateToken);

// Role-based access
function authorizeRole(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
}

app.delete('/api/users/:id',
  authenticateToken,
  authorizeRole('admin'),
  deleteUser
);
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Storing plain text passwords | Data breach exposure |
| Weak password requirements | Easy to guess |
| No rate limiting on login | Brute force attacks |
| Long-lived sessions | Extended attack window |
| Missing HTTPS | Credential interception |
| No account lockout | Continued brute force |
| Predictable tokens | Token guessing |

## Quick Revision

- Never store plain text passwords - use bcrypt
- Use JWT or sessions for state management
- Implement OAuth for third-party login
- Always use HTTPS for authentication
- Add rate limiting to prevent brute force
- Use MFA for sensitive applications
- Implement secure password reset flows
- Set appropriate session expiration
- Validate tokens on every request

---

## Related Topics

- [[What-is-OWASP]] - OWASP Top 10 security risks
- [[What-is-HTTPS]] - Secure HTTP connections
- [[What-is-CSRF]] - CSRF protection
- [[Store-Secrets]] - Secure secret management
- [[What-is-CSP]] - Content Security Policy
- [[What-is-Authorization]] - Authorization vs authentication
