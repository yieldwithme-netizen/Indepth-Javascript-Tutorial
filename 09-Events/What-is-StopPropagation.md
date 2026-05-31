# What is stopPropagation()?

## Definition

`stopPropagation()` **prevents the event from bubbling/capturing** to parent/child elements.

## Example

```html
<div id="outer">
  <div id="inner">
    <button id="btn">Click</button>
  </div>
</div>
```

```javascript
document.getElementById("outer").addEventListener("click", () => {
    console.log("Outer");
});

document.getElementById("inner").addEventListener("click", (e) => {
    e.stopPropagation(); // stops here
    console.log("Inner");
});

document.getElementById("btn").addEventListener("click", (e) => {
    e.stopPropagation(); // stops here
    console.log("Button");
});

// Click button output:
// Button (Inner and Outer don't run)
```

## stopPropagation vs preventDefault

```javascript
// stopPropagation: stops event from reaching other elements
// preventDefault: stops browser default behavior

element.addEventListener("click", (e) => {
    e.stopPropagation();    // parent handlers won't fire
    e.preventDefault();     // browser default won't happen
});
```

## Quick Revision

- `stopPropagation()` stops event propagation
- Prevents parent/child handlers from firing
- Use when: nested elements need separate handling
- Doesn't affect default behavior
- Use `preventDefault()` for that

---

## Related Topics

- [[Stop-Propagation]] - Using stopPropagation
- [[What-is-Bubbling]] - Event bubbling
- [[What-is-Capturing]] - Event capturing
- [[Prevent-Default]] - preventDefault()
