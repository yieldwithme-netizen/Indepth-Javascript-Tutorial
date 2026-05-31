# Handle Keys

## Definition

Keyboard events fire when users press keys. The three main keyboard events are `keydown`, `keyup`, and `keypress` (deprecated). Use these to capture user input and keyboard shortcuts.

## Code Examples

### Basic Keydown Handler

```javascript
document.addEventListener('keydown', (event) => {
  console.log('Key pressed:', event.key);
  console.log('Key code:', event.code);
});
```

### Check for Specific Keys

```javascript
document.addEventListener('keydown', (event) => {
  if (event.key === 'Enter') {
    console.log('Enter pressed');
  }
  
  if (event.key === 'Escape') {
    console.log('Escape pressed');
  }
});
```

### Modifier Keys

```javascript
document.addEventListener('keydown', (event) => {
  if (event.ctrlKey && event.key === 's') {
    event.preventDefault();
    console.log('Ctrl + S pressed');
  }
  
  if (event.shiftKey && event.key === 'A') {
    console.log('Shift + A pressed');
  }
});
```

### Input Field Validation

```javascript
const input = document.getElementById('numberInput');

input.addEventListener('keydown', (event) => {
  const allowedKeys = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
  
  if (!allowedKeys.includes(event.key) && event.key !== 'Backspace') {
    event.preventDefault();
  }
});
```

### Keyboard Shortcuts

```javascript
document.addEventListener('keydown', (event) => {
  const shortcuts = {
    'ctrl+z': () => undo(),
    'ctrl+y': () => redo(),
    'ctrl+s': () => save(),
    'ctrl+f': () => openSearch()
  };
  
  const key = `${event.ctrlKey ? 'ctrl+' : ''}${event.key.toLowerCase()}`;
  
  if (shortcuts[key]) {
    event.preventDefault();
    shortcuts[key]();
  }
});
```

## Key Properties

| Property | Description |
|----------|-------------|
| `event.key` | The key value (Enter, a, 1, etc.) |
| `event.code` | Physical key code (KeyA, Digit1, etc.) |
| `event.ctrlKey` | Ctrl key pressed |
| `event.shiftKey` | Shift key pressed |
| `event.altKey` | Alt key pressed |
| `event.metaKey` | Meta/Command key pressed |
| `event.repeat` | Key is held down |

## Common Use Cases

1. **Form navigation** - Tab between fields, Enter to submit
2. **Keyboard shortcuts** - Ctrl+S to save, Ctrl+Z to undo
3. **Game controls** - Arrow keys for movement
4. **Text input validation** - Restrict allowed characters

## Common Mistakes

```javascript
// Wrong: Using deprecated keypress event
element.addEventListener('keypress', handler); // Deprecated

// Correct: Use keydown
element.addEventListener('keydown', handler);

// Wrong: Comparing event.keyCode (deprecated)
if (event.keyCode === 13) { } // Deprecated

// Correct: Compare event.key
if (event.key === 'Enter') { }
```

## Related Topics

- [[Use-Event-Props]]
- [[Handle-Clicks]]
- [[Handle-Form]]
- [[Prevent-Default]]

## Quick Revision

| Event | When It Fires |
|-------|---------------|
| `keydown` | Key pressed down |
| `keyup` | Key released |
| `keypress` | Deprecated - avoid |
| Use `event.key` | For key identification |
| Use `event.code` | For physical key position |
