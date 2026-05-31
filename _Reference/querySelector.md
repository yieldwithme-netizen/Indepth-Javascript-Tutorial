# querySelector in JavaScript

## Definition

`querySelector()` and `querySelectorAll()` are DOM methods that allow you to select elements using **CSS selector syntax**. They provide a powerful and flexible way to find elements in the DOM, replacing older methods like `getElementById()` and `getElementsByClassName()`.

---

## Basic Usage

```javascript
// querySelector - returns first matching element
const firstParagraph = document.querySelector("p");
const header = document.querySelector("h1");
const submitBtn = document.querySelector('button[type="submit"]');

// querySelectorAll - returns all matching elements (NodeList)
const allParagraphs = document.querySelectorAll("p");
const allLinks = document.querySelectorAll("a.nav-link");

// Convert NodeList to array for more methods
const linksArray = Array.from(document.querySelectorAll("a"));
```

---

## CSS Selectors

```javascript
// Element selector
document.querySelector("div");

// Class selector
document.querySelector(".container");

// ID selector
document.querySelector("#main");

// Attribute selector
document.querySelector("[href]");
document.querySelector('[type="email"]');
document.querySelector("[data-id='123']");

// Pseudo-classes
document.querySelector("li:first-child");
document.querySelector("input:checked");
document.querySelector("a:hover");

// Complex selectors
document.querySelector("nav ul li a.active");
document.querySelector("form > input[type='text']");
```

---

## Combining Selectors

```javascript
// Descendant selector
const item = document.querySelector("div.container ul li");

// Child selector (direct only)
const directChild = document.querySelector("ul > li");

// Sibling selectors
const nextSibling = document.querySelector("h1 + p");
const anySibling = document.querySelector("h1 ~ p");

// Multiple selectors (comma-separated)
const elements = document.querySelectorAll("h1, h2, h3");

// :not() selector
const nonActive = document.querySelectorAll("li:not(.active)");
```

---

## Finding Elements Within Elements

```javascript
// Scope query to specific element
const form = document.querySelector("form");
const emailInput = form.querySelector('input[type="email"]');

// vs global query
const emailInput2 = document.querySelector('form input[type="email"]');

// Useful for dynamic content
const modal = document.querySelector("#modal");
const modalTitle = modal.querySelector(".title");
const modalClose = modal.querySelector(".close-btn");
```

---

## Common Use Cases

### Form Handling

```javascript
const form = document.querySelector("form");

// Get all form inputs
const inputs = form.querySelectorAll("input, textarea, select");

// Get specific input
const emailInput = form.querySelector('input[type="email"]');
const passwordInput = form.querySelector('input[name="password"]');

// Get form values
const formData = new FormData(form);
const data = Object.fromEntries(formData.entries());

// Validate before submit
form.addEventListener("submit", (e) => {
  const requiredFields = form.querySelectorAll("[required]");
  const isValid = Array.from(requiredFields).every(
    field => field.value.trim() !== ""
  );
  
  if (!isValid) {
    e.preventDefault();
    alert("Please fill all required fields");
  }
});
```

### Dynamic Content

```javascript
// Wait for content to load
document.addEventListener("DOMContentLoaded", () => {
  const elements = document.querySelectorAll(".dynamic-item");
  elements.forEach(el => {
    el.addEventListener("click", handleClick);
  });
});

// Re-query after DOM changes
function addItem() {
  const list = document.querySelector("#item-list");
  const newItem = document.createElement("li");
  newItem.textContent = "New Item";
  list.appendChild(newItem);
  
  // Re-query to include new item
  const allItems = document.querySelectorAll("#item-list li");
  console.log(`Total items: ${allItems.length}`);
}
```

### Navigation

```javascript
const currentItem = document.querySelector(".active");

// Get siblings
const prev = currentItem.previousElementSibling;
const next = currentItem.nextElementSibling;

// Get parent
const parent = currentItem.parentElement;
const closest = currentItem.closest(".container"); // Traverses up

// Get children
const children = currentItem.children;
const firstChild = currentItem.firstElementChild;
const lastChild = currentItem.lastElementChild;
```

### Event Delegation

```javascript
// Instead of adding listeners to each item
const list = document.querySelector("#list");

// Add single listener to parent
list.addEventListener("click", (e) => {
  const item = e.target.closest("li");
  if (item) {
    console.log("Clicked:", item.textContent);
  }
});
```

---

## Performance Tips

```javascript
// Cache selectors
const header = document.querySelector("header");
const nav = document.querySelector("nav");
const footer = document.querySelector("footer");

// Use specific selectors
// Bad: scans entire DOM
document.querySelectorAll("div");
// Better: more specific
document.querySelector(".sidebar > div");

// Batch operations
const elements = document.querySelectorAll(".item");
const fragment = document.createDocumentFragment();
elements.forEach(el => {
  const clone = el.cloneNode(true);
  fragment.appendChild(clone);
});

// Use requestAnimationFrame for updates
requestAnimationFrame(() => {
  const items = document.querySelectorAll(".item");
  items.forEach(item => {
    item.style.opacity = "1";
  });
});
```

---

## Common Mistakes

### Mistake 1: Assuming Single Element

```javascript
// querySelector returns first match only
const items = document.querySelectorAll(".item"); // NodeList
const item = document.querySelector(".item"); // Single element

// Wrong: trying to iterate single element
item.forEach(el => {}); // TypeError

// Correct: check if element exists
if (item) {
  item.addEventListener("click", handleClick);
}
```

### Mistake 2: Forgetting NodeList isn't an Array

```javascript
const items = document.querySelectorAll(".item");

// Wrong: array methods don't work
items.map(item => item.textContent); // TypeError
items.filter(item => item.classList.contains("active")); // TypeError

// Correct: convert to array first
const itemsArray = Array.from(items);
itemsArray.map(item => item.textContent);
itemsArray.filter(item => item.classList.contains("active"));

// Or use forEach (available on NodeList)
items.forEach(item => {
  console.log(item.textContent);
});
```

### Mistake 3: Querying Before DOM is Ready

```javascript
// Wrong: element may not exist yet
const element = document.querySelector("#dynamic"); // null

// Correct: wait for DOM
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector("#dynamic");
  // Now element exists
});

// Or use defer attribute in script tag
// <script src="app.js" defer></script>
```

### Mistake 4: Not Handling Null Returns

```javascript
// Wrong: crashes if element not found
const button = document.querySelector("#nonexistent");
button.addEventListener("click", handleClick); // TypeError

// Correct: check for null
const button = document.querySelector("#nonexistent");
if (button) {
  button.addEventListener("click", handleClick);
}

// Or use optional chaining
button?.addEventListener("click", handleClick);
```

---

## Comparison with Other Methods

```javascript
// getElementById - by ID (no # prefix)
document.getElementById("main");

// getElementsByClassName - returns live HTMLCollection
document.getElementsByClassName("item");

// getElementsByTagName - returns live HTMLCollection
document.getElementsByTagName("p");

// querySelector - CSS selectors, static NodeList
document.querySelector(".item");

// querySelectorAll - CSS selectors, static NodeList
document.querySelectorAll(".item");
```

---

## Quick Revision Summary

| Method | Returns | Live? | Selector |
|--------|---------|-------|----------|
| `querySelector()` | First element | No | CSS |
| `querySelectorAll()` | NodeList | No | CSS |
| `getElementById()` | Element | Yes | ID only |
| `getElementsByClassName()` | HTMLCollection | Yes | Class |
| `getElementsByTagName()` | HTMLCollection | Yes | Tag |

---

## Related Topics

- [[DOM-Manipulation]] - Working with DOM elements
- [[Add-Event-Listeners]] - Adding event handlers
- [[appendChild]] - Adding elements to DOM
- [[DOM]] - Document Object Model
- [[document]] - Document object
- [[window]] - Window object
- [[event]] - Event handling