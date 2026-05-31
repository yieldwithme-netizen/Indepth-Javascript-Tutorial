# DOM Manipulation

## Definition

DOM manipulation **changes HTML/CSS** with JavaScript.

## Examples

```javascript
// Select
const el = document.querySelector('.box');

// Modify
el.textContent = "New text";
el.style.color = "red";
el.classList.add("active");

// Create
const div = document.createElement("div");
document.body.appendChild(div);

// Remove
el.remove();
```

## Quick Revision

- Select: querySelector, getElementById
- Modify: textContent, style, classList
- Create: createElement, appendChild
- Remove: remove()

---

## Related Topics

- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[DOM Manipulation]] - [[DOM Manipulation|DOM manipulation]]
- [[What-is-Traversal]] - [[What-is-Traversal|DOM traversal]]
- [[What-is-DOM-Ready]] - [[What-is-DOM-Ready|DOM ready]]
