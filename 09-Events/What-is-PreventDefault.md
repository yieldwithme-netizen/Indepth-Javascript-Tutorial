# What is preventDefault()?

## Definition

`preventDefault()` **stops the browser's default behavior** for an event.

## Examples

```javascript
// Prevent form submission
document.querySelector("form").addEventListener("submit", (e) => {
    e.preventDefault();
    console.log("Form not submitted!");
});

// Prevent link navigation
document.querySelector("a").addEventListener("click", (e) => {
    e.preventDefault();
    console.log("Link clicked but not navigated!");
});

// Prevent context menu
document.addEventListener("contextmenu", (e) => {
    e.preventDefault();
    console.log("Right-click disabled!");
});
```

## Common Use Cases

| Event | Default Behavior | When to Prevent |
|-------|------------------|-----------------|
| submit | Form submits to server | Validate first |
| click on `<a>` | Navigate to href | SPA routing |
| contextmenu | Show right-click menu | Custom menu |
| keydown | Type character | Custom shortcuts |

## Quick Revision

- `preventDefault()` stops browser default
- Use for: form validation, SPA routing, custom menus
- Must call on event object: `event.preventDefault()`
- Doesn't stop event propagation
- Only stops default browser action

---

## Related Topics

- [[Prevent-Default]] - Using preventDefault
- [[What-is-Event]] - Events
- [[Handle-Form]] - Form handling
- [[Stop-Propagation]] - stopPropagation()
