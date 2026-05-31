# insertAdjacentHTML

## Definition

insertAdjacentHTML **inserts HTML** at a specified position.

## Syntax

```javascript
element.insertAdjacentHTML(position, text);
```

## Positions

```javascript
// Before beginning of element
el.insertAdjacentHTML('beforebegin', '<p>Before</p>');

// Inside, at beginning
el.insertAdjacentHTML('afterbegin', '<p>First</p>');

// Inside, at end
el.insertAdjacentHTML('beforeend', '<p>Last</p>');

// After element
el.insertAdjacentHTML('afterend', '<p>After</p>');
```

## Quick Revision

- insertAdjacentHTML: insert HTML string
- 4 positions: beforebegin, afterbegin, beforeend, afterend
- Parses and inserts HTML
- Use for dynamic content

---

## Related Topics

- [[What-is-InnerHTML]] - [[What-is-InnerHTML|innerHTML]]
- [[insertAdjacentHTML]] - [[insertAdjacentHTML|insertAdjacentHTML]]
- [[Create-Elements]] - [[Create-Elements|Creating elements]]
- [[Add-Elements]] - [[Add-Elements|Adding elements]]
