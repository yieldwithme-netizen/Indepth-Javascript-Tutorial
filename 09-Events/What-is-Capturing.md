# What is Event Capturing?

## Definition

Event capturing is when an event **travels down** from the root to the target element (opposite of bubbling).

## Capturing vs Bubbling

```
Capturing: document → html → body → div → button
Bubbling:  button → div → body → html → document
```

## Enabling Capturing

```javascript
// Third parameter: true = capturing, false = bubbling (default)
div.addEventListener("click", handler, true);  // capturing
div.addEventListener("click", handler, false); // bubbling
```

## Example

```javascript
document.getElementById("outer").addEventListener("click", () => {
    console.log("Outer capturing");
}, true);

document.getElementById("inner").addEventListener("click", () => {
    console.log("Inner capturing");
}, true);

document.getElementById("btn").addEventListener("click", () => {
    console.log("Button");
});

// Output when button clicked:
// Outer capturing
// Inner capturing
// Button
```

## Quick Revision

- Capturing = event travels down from root
- Enable with third parameter: `true`
- Default is bubbling (false)
- Order: capturing → target → bubbling
- Rarely used directly

---

## Related Topics

- [[What-is-Bubbling]] - Bubbling phase
- [[Use-Capture]] - Using capture
- [[What-is-Event]] - Events
- [[Stop-Propagation]] - stopPropagation()
