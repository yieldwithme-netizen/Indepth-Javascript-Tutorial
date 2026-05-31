# What is Event Bubbling?

## Definition

Event bubbling is when an event **propagates upward** from the target element to the root.

## How It Works

```html
<div id="outer">
  <div id="inner">
    <button id="btn">Click</button>
  </div>
</div>
```

```javascript
// Click on button
// 1. Button handler runs
// 2. Inner div handler runs
// 3. Outer div handler runs
// 4. Body handler runs
// ... up to document

document.getElementById("btn").addEventListener("click", () => {
    console.log("Button");
});

document.getElementById("inner").addEventListener("click", () => {
    console.log("Inner");
});

document.getElementById("outer").addEventListener("click", () => {
    console.log("Outer");
});

// Output when button clicked:
// Button
// Inner
// Outer
```

## Stop Bubbling

```javascript
button.addEventListener("click", (event) => {
    console.log("Button clicked!");
    event.stopPropagation(); // stops bubbling
});
```

## Quick Revision

- Bubbling = event travels up the DOM tree
- Parent handlers also fire
- Use `stopPropagation()` to stop
- Default behavior (can't be disabled)
- Enables event delegation

---

## Related Topics

- [[Stop-Bubbling]] - Stopping bubbling
- [[What-is-Capturing]] - Capturing phase
- [[What-is-Delegation]] - Event delegation
- [[What-is-Event]] - Events
- [[Stop-Propagation]] - stopPropagation()
