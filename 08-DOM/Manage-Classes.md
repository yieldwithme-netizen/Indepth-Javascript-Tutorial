# Managing Classes with classList

## Definition

The `classList` property provides methods to add, remove, toggle, and check CSS classes on an element. It's the modern, recommended way to manipulate element classes.

## Syntax

```javascript
element.classList.add("className");
element.classList.remove("className");
element.classList.toggle("className");
element.classList.contains("className");
element.classList.replace("oldClass", "newClass");
```

## Code Examples

### Adding Classes

```javascript
// Add single class
document.getElementById("box").classList.add("active");

// Add multiple classes
element.classList.add("highlight", "visible", "animated");
```

### Removing Classes

```javascript
// Remove single class
document.getElementById("box").classList.remove("active");

// Remove multiple classes
element.classList.remove("hidden", "disabled");
```

### Toggling Classes

```javascript
// Toggle a class (add if absent, remove if present)
element.classList.toggle("dark-mode");

// Toggle with force parameter
element.classList.toggle("menu-open", isOpen); // true = add, false = remove
```

### Checking Classes

```javascript
// Check if class exists
if (element.classList.contains("active")) {
    console.log("Element is active");
}

// Get number of classes
let classCount = element.classList.length;
```

### Replacing Classes

```javascript
// Replace one class with another
element.classList.replace("old-theme", "new-theme");
```

### Working with Data Attributes

```javascript
// Toggle class based on data attribute
let isActive = element.dataset.active === "true";
element.classList.toggle("active", isActive);
```

## Common Use Cases

1. **Theme switching**: Toggle dark/light mode
2. **Interactive states**: Active, hover, selected states
3. **Animations**: Add animation classes on events
4. **Responsive design**: Add mobile/desktop classes
5. **Form validation**: Add error/success classes

## Common Mistakes

1. **Using className instead of classList** - classList is more flexible and safer
2. **Forgetting to check before adding** - Use contains() to avoid duplicates
3. **Overusing toggle** - Sometimes explicit add/remove is clearer

## Related Topics

- [[Change-Styles]] - Direct style manipulation
- [[Change-Text]] - Update text content
- [[Add-Listener]] - Handle events to trigger class changes
- [[Create-Elements]] - Create elements with classes

## Quick Revision

| Method | Purpose |
|--------|---------|
| add() | Add class(es) |
| remove() | Remove class(es) |
| toggle() | Toggle class |
| contains() | Check if class exists |
| replace() | Replace one class with another |

**Best Practice**: Use `classList` for all class manipulations - it's safer and more readable than `className`.
