# Authentication in JavaScript

## Definition

Authentication is the process of verifying the identity of a user, device, or system. In web applications, it ensures that users are who they claim to be before granting access to protected resources.

## Types of Authentication

### 1. Password-Based Authentication

```javascript
// Basic login form handling
const loginForm = document.getElementById("login-form");

loginForm.addEventListener("submit", async (e) => {
  e.preventDefault();

  const username = document.getElementById("username").value;
  const password = document.getElementById("password").value;

  try {
    const response = await fetch("/api/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({ username, password })
    });

    const data = await response.json();

    if (data.success) {
      localStorage.setItem("token", data.token);
      window.location.href = "/dashboard";
    } else {
      showError("Invalid credentials");
    }
  } catch (error) {
    showError("Login failed");
  }
});
```

### 2. JWT (JSON Web Token) Authentication

```javascript
// Server-side JWT verification middleware
const jwt = require("jsonwebtoken");

function authenticateToken(req, res, next) {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1];

  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
}

// Client-side token storage and usage
class AuthService {
  constructor() {
    this.token = localStorage.getItem("authToken");
  }

  setToken(token) {
    this.token = token;
    localStorage.setItem("authToken", token);
  }

  getToken() {
    return this.token;
  }

  isAuthenticated() {
    if (!this.token) return false;

    // Check if token is expired
    const payload = JSON.parse(atob(this.token.split(".")[1]));
    return payload.exp * 1000 > Date.now();
  }

  logout() {
    this.token = null;
    localStorage.removeItem("authToken");
  }
}
```

### 3. OAuth 2.0 Authentication

```javascript
// Google OAuth example with Passport.js
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
  },
  (accessToken, refreshToken, profile, done) => {
    User.findOrCreate({ googleId: profile.id }, (err, user) => {
      return done(err, user);
    });
  }
));

// Client-side OAuth flow
async function loginWithGoogle() {
  const response = await fetch("/auth/google", {
    method: "GET",
    credentials: "include"
  });

  const { authUrl } = await response.json();
  window.location.href = authUrl;
}
```

### 4. Session-Based Authentication

```javascript
const express = require("express");
const session = require("express-session");

const app = express();

app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  }
}));

// Login route
app.post("/login", async (req, res) => {
  const { username, password } = req.body;
  const user = await authenticateUser(username, password);

  if (user) {
    req.session.userId = user.id;
    res.json({ success: true });
  } else {
    res.status(401).json({ error: "Invalid credentials" });
  }
});

// Protected route middleware
function requireAuth(req, res, next) {
  if (req.session.userId) {
    next();
  } else {
    res.status(401).json({ error: "Unauthorized" });
  }
}
```

## Best Practices

1. **Never store passwords in plain text** - Use bcrypt or argon2
2. **Use HTTPS** - Encrypt data in transit
3. **Implement rate limiting** - Prevent brute force attacks
4. **Use secure flags on cookies** - `httpOnly`, `secure`, `sameSite`
5. **Validate input** - Prevent injection attacks
6. **Implement CSRF protection** - Use tokens for form submissions

## Common Mistakes

- Storing JWT in localStorage (vulnerable to XSS)
- Using weak session secrets
- Not implementing proper logout mechanisms
- Exposing sensitive data in client-side code
- Missing rate limiting on authentication endpoints

## Related Topics

- [[JWT]] - JSON Web Tokens
- [[Sessions]] - Server-side session management
- [[Cookies]] - Browser cookie handling
- [[OAuth]] - Open Authorization protocol
- [[Password-Hashing]] - Secure password storage
- [[CSRF-Protection]] - Cross-Site Request Forgery prevention

## Quick Revision Summary

| Type | Storage | Stateless | Scalability |
|------|---------|-----------|-------------|
| JWT | Client-side | Yes | High |
| Session | Server-side | No | Medium |
| OAuth | Provider-managed | Yes | High |
| API Keys | Client-side | Yes | High |
