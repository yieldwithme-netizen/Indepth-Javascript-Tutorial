# Delegation Pattern

## Definition

Delegation pattern **handles events** by attaching listener to parent.

## Example

```javascript
document.getElementById('list').addEventListener('click', (e) => {
    if (e.target.matches('li')) {
        console.log(`Clicked: ${e.target.textContent}`);
    }
});
```

## Quick Revision

- One listener on parent
- Uses event bubbling
- Works with dynamic elements
- Memory efficient

---

## Related Topics

- [[What-is-Delegation]] - [[What-is-Delegation|Delegation]]
- [[Event-Delegation]] - [[Event-Delegation|Event delegation]]
- [[Delegation Pattern]] - [[Delegation Pattern|Delegation pattern]]
- [[Implement-Delegation]] - [[Implement-Delegation|Implementing delegation]]
