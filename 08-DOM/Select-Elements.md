# How to Select DOM Elements

## Definition

The Document Object Model (DOM) represents the structure of an HTML document. JavaScript provides several methods to select and manipulate DOM elements.

**Common Selection Methods:**
- `getElementById()` - Select by ID
- `getElementsByClassName()` - Select by class name
- `getElementsByTagName()` - Select by tag name
- `querySelector()` - Select using CSS selector
- `querySelectorAll()` - Select all using CSS selector

## Code Examples

### getElementById()
```javascript
// Select single element by ID
const header = document.getElementById("header");
const mainContent = document.getElementById("main-content");

// Returns null if not found
const notFound = document.getElementById("nonexistent");
console.log(notFound);  // null
```

### getElementsByClassName()
```javascript
// Select elements by class name (returns HTMLCollection)
const items = document.getElementsByClassName("list-item");
console.log(items.length);  // Number of matching elements

// Convert to array for easier manipulation
const itemsArray = Array.from(items);
itemsArray.forEach(item => {
    console.log(item.textContent);
});
```

### getElementsByTagName()
```javascript
// Select elements by tag name (returns HTMLCollection)
const paragraphs = document.getElementsByTagName("p");
const divs = document.getElementsByTagName("div");

// Loop through collection
for (let i = 0; i < paragraphs.length; i++) {
    console.log(paragraphs[i].textContent);
}
```

### querySelector()
```javascript
// Select first matching element using CSS selector
const firstParagraph = document.querySelector("p");
const header = document.querySelector("#header");
const firstListItem = document.querySelector(".list-item");
const navLink = document.querySelector("nav a.active");

// Complex selectors
const submitBtn = document.querySelector("form button[type='submit']");
```

### querySelectorAll()
```javascript
// Select all matching elements (returns NodeList)
const allParagraphs = document.querySelectorAll("p");
const allListItems = document.querySelectorAll("ul li");

// Convert to array for array methods
const paragraphs = Array.from(document.querySelectorAll("p"));
paragraphs.forEach(p => {
    console.log(p.textContent);
});

// Use Array.from with map
const texts = Array.from(allParagraphs).map(p => p.textContent);
```

### Practical Examples

#### Select Form Elements
```javascript
// Get all form inputs
const inputs = document.querySelectorAll("input, textarea, select");

// Get specific input types
const textInputs = document.querySelectorAll("input[type='text']");
const checkboxes = document.querySelectorAll("input[type='checkbox']");

// Get form by name
const form = document.querySelector("form[name='registration']");
```

#### Navigate DOM Tree
```javascript
const element = document.querySelector(".child");

// Parent elements
console.log(element.parentElement);
console.log(element.closest(".parent"));

// Child elements
console.log(element.children);
console.log(element.firstElementChild);
console.log(element.lastElementChild);

// Sibling elements
console.log(element.nextElementSibling);
console.log(element.previousElementSibling);
```

#### Dynamic Content Selection
```javascript
// Select after content is loaded
document.addEventListener("DOMContentLoaded", () => {
    const elements = document.querySelectorAll(".dynamic-content");
    console.log(elements);
});

// MutationObserver for dynamic content
const observer = new MutationObserver(mutations => {
    mutations.forEach(mutation => {
        if (mutation.type === "childList") {
            const newElements = document.querySelectorAll(".new-item");
            console.log("New items:", newElements);
        }
    });
});

observer.observe(document.body, { childList: true, subtree: true });
```

#### Filter Elements
```javascript
const allItems = document.querySelectorAll(".list-item");

// Filter visible items
const visibleItems = Array.from(allItems).filter(
    item => item.offsetParent !== null
);

// Filter by text content
const itemsWithText = Array.from(allItems).filter(
    item => item.textContent.includes("Important")
);

// Filter by attribute
const requiredInputs = document.querySelectorAll("input[required]");
```

## Common Use Cases

1. **Dynamic content manipulation**
2. **Form handling and validation**
3. **Interactive UI components**
4. **Data binding and rendering**
5. **Event handling on elements**

## Common Mistakes

1. **Confusing HTMLCollection and NodeList**: HTMLCollection is live; NodeList may not be
2. **Not checking for null**: Elements may not exist
3. **Using outdated methods**: Prefer `querySelector` over `getElementById`
4. **Performance issues**: Too many DOM queries in loops

## Related Topics

- [[Use-GetById]]
- [[Change-HTML]]
- [[Add-Event-Listeners]]
- [[DOM-Manipulation]]

## Quick Revision Summary

| Method | Returns | Use Case |
|--------|---------|----------|
| `getElementById()` | Single element | Select by ID |
| `getElementsByClassName()` | HTMLCollection | Select by class |
| `getElementsByTagName()` | HTMLCollection | Select by tag |
| `querySelector()` | Single element | CSS selector (first match) |
| `querySelectorAll()` | NodeList | CSS selector (all matches) |
