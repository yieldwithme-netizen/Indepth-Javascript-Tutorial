# querySelector and querySelectorAll

## Definition

`querySelector()` and `querySelectorAll()` are methods that let you select DOM elements using **CSS selectors**. They are the most flexible and modern way to find elements in the DOM, supporting any CSS selector syntax you already know.

## Basic Syntax

```javascript
// Select the FIRST matching element
const element = document.querySelector(selector);

// Select ALL matching elements (returns NodeList)
const elements = document.querySelectorAll(selector);
```

## Selecting by Tag

```javascript
// Get the first <p> element
const firstParagraph = document.querySelector('p');
console.log(firstParagraph.textContent);

// Get all <p> elements
const allParagraphs = document.querySelectorAll('p');
console.log(allParagraphs.length); // e.g., 5
```

## Selecting by ID

```javascript
// Selects element with id="header"
const header = document.querySelector('#header');
```

## Selecting by Class

```javascript
// Get first element with class="card"
const firstCard = document.querySelector('.card');

// Get all elements with class="card"
const allCards = document.querySelectorAll('.card');
```

## Selecting with Complex Selectors

```javascript
// Select first element inside a container
const item = document.querySelector('.container .item');

// Select first list item inside a nav
const navItem = document.querySelector('nav ul li:first-child');

// Select all links inside the header
const headerLinks = document.querySelectorAll('header a');

// Select element with specific attribute
const input = document.querySelector('input[type="email"]');

// Select element with multiple classes
const element = document.querySelector('.btn.primary.active');
```

## Working with NodeList

```javascript
const items = document.querySelectorAll('.list-item');

// forEach — works directly on NodeList
items.forEach((item, index) => {
  console.log(`${index}: ${item.textContent}`);
});

// Convert to array for array methods
const itemsArray = [...items];
const filtered = itemsArray.filter(item => item.textContent.includes('important'));

// Convert using Array.from
const itemsArray2 = Array.from(items);
```

## Practical Examples

### Highlight Current Menu Item

```javascript
const menuLinks = document.querySelectorAll('nav a');
const currentPage = window.location.pathname;

menuLinks.forEach(link => {
  if (link.getAttribute('href') === currentPage) {
    link.classList.add('active');
  }
});
```

### Form Validation Setup

```javascript
const requiredFields = document.querySelectorAll('input[required]');

requiredFields.forEach(field => {
  field.addEventListener('blur', () => {
    if (!field.value.trim()) {
      field.classList.add('error');
    } else {
      field.classList.remove('error');
    }
  });
});
```

### Toggle All Checkboxes

```javascript
const selectAllCheckbox = document.querySelector('#select-all');
const checkboxes = document.querySelectorAll('.item-checkbox');

selectAllCheckbox.addEventListener('change', () => {
  checkboxes.forEach(cb => {
    cb.checked = selectAllCheckbox.checked;
  });
});
```

### Filter Items by Search

```javascript
const searchInput = document.querySelector('#search');
const items = document.querySelectorAll('.product');

searchInput.addEventListener('input', (e) => {
  const term = e.target.value.toLowerCase();
  items.forEach(item => {
    const text = item.textContent.toLowerCase();
    item.style.display = text.includes(term) ? 'block' : 'none';
  });
});
```

## querySelector vs getElementById

```javascript
// Both work, but querySelector is more flexible
const byId = document.getElementById('myId');
const bySelector = document.querySelector('#myId');

// querySelector can do more — but getElementById is slightly faster
```

## Common Use Cases

| Use Case | Code |
|----------|------|
| First matching element | `document.querySelector('.class')` |
| All matching elements | `document.querySelectorAll('.class')` |
| Nested selector | `document.querySelector('.parent .child')` |
| Attribute selector | `document.querySelector('[data-id="5"]')` |
| Pseudo-class selector | `document.querySelector('li:nth-child(2)')` |

## Common Mistakes to Avoid

1. **Forgetting it returns only the first match** — Use `querySelectorAll` for multiple
2. **Not converting NodeList to array** — NodeList doesn't have all array methods
3. **Selector errors are silent** — Invalid selectors throw DOMException

```javascript
// WRONG: Only gets the first card
const card = document.querySelector('.card');

// RIGHT: Gets all cards
const cards = document.querySelectorAll('.card');
```

## Related Topics

- [[What-is-GetById]]
- [[What-is-Traversal]]
- [[What-is-ClassList]]
- [[What-is-Style]]

## Quick Revision

| Method | Returns | Supports |
|--------|---------|----------|
| `querySelector()` | First matching element | Any CSS selector |
| `querySelectorAll()` | NodeList of all matches | Any CSS selector |
| `getElementById()` | Single element | ID only |
| `getElementsByClassName()` | HTMLCollection | Class only |
| `getElementsByTagName()` | HTMLCollection | Tag only |
