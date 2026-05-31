# How to Store Secrets Safely

Secrets are sensitive data like API keys, passwords, and tokens that must be protected. Improper storage can lead to security breaches, data leaks, and unauthorized access.

## Environment Variables

### Node.js with dotenv

```javascript
// .env file (NEVER commit to git)
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=secret123
API_KEY=your-api-key-here
JWT_SECRET=your-jwt-secret

// .gitignore
.env
.env.local
.env.production

// Load environment variables
require('dotenv').config();

// Access variables
const dbPassword = process.env.DB_PASSWORD;
const apiKey = process.env.API_KEY;
```

### Using in different environments

```javascript
// config.js
const config = {
  development: {
    dbUrl: process.env.DEV_DB_URL || 'mongodb://localhost:27017/dev',
    apiKey: process.env.DEV_API_KEY,
  },
  production: {
    dbUrl: process.env.PROD_DB_URL,
    apiKey: process.env.PROD_API_KEY,
  },
};

const env = process.env.NODE_ENV || 'development';
module.exports = config[env];
```

## Client-Side Secrets (React/Vue)

```javascript
// ✅ CORRECT: Using environment variables
const API_KEY = process.env.REACT_APP_API_KEY;

// Create .env file
// REACT_APP_API_KEY=your-public-api-key

// Access in component
function App() {
  const apiKey = process.env.REACT_APP_API_KEY;
  // ...
}

// ✅ CORRECT: Using a proxy server
// Frontend calls your backend, backend calls external API
// proxy.js
const express = require('express');
const axios = require('axios');
const app = express();

app.get('/api/data', async (req, res) => {
  const response = await axios.get('https://api.external.com/data', {
    headers: {
      'Authorization': `Bearer ${process.env.EXTERNAL_API_KEY}`
    }
  });
  res.json(response.data);
});
```

## Secret Management Services

```javascript
// AWS Secrets Manager
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getSecret(secretName) {
  const data = await secretsManager.getSecretValue({ SecretId: secretName }).promise();
  return JSON.parse(data.SecretString);
}

// Usage
const dbCredentials = await getSecret('production/database');

// HashiCorp Vault
const vault = require('node-vault')({
  apiVersion: 'v1',
  endpoint: 'http://127.0.0.1:8200',
  token: process.env.VAULT_TOKEN
});

async function getVaultSecret(path) {
  const { data } = await vault.read(path);
  return data;
}
```

## Secure Configuration Object

```javascript
// config/secure.js
class SecureConfig {
  constructor() {
    this.secrets = new Map();
  }

  async loadSecrets() {
    // Load from environment
    const secretKeys = [
      'DB_PASSWORD',
      'API_KEY',
      'JWT_SECRET',
      'ENCRYPTION_KEY'
    ];

    for (const key of secretKeys) {
      const value = process.env[key];
      if (!value) {
        throw new Error(`Missing required secret: ${key}`);
      }
      this.secrets.set(key, value);
    }
  }

  get(key) {
    if (!this.secrets.has(key)) {
      throw new Error(`Secret not found: ${key}`);
    }
    return this.secrets.get(key);
  }

  // Prevent logging of secrets
  toJSON() {
    return { '[REDACTED]': 'Secrets cannot be serialized' };
  }
}

module.exports = new SecureConfig();
```

## Encryption at Rest

```javascript
const crypto = require('crypto');

class SecretEncryption {
  constructor() {
    this.algorithm = 'aes-256-gcm';
    this.key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');
  }

  encrypt(text) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipher(this.algorithm, this.key);

    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');

    const authTag = cipher.getAuthTag();

    return {
      iv: iv.toString('hex'),
      encryptedData: encrypted,
      authTag: authTag.toString('hex')
    };
  }

  decrypt(encryptedObj) {
    const decipher = crypto.createDecipher(this.algorithm, this.key);

    decipher.setAuthTag(Buffer.from(encryptedObj.authTag, 'hex'));

    let decrypted = decipher.update(encryptedObj.encryptedData, 'hex', 'utf8');
    decrypted += decipher.final('utf8');

    return decrypted;
  }
}

// Usage
const encryptor = new SecretEncryption();
const encrypted = encryptor.encrypt('my-secret-data');
const decrypted = encryptor.decrypt(encrypted);
```

## Common Use Cases

- **Database credentials**: Connection strings and passwords
- **API keys**: Third-party service access
- **JWT secrets**: Token signing keys
- **OAuth client secrets**: Third-party authentication
- **Encryption keys**: Data encryption keys
- **SSH keys**: Server access credentials

## Common Mistakes

1. **Committing .env files** - Always add to .gitignore
2. **Hardcoding secrets in source code** - Use environment variables
3. **Logging secrets** - Never log sensitive data
4. **Sharing secrets via email/Slack** - Use secure secret management
5. **Using weak encryption keys** - Use strong, random keys
6. **No secret rotation** - Regularly rotate secrets

## Related Topics

- [[Implement-Auth]]
- [[What-is-OWASP]]
- [[What-is-RateLimit]]
- [[What-is-Documentation]]

## Quick Revision

| Method | Best For |
|--------|----------|
| Environment Variables | Simple deployments |
| Secret Manager Services | Production, cloud |
| Encrypted Files | On-premise |
| Key Vault | Enterprise |
| Proxy Server | Client-side apps |

**Security Checklist:**
- [ ] Secrets in .gitignore
- [ ] No hardcoded credentials
- [ ] Using environment variables
- [ ] Secrets encrypted at rest
- [ ] Regular secret rotation
- [ ] Access logging enabled
