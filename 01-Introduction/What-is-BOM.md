# What is BOM (Browser Object Model)?

## Definition

BOM is a **collection of browser objects** that let [[JavaScript]] interact with the browser itself (not the page content).

## BOM vs [[DOM]]

| BOM | [[DOM]] |
|-----|-----|
| Browser window | Page content |
| `[[window]]`, `navigator`, `location` | `[[document]]`, elements |
| No standard | W3C standard |

## BOM Objects

### [[window]] Object

```javascript
// Window properties
window.innerWidth;      // Browser width
window.innerHeight;     // Browser height
window.outerWidth;      // Browser outer width
window.outerHeight;     // Browser outer height
window.scrollX;         // Horizontal scroll
window.scrollY;         // Vertical scroll

// Window methods
window.alert("Hello");
window.confirm("OK?");
window.prompt("Enter name:");
window.open("https://google.com");
window.close();
```

### navigator Object

```javascript
navigator.userAgent;        // Browser info
navigator.platform;         // OS platform
navigator.language;         // Browser language
navigator.onLine;           // Online status
navigator.cookieEnabled;    // Cookies enabled?
navigator.geolocation;      // Location API
```

### location Object

```javascript
location.href;          // Full URL
location.protocol;      // http/https
location.host;          // domain.com
location.pathname;      // /page
location.hash;          // #section
location.search;        // ?param=value

// Methods
location.assign("url"); // Load new page
location.reload();      // Refresh page
location.replace("url"); // Replace current
```

### history Object

```javascript
history.length;             // Number of entries
history.back();             // Go back
history.forward();          // Go forward
history.go(-2);             // Go back 2 pages
```

### screen Object

```javascript
screen.width;           // Screen width
screen.height;          // Screen height
screen.availWidth;      // Available width
screen.availHeight;     // Available height
screen.colorDepth;      // Color depth
```

## Practical Examples

```javascript
// Redirect if mobile
if (navigator.userAgent.match(/Mobile/)) {
    window.location.href = "https://m.example.com";
}

// Show user's IP
fetch("https://api.ipify.org?format=json")
    .then(res => res.json())
    .then(data => console.log(data.ip));

// Copy to clipboard
navigator.clipboard.writeText("Hello");

// Fullscreen
document.documentElement.requestFullscreen();
```

## Quick Revision

- BOM = browser interaction (not page content)
- Main object: `[[window]]`
- `navigator` = browser info
- `location` = URL control
- `history` = navigation
- `screen` = display info

---

## Related Topics

- [[What-is-DOM]] - Page content manipulation
- [[What-is-JavaScript]] - JS basics
- [[Use-BOM-Functions]] - BOM methods
- [[Handle-Clicks]] - [[event]] handling