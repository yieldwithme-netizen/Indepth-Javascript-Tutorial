# Environment Variables

## Definition

Environment variables are key-value pairs used to store configuration settings and secrets outside of your codebase. They allow you to configure applications for different environments (development, staging, production) without hardcoding sensitive information like API keys, database credentials, or feature flags.

## Node.js Environment Variables

### 1. process.env

```javascript
// Access environment variables
console.log(process.env.NODE_ENV); // "development"
console.log(process.env.PORT); // "3000"
console.log(process.env.DATABASE_URL); // "mongodb://localhost:27017/mydb"
```

### 2. Setting Environment Variables

```bash
# In terminal (Linux/Mac)
export PORT=3000
export NODE_ENV=development

# In terminal (Windows)
set PORT=3000
set NODE_ENV=development

# Run script with env vars
PORT=3000 node app.js
```

### 3. .env Files

```bash
# .env file
PORT=3000
NODE_ENV=development
DATABASE_URL=mongodb://localhost:27017/mydb
API_KEY=your-api-key-here
SECRET_KEY=your-secret-key
```

### 4. Using dotenv Package

```javascript
// Install: npm install dotenv
require('dotenv').config();

// Now access variables
const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;
const apiKey = process.env.API_KEY;
```

### 5. Advanced dotenv Usage

```javascript
// Load specific env file
require('dotenv').config({ path: './config/.env.production' });

// Parse .env string directly
require('dotenv').config({
  parsed: {
    PORT: '3000',
    NODE_ENV: 'production'
  }
});
```

## Browser Environment Variables

### 1. Runtime Configuration

```javascript
// config.js for browser
const config = {
  apiUrl: process.env.REACT_APP_API_URL || 'http://localhost:3001',
  appName: process.env.REACT_APP_APP_NAME || 'My App',
  debug: process.env.REACT_APP_DEBUG === 'true'
};

export default config;
```

### 2. React Environment Variables

```bash
# .env
REACT_APP_API_URL=https://api.example.com
REACT_APP_TITLE=My React App

# Access in React components
console.log(process.env.REACT_APP_API_URL);
```

### 3. Vite Environment Variables

```bash
# .env
VITE_API_URL=https://api.example.com
VITE_APP_TITLE=My Vite App

// Access in code
console.log(import.meta.env.VITE_API_URL);
```

### 4. Webpack Environment Variables

```javascript
// webpack.config.js
const webpack = require('webpack');

module.exports = {
  plugins: [
    new webpack.DefinePlugin({
      'process.env.API_URL': JSON.stringify(process.env.API_URL),
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
    })
  ]
};
```

## Common Use Cases

### 1. Database Configuration

```javascript
// config/database.js
require('dotenv').config();

const dbConfig = {
  host: process.env.DB_HOST || 'localhost',
  port: parseInt(process.env.DB_PORT) || 5432,
  database: process.env.DB_NAME || 'myapp',
  username: process.env.DB_USER || 'root',
  password: process.env.DB_PASSWORD,
  ssl: process.env.DB_SSL === 'true'
};

module.exports = dbConfig;
```

### 2. API Configuration

```javascript
// config/api.js
const apiConfig = {
  baseUrl: process.env.API_BASE_URL || 'http://localhost:3001',
  timeout: parseInt(process.env.API_TIMEOUT) || 5000,
  apiKey: process.env.API_KEY,
  version: process.env.API_VERSION || 'v1'
};

export default apiConfig;
```

### 3. Feature Flags

```javascript
// features.js
const features = {
  darkMode: process.env.FEATURE_DARK_MODE === 'true',
  betaFeatures: process.env.FEATURE_BETA === 'true',
  maintenanceMode: process.env.MAINTENANCE_MODE === 'true'
};

export default features;

// Usage
if (features.darkMode) {
  enableDarkMode();
}
```

### 4. Logging Configuration

```javascript
// config/logger.js
const loggerConfig = {
  level: process.env.LOG_LEVEL || 'info',
  format: process.env.LOG_FORMAT || 'json',
  outputs: {
    console: process.env.LOG_CONSOLE !== 'false',
    file: process.env.LOG_FILE_PATH || './logs/app.log'
  }
};

export default loggerConfig;
```

### 5. Authentication Secrets

```javascript
// config/auth.js
const authConfig = {
  jwtSecret: process.env.JWT_SECRET,
  jwtExpiresIn: process.env.JWT_EXPIRES_IN || '7d',
  bcryptRounds: parseInt(process.env.BCRYPT_ROUNDS) || 10,
  sessionSecret: process.env.SESSION_SECRET
};

export default authConfig;
```

### 6. Environment-Specific Settings

```javascript
// config/environment.js
const env = process.env.NODE_ENV || 'development';

const configs = {
  development: {
    port: 3000,
    db: 'mongodb://localhost:27017/dev',
    debug: true
  },
  staging: {
    port: 3000,
    db: process.env.DATABASE_URL,
    debug: true
  },
  production: {
    port: process.env.PORT || 8080,
    db: process.env.DATABASE_URL,
    debug: false
  }
};

export default configs[env];
```

## .env File Structure

```bash
# .env.example (committed to git)
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=root
DB_PASSWORD=

# API
API_KEY=
API_SECRET=

# Authentication
JWT_SECRET=
SESSION_SECRET=

# Services
REDIS_URL=
AWS_ACCESS_KEY=
AWS_SECRET_KEY=

# Feature Flags
FEATURE_DARK_MODE=true
FEATURE_BETA=false
```

```bash
# .env.local (not committed)
DB_PASSWORD=secretpassword
API_KEY=abc123
JWT_SECRET=my-secret-key
```

## Managing Environment Variables

### 1. Validation

```javascript
// config/validate.js
const requiredEnvVars = [
  'DATABASE_URL',
  'JWT_SECRET',
  'API_KEY'
];

const missingVars = requiredEnvVars.filter(varName => !process.env[varName]);

if (missingVars.length > 0) {
  console.error('Missing required environment variables:');
  missingVars.forEach(varName => {
    console.error(`  - ${varName}`);
  });
  process.exit(1);
}
```

### 2. Type Conversion

```javascript
// utils/env.js
export const getEnv = (key, defaultValue = undefined) => {
  return process.env[key] || defaultValue;
};

export const getEnvNumber = (key, defaultValue = 0) => {
  const value = process.env[key];
  return value ? parseInt(value, 10) : defaultValue;
};

export const getEnvBoolean = (key, defaultValue = false) => {
  const value = process.env[key];
  return value ? value.toLowerCase() === 'true' : defaultValue;
};

// Usage
const port = getEnvNumber('PORT', 3000);
const debug = getEnvBoolean('DEBUG', false);
```

### 3. Multi-Environment Setup

```bash
# .env.development
NODE_ENV=development
DEBUG=true
API_URL=http://localhost:3001

# .env.staging
NODE_ENV=staging
DEBUG=true
API_URL=https://staging-api.example.com

# .env.production
NODE_ENV=production
DEBUG=false
API_URL=https://api.example.com
```

## Common Mistakes

### 1. Committing Secrets to Git

```bash
# WRONG: .env file in git
git add .env  # Never do this!

# RIGHT: Add to .gitignore
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
echo ".env.*.local" >> .gitignore
```

### 2. Hardcoding Default Values

```javascript
// WRONG: Hardcoded secret
const apiKey = process.env.API_KEY || 'hardcoded-key-123';

// RIGHT: Use placeholder or throw error
const apiKey = process.env.API_KEY;
if (!apiKey) {
  throw new Error('API_KEY environment variable is required');
}
```

### 3. Not Validating Input

```javascript
// WRONG: Trusting env vars
const port = process.env.PORT;

// RIGHT: Validate and convert
const port = parseInt(process.env.PORT, 10);
if (isNaN(port) || port < 1 || port > 65535) {
  throw new Error('Invalid PORT environment variable');
}
```

### 4. Exposing Secrets in Logs

```javascript
// WRONG: Logging entire env
console.log(process.env);

// RIGHT: Log specific, non-sensitive values
console.log('App started on port:', process.env.PORT);
console.log('Environment:', process.env.NODE_ENV);
```

## Quick Revision Summary

- Use `process.env` in Node.js to access environment variables
- Use `.env` files with `dotenv` package for local development
- Prefix browser vars with `REACT_APP_` or `VITE_` as needed
- Never commit `.env` files with secrets to version control
- Validate and type-check environment variables
- Use different `.env` files for different environments
- Document required variables in `.env.example`

## Related Topics

- [[Node.js-Configuration]] - Node.js config patterns
- [[Security-Best-Practices]] - Protecting secrets
- [[Deployment]] - Production deployment tips
- [[Docker-Environment]] - Docker environment variables
- [[CI-CD-Variables]] - CI/CD pipeline variables
- [[Config-Patterns]] - Configuration management
