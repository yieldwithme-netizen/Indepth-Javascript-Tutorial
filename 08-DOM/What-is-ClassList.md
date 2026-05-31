# classList API

## Definition

`classList` is a property that returns a DOMTokenList of the CSS classes applied to an element. It provides methods to add, remove, toggle, and check classes without directly manipulating the `className` string.

## Basic Syntax

```javascript
// Get classList
const classes = element.classList;

// Add a class
element.classList.add('active');

// Remove a class
element.classList.remove('inactive');

// Toggle a class
element.classList.toggle('visible');

// Check if class exists
element.classList.contains('active');
```

## Adding Classes

```javascript
const button = document.getElementById('submit-btn');

// Add single class
button.classList.add('loading');

// Add multiple classes
button.classList.add('loading', 'disabled', 'opacity-50');

// Safe to call multiple times — no duplicates
button.classList.add('loading');
button.classList.add('loading'); // No error, still one 'loading' class
```

## Removing Classes

```javascript
const button = document.getElementById('submit-btn');

// Remove single class
button.classList.remove('loading');

// Remove multiple classes
button.classList.remove('loading', 'disabled');

// Removing a non-existent class — no error
button.classList.remove('nonexistent');
```

## Toggling Classes

```javascript
const menu = document.getElementById('menu');

// Toggle — adds if missing, removes if present
menu.classList.toggle('open');

// Toggle with force parameter
menu.classList.toggle('open', true);   // Always adds
menu.classList.toggle('open', false);  // Always removes

// Returns true if class was added, false if removed
const wasAdded = menu.classList.toggle('active');
console.log(wasAdded); // true or false
```

## Checking for Classes

```javascript
const element = document.getElementById('card');

// Returns true/false
if (element.classList.contains('highlighted')) {
  console.log('Element is highlighted');
}

// Check for multiple classes
const hasClasses = element.classList.contains('card') &&
                   element.classList.contains('active');

// Check length
console.log(element.classList.length); // Number of classes
```

## Replacing Classes

```javascript
const element = document.getElementById('status');

// Replace one class with another
element.classList.replace('old-class', 'new-class');

// If 'old-class' doesn't exist, nothing happens
element.classList.replace('missing', 'new-class');
```

## Practical Examples

### Dark Mode Toggle

```javascript
const toggleBtn = document.getElementById('theme-toggle');
const body = document.body;

toggleBtn.addEventListener('click', () => {
  body.classList.toggle('dark-mode');

  const isDark = body.classList.contains('dark-mode');
  toggleBtn.textContent = isDark ? 'Light Mode' : 'Dark Mode';
});
```

### Active Navigation Link

```javascript
const navLinks = document.querySelectorAll('nav a');
const currentPage = window.location.pathname;

navLinks.forEach(link => {
  if (link.getAttribute('href') === currentPage) {
    link.classList.add('active');
  }
});
```

### Accordion Component

```javascript
const accordions = document.querySelectorAll('.accordion-header');

accordions.forEach(header => {
  header.addEventListener('click', () => {
    const content = header.nextElementSibling;
    const isOpen = header.classList.contains('open');

    // Close all
    accordions.forEach(h => {
      h.classList.remove('open');
      h.nextElementSibling.classList.remove('show');
    });

    // Toggle clicked
    if (!isOpen) {
      header.classList.add('open');
      content.classList.add('show');
    }
  });
});
```

### Toast Notifications

```javascript
function showToast(message, type = 'info') {
  const toast = document.createElement('div');
  toast.className = `toast toast-${type}`;
  toast.textContent = message;

  document.getElementById('toast-container').appendChild(toast);

  // Trigger animation
  requestAnimationFrame(() => {
    toast.classList.add('show');
  });

  // Remove after delay
  setTimeout(() => {
    toast.classList.remove('show');
    setTimeout(() => toast.remove(), 300);
  }, 3000);
}
```

### Form Validation States

```javascript
const form = document.getElementById('contact-form');
const inputs = form.querySelectorAll('input[required]');

inputs.forEach(input => {
  input.addEventListener('blur', () => {
    if (input.value.trim() === '') {
      input.classList.add('error');
      input.classList.remove('valid');
    } else {
      input.classList.remove('error');
      input.classList.add('valid');
    }
  });

  input.addEventListener('input', () => {
    input.classList.remove('error');
  });
});
```

## classList vs className

```javascript
const element = document.getElementById('box');

// className — returns/sets the full string
console.log(element.className); // "box large red"
element.className = 'box small blue'; // Replaces all

// classList — returns DOMTokenList with methods
console.log(element.classList); // ['box', 'large', 'red']
element.classList.add('green'); // Adds, doesn't replace
```

## Common Use Cases

| Use Case | Method |
|----------|--------|
| Add class | `el.classList.add('class')` |
| Remove class | `el.classList.remove('class')` |
| Toggle class | `el.classList.toggle('class')` |
| Check class | `el.classList.contains('class')` |
| Replace class | `el.classList.replace('old', 'new')` |
| Get count | `el.classList.length` |

## Common Mistakes to Avoid

1. **Using className instead of classList** — classList is safer and more flexible
2. **Forgetting toggle returns value** — Can be used in conditionals
3. **Not chaining** — Methods return void, cannot chain

```javascript
// WRONG: Replaces all classes
element.className = 'active';

// RIGHT: Adds to existing
element.classList.add('active');

// WRONG: Chaining doesn't work
element.classList.add('a').add('b'); // Error

// RIGHT: Separate calls
element.classList.add('a');
element.classList.add('b');
```

## Related Topics

- [[What-is-Style]]
- [[What-is-QuerySelector]]
- [[What-is-GetById]]
- [[What-is-InnerHTML]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `add()` | Add class(es) |
| `remove()` | Remove class(es) |
| `toggle()` | Toggle class |
| `contains()` | Check if class exists |
| `replace()` | Replace one class with another |
| `length` | Number of classes |
