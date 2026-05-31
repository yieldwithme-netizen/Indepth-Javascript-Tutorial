# What is a Keyboard Event?

## Definition

Keyboard events fire when the user **presses or releases keys**.

## Keyboard Events

| Event | When It Fires |
|-------|---------------|
| keydown | Key pressed (repeats) |
| keyup | Key released |
| keypress | Key pressed (deprecated) |

## Examples

```javascript
// Keydown (most common)
document.addEventListener("keydown", (e) => {
    console.log(`Key: ${e.key}`);
    console.log(`Code: ${e.code}`);
});

// Key up
document.addEventListener("keyup", (e) => {
    console.log(`Released: ${e.key}`);
});
```

## Key Properties

```javascript
document.addEventListener("keydown", (e) => {
    console.log(e.key);      // "a", "Enter", "Escape"
    console.log(e.code);     // "KeyA", "Enter", "Escape"
    console.log(e.altKey);   // true if Alt pressed
    console.log(e.ctrlKey);  // true if Ctrl pressed
    console.log(e.shiftKey); // true if Shift pressed
    console.log(e.repeat);   // true if key held down
});
```

## Common Patterns

```javascript
// Keyboard shortcuts
document.addEventListener("keydown", (e) => {
    if (e.ctrlKey && e.key === "s") {
        e.preventDefault();
        saveDocument();
    }
});

// Game controls
document.addEventListener("keydown", (e) => {
    switch(e.key) {
        case "ArrowUp": moveUp(); break;
        case "ArrowDown": moveDown(); break;
        case "ArrowLeft": moveLeft(); break;
        case "ArrowRight": moveRight(); break;
    }
});

// Input validation
input.addEventListener("keydown", (e) => {
    if (!/[0-9]/.test(e.key) && e.key !== "Backspace") {
        e.preventDefault();
    }
});
```

## Quick Revision

- keydown: key pressed (use this)
- keyup: key released
- key: the actual key ("a", "Enter")
- code: physical key ("KeyA", "Enter")
- Use for: shortcuts, games, validation

---

## Related Topics

- [[Handle-Keys]] - Key handling
- [[What-is-Event]] - Events
- [[What-is-EventObject]] - Event object
- [[What-is-MouseEvent]] - Mouse events
