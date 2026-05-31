# Event Delegation

## Definition

Event delegation **attaches one listener** to parent instead of multiple to children.

## Example

```javascript
document.getElementById('list').addEventListener('click', (e) => {
    if (e.target.tagName === 'LI') {
        console.log(`Clicked: ${e.target.textContent}`);
    }
});
```

## Quick Revision

- One listener on parent
- Works with dynamic elements
- Uses event bubbling
- Memory efficient

---

## Related Topics

- [[What-is-Delegation]] - [[What-is-Delegation|Delegation]]
- [[Event-Delegation]] - [[Event-Delegation|Event delegation]]
- [[Implement-Delegation]] - [[Implement-Delegation|Implementing delegation]]
- [[What-is-Bubbling]] - [[What-is-Bubbling|Bubbling]]
