# DocumentFragment

## Definition

DocumentFragment is a **lightweight container** for DOM elements.

## Example

```javascript
const fragment = document.createDocumentFragment();

for (let i = 0; i < 10; i++) {
    const li = document.createElement('li');
    li.textContent = `Item ${i}`;
    fragment.appendChild(li);
}

document.getElementById('list').appendChild(fragment);
```

## Quick Revision

- DocumentFragment for batch DOM updates
- Not part of DOM until appended
- Reduces reflows
- Use for performance

---

## Related Topics

- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[DocumentFragment]] - [[DocumentFragment|DocumentFragment]]
- [[Create-Elements]] - [[Create-Elements|Creating elements]]
- [[Add-Elements]] - [[Add-Elements|Adding elements]]
