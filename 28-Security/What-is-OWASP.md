# What is OWASP

OWASP (Open Web Application Security Project) is a nonprofit foundation that works to improve the security of software. It provides free resources, tools, and standards for securing web applications.

## OWASP Top 10 (2021)

The OWASP Top 10 is a standard awareness document representing a broad consensus about the most critical security risks to web applications.

### 1. Broken Access Control

```javascript
// ❌ BAD: Missing authorization check
app.get('/api/admin/users', (req, res) => {
  // Any authenticated user can access admin data
  const users = db.getAllUsers();
  res.json(users);
});

// ✅ GOOD: Proper authorization check
app.get('/api/admin/users', authenticateToken, authorize('admin'), (req, res) => {
  const users = db.getAllUsers();
  res.json(users);
});

function authorize(...allowedRoles) {
  return (req, res, next) => {
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
}
```

### 2. Cryptographic Failures

```javascript
// ❌ BAD: Weak hashing
const hash = md5(password); // Never use MD5 for passwords

// ❌ BAD: No salt
const hash = bcrypt.hashSync(password, 10); // Predictable

// ✅ GOOD: Strong hashing with salt
const salt = await bcrypt.genSalt(12);
const hash = await bcrypt.hash(password, salt);

// ✅ GOOD: Using crypto for encryption
const crypto = require('crypto');
const algorithm = 'aes-256-gcm';
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);
```

### 3. Injection

```javascript
// ❌ BAD: SQL Injection vulnerability
const query = `SELECT * FROM users WHERE id = ${userId}`;
db.query(query);

// ✅ GOOD: Parameterized query
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);

// ❌ BAD: NoSQL Injection
const user = await User.findOne({ username: req.body.username });

// ✅ GOOD: Input validation and sanitization
const username = sanitize(req.body.username);
const user = await User.findOne({ username });

// ✅ GOOD: Using an ORM
const user = await User.findByPk(req.body.id);
```

### 4. Insecure Design

```javascript
// ❌ BAD: Password reset without rate limiting
app.post('/reset-password', async (req, res) => {
  const { email } = req.body;
  const token = crypto.randomBytes(32).toString('hex');
  await sendResetEmail(email, token);
  // No rate limiting - attacker can spam requests
});

// ✅ GOOD: Rate-limited password reset
const resetLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 3, // 3 attempts per hour
  message: 'Too many password reset attempts'
});

app.post('/reset-password', resetLimiter, async (req, res) => {
  const { email } = req.body;
  const token = crypto.randomBytes(32).toString('hex');
  await sendResetEmail(email, token);
  res.json({ message: 'If email exists, reset link sent' });
});
```

### 5. Security Misconfiguration

```javascript
// ❌ BAD: Debug mode in production
app.use(errorHandler({ dumpExceptions: true, showStack: true }));

// ✅ GOOD: Environment-based configuration
if (process.env.NODE_ENV === 'production') {
  app.use(errorHandler({ dumpExceptions: false, showStack: false }));
}

// ❌ BAD: Default credentials
const db = mongoose.connect('mongodb://admin:password@localhost/db');

// ✅ GOOD: Environment variables
const db = mongoose.connect(process.env.DATABASE_URL);
```

### 6. Vulnerable Components

```javascript
// Check for vulnerable dependencies
// npm audit
// npm audit fix

// Use dependency scanning
const audit = require('npm-audit-resolver');
audit.check().then(report => {
  console.log(report);
});
```

### 7. Authentication Failures

```javascript
// ❌ BAD: No account lockout
app.post('/login', async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  if (user && await bcrypt.compare(req.body.password, user.password)) {
    res.json({ token: generateToken(user) });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});

// ✅ GOOD: Account lockout after failed attempts
app.post('/login', loginLimiter, async (req, res) => {
  const user = await User.findOne({ email: req.body.email });

  if (!user || !(await bcrypt.compare(req.body.password, user.password))) {
    await incrementFailedAttempts(user);
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  await resetFailedAttempts(user);
  res.json({ token: generateToken(user) });
});
```

### 8. Software and Data Integrity Failures

```javascript
// ❌ BAD: Loading scripts from untrusted sources
<script src="http://example.com/library.js"></script>

// ✅ GOOD: Using Subresource Integrity
<script src="https://cdn.example.com/library.js" 
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
        crossorigin="anonymous"></script>
```

### 9. Security Logging and Monitoring Failures

```javascript
// Log security events
const securityLogger = {
  logLoginAttempt(email, success, ip) {
    console.log(JSON.stringify({
      event: 'LOGIN_ATTEMPT',
      email,
      success,
      ip,
      timestamp: new Date().toISOString()
    }));
  },

  logPrivilegeEscalation(userId, action) {
    console.log(JSON.stringify({
      event: 'PRIVILEGE_ESCALATION',
      userId,
      action,
      timestamp: new Date().toISOString()
    }));
  }
};
```

### 10. Server-Side Request Forgery (SSRF)

```javascript
// ❌ BAD: User-controlled URL
app.get('/fetch', async (req, res) => {
  const response = await fetch(req.query.url); // SSRF vulnerability
  res.send(response.data);
});

// ✅ GOOD: URL validation
const allowedHosts = ['api.example.com', 'cdn.example.com'];

app.get('/fetch', async (req, res) => {
  const url = new URL(req.query.url);

  if (!allowedHosts.includes(url.hostname)) {
    return res.status(400).json({ error: 'Invalid URL' });
  }

  const response = await fetch(url.toString());
  res.send(response.data);
});
```

## Common Use Cases

- **Security audits**: Identifying vulnerabilities in applications
- **Compliance requirements**: Meeting security standards
- **Security training**: Educating developers about security
- **Risk assessment**: Evaluating application security posture
- **Secure development**: Building security into the development process

## Common Mistakes

1. **Ignoring the OWASP Top 10** - Not understanding common vulnerabilities
2. **Not validating input** - Allowing malicious data
3. **Hardcoding secrets** - Exposing sensitive information
4. **Not using HTTPS** - Transmitting data insecurely
5. **Insufficient logging** - Missing security events

## Related Topics

- [[Implement-Auth]]
- [[Store-Secrets]]
- [[What-is-RateLimit]]
- [[What-is-Documentation]]

## Quick Revision

| Risk | Description | Prevention |
|------|-------------|------------|
| Broken Access | Unauthorized data access | Role-based access control |
| Injection | Malicious code execution | Input validation, parameterized queries |
| XSS | Malicious script execution | Output encoding, CSP |
| CSRF | Unauthorized actions | CSRF tokens |
| Misconfiguration | Default/insecure settings | Security hardening |

**OWASP Resources:**
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
