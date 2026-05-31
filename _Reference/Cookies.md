# Cookies

Cookies are small pieces of data stored on the user's browser. They are primarily used for maintaining state, remembering user preferences, and tracking user sessions across web applications.

## What Are Cookies?

Cookies are HTTP cookies - small text files that websites store on a user's computer. They are sent back to the server with each HTTP request, allowing the server to recognize returning users.

## Creating Cookies with JavaScript

### Setting Cookies

```javascript
// Basic cookie
document.cookie = "username=JohnDoe";

// With expiration date
document.cookie = "username=JohnDoe; expires=Fri, 31 Dec 2026 23:59:59 GMT";

// With path
document.cookie = "username=JohnDoe; path=/";

// With domain
document.cookie = "username=JohnDoe; domain=example.com";

// With Secure flag (HTTPS only)
document.cookie = "username=JohnDoe; secure";

// With SameSite attribute
document.cookie = "username=JohnDoe; SameSite=Strict";
```

### Reading Cookies

```javascript
// Read all cookies
console.log(document.cookie);

// Parse cookies into an object
function getCookies() {
  const cookies = {};
  document.cookie.split(';').forEach(cookie => {
    const [name, value] = cookie.trim().split('=');
    cookies[name] = decodeURIComponent(value);
  });
  return cookies;
}

const allCookies = getCookies();
console.log(allCookies.username); // "JohnDoe"
```

### Deleting Cookies

```javascript
// Set expiration to past date
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
```

## Cookie Utility Functions

```javascript
class CookieManager {
  static set(name, value, days = 7) {
    const expires = new Date();
    expires.setTime(expires.getTime() + days * 24 * 60 * 60 * 1000);
    document.cookie = `${name}=${encodeURIComponent(value)}; expires=${expires.toUTCString()}; path=/`;
  }

  static get(name) {
    const nameEQ = name + '=';
    const cookies = document.cookie.split(';');
    for (let cookie of cookies) {
      cookie = cookie.trim();
      if (cookie.indexOf(nameEQ) === 0) {
        return decodeURIComponent(cookie.substring(nameEQ.length));
      }
    }
    return null;
  }

  static remove(name) {
    this.set(name, '', -1);
  }

  static exists(name) {
    return this.get(name) !== null;
  }
}

// Usage
CookieManager.set('theme', 'dark', 30);
const theme = CookieManager.get('theme');
CookieManager.remove('theme');
```

## Cookie Options Explained

| Option | Description | Example |
|--------|-------------|---------|
| `expires` | Date when cookie expires | `expires=Fri, 31 Dec 2026` |
| `max-age` | Seconds until expiration | `max-age=86400` (1 day) |
| `path` | URL path cookie applies to | `path=/` |
| `domain` | Domain cookie applies to | `domain=example.com` |
| `secure` | Only send over HTTPS | `secure` |
| `SameSite` | CSRF protection | `SameSite=Strict` |

## Common Use Cases

- **Session management**: Keep users logged in
- **User preferences**: Remember settings (theme, language)
- **Shopping carts**: Store items before checkout
- **Analytics**: Track user behavior
- **Authentication tokens**: Store JWT or session IDs

## Common Mistakes

1. **Not encoding values** - Use `encodeURIComponent()` for special characters
2. **Ignoring size limits** - Max 4KB per cookie
3. **Overusing cookies** - Store only essential data
4. **Not setting SameSite** - Vulnerable to CSRF attacks
5. **Storing sensitive data** - Cookies are visible in browser

## Security Considerations

```javascript
// Secure cookie practices
document.cookie = "token=abc123; secure; SameSite=Strict; HttpOnly";
// Note: HttpOnly must be set by server, not JavaScript
```

## Related Topics

- [[Local-Storage]]
- [[Session-Storage]]
- [[Authentication]]
- [[HTTP-Requests]]
- [[Security]]

## Quick Revision

| Action | Method |
|--------|--------|
| Set cookie | `document.cookie = "name=value"` |
| Read cookie | `document.cookie` |
| Delete cookie | Set expiration to past date |
| Max size | 4KB per cookie |
| Security | Use `secure` and `SameSite` |

Cookies remain essential for web development, though modern alternatives like localStorage offer more storage capacity for client-side data.