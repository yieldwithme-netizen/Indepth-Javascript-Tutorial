# What is Event Delegation?

## Definition

Event delegation is a technique where you **attach one event listener to a parent** instead of multiple listeners to children.

## Without Delegation

```javascript
// ❌ Bad: Multiple listeners
document.querySelectorAll("button").forEach(button => {
    button.addEventListener("click", handleClick);
});
```

## With Delegation

```javascript
// ✅ Good: One listener on parent
document.getElementById("list").addEventListener("click", (event) => {
    if (event.target.tagName === "LI") {
        console.log(`Clicked: ${event.target.textContent}`);
    }
});
```

## Why Use Delegation

```javascript
// 1. Works with dynamic elements
const addItem = () => {
    const li = document.createElement("li");
    li.textContent = "New item";
    document.getElementById("list").appendChild(li);
    // No need to add listener to new li!
};

// 2. Memory efficient
// One listener instead of hundreds

// 3. Simpler code
// Less code to manage
```

## Quick Revision

- Delegation = parent handles child events
- One listener instead of many
- Works with dynamically added elements
- Use `event.target` to find clicked element
- Memory efficient

---

## Related Topics

- [[Implement-Delegation]] - Implementing delegation
- [[What-is-Bubbling]] - Event bubbling
- [[What-is-Event]] - Events
- [[Handle-Clicks]] - Click handling
