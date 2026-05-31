# What is HTTPS?

## Definition

HTTPS (HTTP Secure) is **HTTP encrypted with TLS/SSL** that encrypts data in transit, preventing eavesdropping, tampering, and man-in-the-middle attacks between client and server.

## HTTPS vs HTTP

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Encryption | None | TLS/SSL |
| Port | 80 | 443 |
| Security | Vulnerable | Secure |
| SEO | No boost | Ranking boost |
| Browser | Warning | Lock icon |

## Code Examples

### 1. Node.js HTTPS Server

```javascript
import https from 'https';
import fs from 'fs';

// Load SSL certificates
const options = {
  key: fs.readFileSync('private-key.pem'),
  cert: fs.readFileSync('certificate.pem')
};

// Create HTTPS server
https.createServer(options, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Secure connection established');
}).listen(443);

// Redirect HTTP to HTTPS
import http from 'http';

http.createServer((req, res) => {
  res.writeHead(301, {
    'Location': `https://${req.headers.host}${req.url}`
  });
  res.end();
}).listen(80);
```

### 2. Express with HTTPS

```javascript
import express from 'express';
import https from 'https';
import fs from 'fs';
import helmet from 'helmet';

const app = express();

// Security headers
app.use(helmet());

// HSTS header
app.use((req, res, next) => {
  res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
  next();
});

// HTTPS redirect middleware
function requireHTTPS(req, res, next) {
  if (req.headers['x-forwarded-proto'] !== 'https') {
    return res.redirect(301, `https://${req.hostname}${req.url}`);
  }
  next();
}

app.use(requireHTTPS);

// Server
const options = {
  key: fs.readFileSync('./certs/private-key.pem'),
  cert: fs.readFileSync('./certs/certificate.pem')
};

https.createServer(app).listen(443);
```

### 3. Let's Encrypt with Certbot

```bash
# Install certbot
sudo apt install certbot

# Get certificate
sudo certbot certonly --standalone -d example.com -d www.example.com

# Certificates stored at
# /etc/letsencrypt/live/example.com/fullchain.pem
# /etc/letsencrypt/live/example.com/privkey.pem

# Auto-renew
sudo certbot renew --dry-run
```

### 4. Nginx HTTPS Configuration

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    # HSTS
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    location / {
        proxy_pass http://localhost:3000;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}
```

### 5. Certificate Validation

```javascript
import https from 'https';
import tls from 'tls';

// Validate certificate
function checkCertificate(hostname) {
  return new Promise((resolve, reject) => {
    const socket = tls.connect(443, hostname, {
      rejectUnauthorized: true
    }, () => {
      const cert = socket.getPeerCertificate();
      socket.end();

      resolve({
        valid: socket.authorized,
        issuer: cert.issuer.O,
        subject: cert.subject.CN,
        validFrom: cert.valid_from,
        validTo: cert.valid_to
      });
    });

    socket.on('error', reject);
  });
}

// Usage
const certInfo = await checkCertificate('example.com');
console.log(certInfo);
```

### 6. Content Security with HTTPS

```javascript
// Enforce HTTPS in CSP
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', `
    default-src https:;
    script-src https:;
    style-src https: 'unsafe-inline';
    upgrade-insecure-requests
  `);
  next();
});

// Mixed content prevention
app.use((req, res, next) => {
  // Don't allow HTTP resources on HTTPS page
  res.setHeader('Content-Security-Policy', "upgrade-insecure-requests");
  next();
});
```

## Common Use Cases

```javascript
// API with HTTPS enforcement
app.use('/api', (req, res, next) => {
  if (!req.secure && process.env.NODE_ENV === 'production') {
    return res.redirect(301, `https://${req.headers.host}${req.url}`);
  }
  next();
});

// Secure cookie settings
res.cookie('session', token, {
  secure: true,      // HTTPS only
  httpOnly: true,
  sameSite: 'strict'
});

// Service worker with HTTPS
if ('serviceWorker' in navigator && location.protocol === 'https:') {
  navigator.serviceWorker.register('/sw.js');
}
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Mixed HTTP/HTTPS content | Security warnings |
| Expired certificates | Site inaccessible |
| Weak TLS versions | Vulnerable to attacks |
| Missing HSTS header | Protocol downgrade |
| Not redirecting HTTP | Users on insecure connection |
| Self-signed in production | Trust issues |
| Missing certificate chain | Validation fails |

## Quick Revision

- HTTPS encrypts data in transit using TLS/SSL
- Always use HTTPS in production
- Use Let's Encrypt for free certificates
- Enable HSTS to prevent protocol downgrade
- Redirect all HTTP traffic to HTTPS
- Validate certificates properly
- Use modern TLS versions (1.2, 1.3)
- Check for mixed content warnings
- HTTPS is required for many modern APIs

---

## Related Topics

- [[What-is-Authentication]] - Secure authentication requires HTTPS
- [[Store-Secrets]] - Secure secret storage
- [[What-is-CSP]] - Content Security Policy
- [[What-is-CSRF]] - CSRF protection
- [[What-is-Deployment]] - Deployment security
- [[What-is-OWASP]] - OWASP guidelines
