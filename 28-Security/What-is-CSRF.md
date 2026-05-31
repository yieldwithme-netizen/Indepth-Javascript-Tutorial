# What is CSRF?

## Definition

CSRF (Cross-Site Request Forgery) is an attack where **malicious websites trick users into performing actions** on a different site where they're authenticated, exploiting the user's existing session.

## How CSRF Works

| Step | Description |
|------|-------------|
| 1. User logs into site A | Session cookie stored |
| 2. User visits malicious site B | Site B sends request to A |
| 3. Browser includes cookies | Session cookie sent automatically |
| 4. Action performed | Site A processes request |

## Code Examples

### 1. CSRF Protection with Tokens

```javascript
import crypto from 'crypto';
import csrf from 'csurf';

// Generate CSRF token
const csrfProtection = csrf({ cookie: true });

// Include token in form
app.get('/form', csrfProtection, (req, res) => {
  res.send(`
    <form action="/process" method="POST">
      <input type="hidden" name="_csrf" value="${req.csrfToken()}">
      <input type="text" name="data">
      <button type="submit">Submit</button>
    </form>
  `);
});

// Validate token on POST
app.post('/process', csrfProtection, (req, res) => {
  // Token is validated automatically by middleware
  res.json({ success: true });
});
```

### 2. Double Submit Cookie Pattern

```javascript
import crypto from 'crypto';

// Generate and set CSRF cookie
app.get('/api/csrf-token', (req, res) => {
  const token = crypto.randomBytes(32).toString('hex');

  res.cookie('csrf-token', token, {
    httpOnly: false,  // JavaScript needs to read it
    secure: true,
    sameSite: 'strict'
  });

  res.json({ token });
});

// Validate on mutations
function validateCSRF(req, res, next) {
  const tokenFromCookie = req.cookies['csrf-token'];
  const tokenFromHeader = req.headers['x-csrf-token'];

  if (!tokenFromCookie || !tokenFromHeader) {
    return res.status(403).json({ error: 'CSRF token missing' });
  }

  if (tokenFromCookie !== tokenFromHeader) {
    return res.status(403).json({ error: 'CSRF token invalid' });
  }

  next();
}

// Use in routes
app.post('/api/data', validateCSRF, (req, res) => {
  // Process request
});
```

### 3. SameSite Cookie Attribute

```javascript
// Set cookies with SameSite attribute
app.use(session({
  secret: process.env.SESSION_SECRET,
  cookie: {
    secure: true,
    httpOnly: true,
    sameSite: 'strict'  // or 'lax'
  }
}));

// 'strict' - Only send with same-site requests
// 'lax' - Send with top-level navigations
// 'none' - Send everywhere (requires Secure)
```

### 4. Custom CSRF Protection Middleware

```javascript
import crypto from 'crypto';

const tokens = new Map();

function generateToken(sessionId) {
  const token = crypto.randomBytes(32).toString('hex');
  tokens.set(sessionId, {
    token,
    expires: Date.now() + 3600000 // 1 hour
  });
  return token;
}

function validateToken(sessionId, token) {
  const stored = tokens.get(sessionId);

  if (!stored) return false;
  if (stored.expires < Date.now()) {
    tokens.delete(sessionId);
    return false;
  }

  return stored.token === token;
}

// Middleware
function csrfProtection(req, res, next) {
  if (['POST', 'PUT', 'DELETE'].includes(req.method)) {
    const token = req.headers['x-csrf-token'];
    const sessionId = req.session.id;

    if (!validateToken(sessionId, token)) {
      return res.status(403).json({ error: 'Invalid CSRF token' });
    }
  }
  next();
}

// Get token endpoint
app.get('/api/csrf', (req, res) => {
  const token = generateToken(req.session.id);
  res.json({ token });
});
```

### 5. React with CSRF Protection

```javascript
// Fetch wrapper with CSRF token
async function apiRequest(url, options = {}) {
  // Get CSRF token from cookie
  const csrfToken = document.cookie
    .split('; ')
    .find(row => row.startsWith('csrf-token='))
    ?.split('=')[1];

  const response = await fetch(url, {
    ...options,
    headers: {
      ...options.headers,
      'X-CSRF-Token': csrfToken,
      'Content-Type': 'application/json'
    },
    credentials: 'same-origin'  // Include cookies
  });

  return response.json();
}

// Usage
function submitForm(data) {
  return apiRequest('/api/submit', {
    method: 'POST',
    body: JSON.stringify(data)
  });
}
```

### 6. Express with Helmet for CSRF

```javascript
import helmet from 'helmet';

app.use(helmet());

// Custom CSRF protection
app.use((req, res, next) => {
  if (req.method === 'GET') {
    // Generate token for forms
    res.locals.csrfToken = crypto.randomBytes(32).toString('hex');
  }
  next();
});

// In template
app.get('/form', (req, res) => {
  res.send(`
    <form method="POST">
      <input type="hidden" name="_csrf" value="${res.locals.csrfToken}">
      <input type="text" name="username">
      <button type="submit">Submit</button>
    </form>
  `);
});
```

## Common Use Cases

```javascript
// Protect state-changing operations
app.post('/transfer', csrfProtection, async (req, res) => {
  // Transfer funds - must be protected
  await transferFunds(req.body.amount);
  res.json({ success: true });
});

// Don't protect safe methods
app.get('/data', (req, res) => {
  // GET is safe - no CSRF needed
  res.json({ data: 'public' });
});

// API with token validation
app.put('/api/profile', validateCSRF, async (req, res) => {
  await updateProfile(req.user.id, req.body);
  res.json({ success: true });
});
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| No CSRF protection | All forms vulnerable |
| Using GET for mutations | Easy to exploit |
| Predictable tokens | Token guessing |
| Token in URL | Referrer leakage |
| Missing SameSite cookies | Session hijacking |
| Not validating token | Bypass protection |
| Client-side only validation | Easily bypassed |

## Quick Revision

- CSRF tricks users into performing unwanted actions
- Always use CSRF tokens for state-changing operations
- Use SameSite cookie attribute as additional protection
- Validate tokens on server side
- Use POST, PUT, DELETE for mutations
- GET requests should never modify data
- Double submit cookie pattern is effective
- Combine multiple protection methods
- Test CSRF protection thoroughly

---

## Related Topics

- [[What-is-XSS]] - Related attack vector
- [[Prevent-XSS]] - XSS prevention
- [[What-is-Authentication]] - Session management
- [[What-is-CSP]] - Content Security Policy
- [[What-is-HTTPS]] - HTTPS protection
- [[What-is-OWASP]] - OWASP Top 10
