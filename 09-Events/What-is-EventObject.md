# What is the Event Object?

## Definition

The event object contains **information about the event** that occurred.

## Accessing the Event Object

```javascript
button.addEventListener("click", (event) => {
    console.log(event);
});
```

## Common Properties

```javascript
button.addEventListener("click", (event) => {
    // Event type
    console.log(event.type); // "click"
    
    // Target element
    console.log(event.target); // clicked element
    
    // Current target (element with listener)
    console.log(event.currentTarget); // button
    
    // Mouse position
    console.log(event.clientX, event.clientY);
    
    // Time
    console.log(event.timeStamp);
});
```

## Keyboard Event Properties

```javascript
document.addEventListener("keydown", (event) => {
    console.log(event.key);     // "Enter", "a", "Escape"
    console.log(event.code);    // "KeyA", "Enter"
    console.log(event.altKey);  // true if Alt pressed
    console.log(event.ctrlKey); // true if Ctrl pressed
    console.log(event.shiftKey);// true if Shift pressed
});
```

## Form Event Properties

```javascript
form.addEventListener("submit", (event) => {
    event.preventDefault();
    console.log(event.target); // the form element
});
```

## Quick Revision

- Event object passed to event handler
- Contains: type, target, coordinates, time
- Keyboard: key, code, altKey, ctrlKey
- Use `event.target` for clicked element
- Use `event.preventDefault()` to stop defaults

---

## Related Topics

- [[What-is-Event]] - Events
- [[Use-Event-Props]] - Event properties
- [[Handle-Clicks]] - Click handling
- [[Handle-Keys]] - Keyboard handling
- [[Handle-Form]] - Form handling
