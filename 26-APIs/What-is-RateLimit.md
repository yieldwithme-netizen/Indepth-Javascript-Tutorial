# What is Rate Limiting

Rate limiting is a technique used to control the number of requests a client can make to an API within a specific time period. It protects servers from abuse, prevents overloading, and ensures fair usage among clients.

## Why Rate Limiting is Important

- **Prevents abuse and DDoS attacks**
- **Ensures fair resource allocation**
- **Reduces server load**
- **Controls costs** (especially for paid APIs)
- **Protects against brute force attacks**

## Common Rate Limiting Algorithms

### 1. Fixed Window Counter

```javascript
class FixedWindowRateLimiter {
  constructor(windowMs, maxRequests) {
    this.windowMs = windowMs;
    this.maxRequests = maxRequests;
    this.requestCounts = new Map();
  }

  isAllowed(clientId) {
    const now = Date.now();
    const windowStart = Math.floor(now / this.windowMs) * this.windowMs;

    if (!this.requestCounts.has(clientId)) {
      this.requestCounts.set(clientId, { count: 0, windowStart });
    }

    const clientData = this.requestCounts.get(clientId);

    if (windowStart > clientData.windowStart) {
      clientData.count = 0;
      clientData.windowStart = windowStart;
    }

    if (clientData.count >= this.maxRequests) {
      return false;
    }

    clientData.count++;
    return true;
  }
}

// Usage
const limiter = new FixedWindowRateLimiter(60000, 100); // 100 requests per minute

app.use((req, res, next) => {
  if (!limiter.isAllowed(req.ip)) {
    return res.status(429).json({ error: 'Rate limit exceeded' });
  }
  next();
});
```

### 2. Sliding Window Log

```javascript
class SlidingWindowRateLimiter {
  constructor(windowMs, maxRequests) {
    this.windowMs = windowMs;
    this.maxRequests = maxRequests;
    this.requestLogs = new Map();
  }

  isAllowed(clientId) {
    const now = Date.now();

    if (!this.requestLogs.has(clientId)) {
      this.requestLogs.set(clientId, []);
    }

    const logs = this.requestLogs.get(clientId);

    // Remove old entries outside the window
    while (logs.length > 0 && logs[0] <= now - this.windowMs) {
      logs.shift();
    }

    if (logs.length >= this.maxRequests) {
      return false;
    }

    logs.push(now);
    return true;
  }
}
```

### 3. Token Bucket

```javascript
class TokenBucket {
  constructor(capacity, refillRate, refillInterval) {
    this.capacity = capacity;
    this.tokens = capacity;
    this.refillRate = refillRate;
    this.refillInterval = refillInterval;
    this.lastRefill = Date.now();
  }

  consume() {
    this.refill();

    if (this.tokens >= 1) {
      this.tokens--;
      return true;
    }
    return false;
  }

  refill() {
    const now = Date.now();
    const elapsed = now - this.lastRefill;
    const tokensToAdd = Math.floor(elapsed / this.refillInterval) * this.refillRate;

    if (tokensToAdd > 0) {
      this.tokens = Math.min(this.capacity, this.tokens + tokensToAdd);
      this.lastRefill = now;
    }
  }
}
```

## Express.js Rate Limiting Middleware

```javascript
const rateLimit = require('express-rate-limit');

// Basic rate limiter
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests, please try again later.',
  standardHeaders: true,
  legacyHeaders: false,
});

// Stricter limiter for auth routes
const authLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 5, // 5 attempts per hour
  message: 'Too many login attempts, please try again after an hour',
});

app.use('/api/', apiLimiter);
app.use('/auth/login', authLimiter);
```

## Adding Rate Limit Headers

```javascript
function rateLimitHeaders(res, limit, remaining, resetTime) {
  res.set({
    'X-RateLimit-Limit': limit,
    'X-RateLimit-Remaining': remaining,
    'X-RateLimit-Reset': resetTime,
    'Retry-After': Math.ceil((resetTime - Date.now()) / 1000)
  });
}

// Client-side handling
fetch('/api/data', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
})
.then(response => {
  const limit = response.headers.get('X-RateLimit-Limit');
  const remaining = response.headers.get('X-RateLimit-Remaining');
  console.log(`Rate limit: ${remaining}/${limit}`);

  if (response.status === 429) {
    const retryAfter = response.headers.get('Retry-After');
    console.log(`Retry after ${retryAfter} seconds`);
  }
});
```

## Common Use Cases

- **API protection**: Preventing abuse of public APIs
- **Login protection**: Limiting login attempts
- **Web scraping prevention**: Blocking excessive crawling
- **Resource protection**: Limiting expensive operations
- **DDoS mitigation**: Reducing impact of distributed attacks

## Common Mistakes

1. **Not implementing rate limiting** - Leaving APIs unprotected
2. **Using only IP-based limiting** - Users behind NAT share IPs
3. **Not handling 429 responses** - Clients should gracefully handle rate limits
4. **Static limits for all endpoints** - Different endpoints need different limits
5. **Ignoring legitimate high-volume users** - Consider tiered limits

## Related Topics

- [[Implement-Auth]]
- [[What-is-MemoryLeak]]
- [[Store-Secrets]]
- [[What-is-Documentation]]

## Quick Revision

| Algorithm | Description | Best For |
|-----------|-------------|----------|
| Fixed Window | Counts requests in fixed time windows | Simple implementation |
| Sliding Window | Tracks request timestamps | More accurate limiting |
| Token Bucket | Tokens refilled at fixed rate | Burst tolerance |
| Leaky Bucket | Requests queued and processed at fixed rate | Smooth traffic |
