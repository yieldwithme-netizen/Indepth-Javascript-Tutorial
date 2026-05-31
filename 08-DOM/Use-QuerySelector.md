# How to Use querySelector

## Basic Syntax

```javascript
// Select first match
const element = document.querySelector("selector");

// Select all matches
const elements = document.querySelectorAll("selector");
```

## CSS Selectors

```javascript
// By ID
const header = document.querySelector("#header");

// By class
const box = document.querySelector(".box");

// By tag
const paragraph = document.querySelector("p");

// Attribute
const link = document.querySelector("a[href='https://example.com']");

// Nested
const item = document.querySelector(".list .item");

// Pseudo-class
const first = document.querySelector("li:first-child");
```

## QuerySelectorAll

```javascript
// All paragraphs
const paragraphs = document.querySelectorAll("p");

// Loop through
paragraphs.forEach(p => {
    console.log(p.textContent);
});

// Convert to array
const arr = Array.from(document.querySelectorAll("li"));
```

## querySelector vs getElementById

```javascript
// getElementById (faster, ID only)
const el = document.getElementById("my-id");

// querySelector (flexible, any selector)
const el = document.querySelector("#my-id");
```

## Quick Revision

- `querySelector()` - first match
- `querySelectorAll()` - all matches
- Uses CSS selectors
- Returns `NodeList` (forEach works)
- More flexible than `getElementById`

---

## Related Topics

- [[What-is-QuerySelector]] - [[What-is-QuerySelector|querySelector]] overview
- [[Use-QuerySelector]] - [[Use-QuerySelector|Using querySelector]]
- [[What-is-GetById]] - [[What-is-GetById|getElementById]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]] overview
- [[Select-Elements]] - [[Select-Elements|Selecting elements]]
