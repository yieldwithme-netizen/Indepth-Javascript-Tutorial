# What is Content Security Policy (CSP)?

## Definition

Content Security Policy (CSP) is a **security layer** that helps detect and mitigate XSS and data injection attacks by specifying which resources (scripts, styles, images) are allowed to load.

## CSP Directives

| Directive | Purpose |
|-----------|---------|
| default-src | Fallback for all resource types |
| script-src | Allowed JavaScript sources |
| style-src | Allowed CSS sources |
| img-src | Allowed image sources |
| font-src | Allowed font sources |
| connect-src | Allowed AJAX/WebSocket connections |
| frame-src | Allowed iframe sources |
| base-uri | Allowed base URLs |
| form-action | Allowed form submission targets |

## Code Examples

### 1. Basic CSP Header (Express.js)

```javascript
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', `
    default-src 'self';
    script-src 'self';
    style-src 'self' 'unsafe-inline';
    img-src 'self' data: https:;
    font-src 'self';
    connect-src 'self';
    frame-ancestors 'none';
    base-uri 'self';
    form-action 'self'
  `);
  next();
});
```

### 2. Inline Scripts with Nonce

```javascript
import crypto from 'crypto';

app.use((req, res, next) => {
  // Generate unique nonce for each request
  const nonce = crypto.randomBytes(16).toString('base64');
  res.locals.nonce = nonce;

  res.setHeader('Content-Security-Policy', `
    default-src 'self';
    script-src 'self' 'nonce-${nonce}';
    style-src 'self' 'nonce-${nonce}';
    img-src 'self' data: https:;
  `);
  next();
});

// In your template
app.get('/', (req, res) => {
  res.send(`
    <!DOCTYPE html>
    <html>
    <head>
      <style nonce="${res.locals.nonce}">
        body { font-family: sans-serif; }
      </style>
    </head>
    <body>
      <h1>Secure Page</h1>
      <script nonce="${res.locals.nonce}">
        console.log('This script is allowed');
      </script>
    </body>
    </html>
  `);
});
```

### 3. Trusted Types (Modern XSS Protection)

```javascript
// Enable Trusted Types in CSP
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', `
    trusted-types dompurify;
    default-src 'self';
    script-src 'self';
  `);
  next();
});

// Use Trusted Types in JavaScript
const policy = trustedTypes.createPolicy('dompurify', {
  createHTML: (input) => DOMPurify.sanitize(input),
  createScriptURL: (input) => {
    const url = new URL(input, document.baseURI);
    if (url.origin !== window.location.origin) {
      throw new Error('Cross-origin script URL blocked');
    }
    return url.toString();
  }
});

// Safe DOM manipulation
element.innerHTML = policy.createHTML(userContent);
```

### 4. CSP for React/SPA Applications

```javascript
// webpack.config.js or vite.config.js
module.exports = {
  // Generate nonce at build time or use hash
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      CSP: {
        'default-src': "'self'",
        'script-src': "'self'",
        'style-src': "'self' 'unsafe-inline'",
        'img-src': "'self' data: blob:",
        'font-src': "'self' https://fonts.gstatic.com",
        'connect-src': "'self' https://api.example.com",
        'frame-src': "'none'"
      }
    })
  ]
};
```

### 5. Reporting CSP Violations

```javascript
// Set up CSP violation endpoint
app.post('/csp-report', express.json({ type: 'application/csp-report' }), (req, res) => {
  const violation = req.body['csp-report'];

  console.error('CSP Violation:', {
    documentUri: violation['document-uri'],
    violatedDirective: violation['violated-directive'],
    blockedUri: violation['blocked-uri'],
    sourceFile: violation['source-file'],
    lineNumber: violation['line-number']
  });

  // Log to monitoring service
  reportToSecurityMonitoring(violation);

  res.status(204).end();
});

// CSP header with report URI
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy-Report-Only', `
    default-src 'self';
    script-src 'self';
    style-src 'self';
    report-uri /csp-report;
    report-to csp-endpoint
  `);
  next();
});
```

### 6. Common CSP Configurations

```javascript
// Development (more permissive)
const devCSP = `
  default-src 'self' 'unsafe-inline' 'unsafe-eval';
  script-src 'self' 'unsafe-inline' 'unsafe-eval' http://localhost:3000;
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: blob: http://localhost:3000;
  connect-src 'self' http://localhost:3000 ws://localhost:3000;
`;

// Production (strict)
const prodCSP = `
  default-src 'self';
  script-src 'self' 'strict-dynamic' 'nonce-${nonce}';
  style-src 'self' 'nonce-${nonce}';
  img-src 'self' data: https:;
  font-src 'self' https://fonts.gstatic.com;
  connect-src 'self' https://api.example.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
  upgrade-insecure-requests
`;
```

## Common Use Cases

```javascript
// Protect against inline script attacks
// ❌ Without CSP - vulnerable to XSS
// <script>document.cookie</script> can steal data

// ✅ With CSP - script-src 'self'
// Inline scripts are blocked by default

// Allow specific CDN resources
style-src 'self' https://cdn.jsdelivr.net;
script-src 'self' https://cdn.example.com;

// Block all frame embedding
frame-ancestors 'none';

// Prevent clickjacking
X-Frame-Options: DENY
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Using 'unsafe-inline' | Allows XSS attacks |
| Using 'unsafe-eval' | Allows code injection |
| Overly permissive policy | Weakened protection |
| No violation reporting | Can't detect attacks |
| Not testing CSP | Broken functionality |
| Missing img-src for data: | Broken images |
| No frame-ancestors | Clickjacking possible |

## Quick Revision

- CSP restricts which resources can load
- Use `default-src` as fallback for all types
- Avoid `unsafe-inline` and `unsafe-eval`
- Use nonces or hashes for inline scripts
- Enable CSP violation reporting
- Test with `Content-Security-Policy-Report-Only` first
- CSP is defense-in-depth, not a complete solution
- Combine with other security measures (XSS prevention, input sanitization)
- Report violations to monitor attack attempts

---

## Related Topics

- [[What-is-XSS]] - XSS attacks CSP helps prevent
- [[Prevent-XSS]] - Additional XSS prevention
- [[Sanitize-Input]] - Input sanitization
- [[What-is-OWASP]] - OWASP security guidelines
- [[What-is-CSRF]] - CSRF protection
- [[What-is-HTTPS]] - HTTPS and CSP
