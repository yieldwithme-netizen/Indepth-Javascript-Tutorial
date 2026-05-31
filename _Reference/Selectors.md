# Selectors

## Definition

Selectors are **patterns used to select and manipulate DOM elements**.

## CSS Selectors

```javascript
// By ID
document.querySelector("#header");
document.getElementById("header");

// By class
document.querySelector(".box");
document.getElementsByClassName("box");

// By tag
document.querySelector("p");
document.getElementsByTagName("p");

// Attribute
document.querySelector("a[href='https://example.com']");

// Nested
document.querySelector(".list .item");

// Pseudo-class
document.querySelector("li:first-child");
```

## Advanced Selectors

```javascript
// Multiple selectors
document.querySelectorAll("h1, h2, h3");

// Child selector
document.querySelectorAll("ul > li");

// Sibling selector
document.querySelectorAll("h1 + p");

// Attribute contains
document.querySelector("input[type='text']");

// Not selector
document.querySelectorAll("li:not(.disabled)");
```

## Performance Tips

```javascript
// ❌ Slow: querySelectorAll
document.querySelectorAll(".item");

// ✅ Fast: getElementById
document.getElementById("item");

// ✅ Fast: Direct reference
const el = document.querySelector("#specific-id");
```

## Quick Revision

- `querySelector()` - first match
- `querySelectorAll()` - all matches
- `getElementById()` - by ID (fastest)
- CSS selectors for complex patterns
- Cache selectors for performance

---

## Related Topics

- [[What-is-QuerySelector]] - [[What-is-QuerySelector|querySelector]]
- [[Use-QuerySelector]] - [[Use-QuerySelector|Using querySelector]]
- [[What-is-GetById]] - [[What-is-GetById|getElementById]]
- [[Select-Elements]] - [[Select-Elements|Selecting elements]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
