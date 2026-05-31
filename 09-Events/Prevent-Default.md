# Prevent Default

## Definition

`preventDefault()` is a method on the event object that stops the browser's default behavior for a specific event. For example, it prevents a form from submitting or a link from navigating.

## Syntax

```javascript
element.addEventListener('event', (event) => {
  event.preventDefault();
});
```

## Code Examples

### Prevent Form Submission

```javascript
const form = document.getElementById('myForm');

form.addEventListener('submit', (event) => {
  event.preventDefault();
  console.log('Form submission prevented!');
  // Process form data with JavaScript instead
});
```

### Prevent Link Navigation

```javascript
const link = document.getElementById('myLink');

link.addEventListener('click', (event) => {
  event.preventDefault();
  console.log('Link clicked but not navigated');
});
```

### Prevent Default on Multiple Events

```javascript
document.addEventListener('contextmenu', (event) => {
  event.preventDefault();
  console.log('Right-click menu disabled');
});

input.addEventListener('keydown', (event) => {
  if (event.key === 'Enter') {
    event.preventDefault();
    console.log('Enter key pressed but form not submitted');
  }
});
```

## Common Use Cases

1. **Form validation** - Validate data before submission
2. **Custom form handling** - Submit forms via AJAX instead
3. **Prevent navigation** - Keep users on the current page
4. **Disable context menu** - Block right-click menu
5. **Custom keyboard shortcuts** - Override default browser shortcuts

## Common Mistakes

```javascript
// Wrong: Missing event parameter
element.addEventListener('click', () => {
  event.preventDefault(); // ReferenceError
});

// Correct
element.addEventListener('click', (event) => {
  event.preventDefault();
});
```

## Related Topics

- [[Stop-Propagation]]
- [[Handle-Clicks]]
- [[Handle-Form]]
- [[Use-Event-Props]]

## Quick Revision

| Method | Purpose |
|--------|---------|
| `preventDefault()` | Stops browser default behavior |
| Use on | Forms, links, buttons, keyboard events |
| Common error | Forgetting to pass event parameter |
