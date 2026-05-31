# Keyboard Events

## Definition

Keyboard events fire when user **presses keys**.

## Events

| Event | When |
|-------|------|
| keydown | Key pressed |
| keyup | Key released |

## Example

```javascript
document.addEventListener('keydown', (e) => {
    console.log(`Key: ${e.key}`);
    console.log(`Code: ${e.code}`);
    
    // Shortcuts
    if (e.ctrlKey && e.key === 's') {
        e.preventDefault();
        save();
    }
});
```

## Quick Revision

- keydown: key pressed
- keyup: key released
- `e.key`: the key value
- `e.code`: physical key

---

## Related Topics

- [[What-is-KeyboardEvent]] - [[What-is-KeyboardEvent|Keyboard events]]
- [[Keyboard-Events]] - [[Keyboard-Events|Keyboard events]]
- [[Handle-Keys]] - [[Handle-Keys|Key handling]]
- [[What-is-Event]] - [[What-is-Event|Events]]
