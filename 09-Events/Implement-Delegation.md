# Implementing Event Delegation

## Definition

Event delegation is a technique where you attach a single event listener to a parent element to handle events from multiple child elements. This works because events bubble up from child to parent.

## Why Use Event Delegation

1. **Performance**: One listener instead of many
2. **Dynamic content**: Works for elements added later
3. **Memory efficiency**: Less memory usage
4. **Simpler maintenance**: Easier to manage

## Syntax

```javascript
// Parent listener with event.target check
parentElement.addEventListener("click", function(event) {
    if (event.target.matches("selector")) {
        // Handle the event
    }
});
```

## Code Examples

### Basic Event Delegation

```javascript
// Instead of adding listener to each item:
// BAD
items.forEach(item => {
    item.addEventListener("click", handleClick);
});

// GOOD - delegate to parent
document.getElementById("list").addEventListener("click", function(event) {
    if (event.target.tagName === "LI") {
        handleClick(event.target);
    }
});
```

### Using matches() Method

```javascript
// Check if clicked element matches selector
document.getElementById("container").addEventListener("click", function(event) {
    let target = event.target;
    
    if (target.matches(".delete-btn")) {
        deleteItem(target);
    } else if (target.matches(".edit-btn")) {
        editItem(target);
    } else if (target.matches(".item")) {
        selectItem(target);
    }
});
```

### Dynamic Content Example

```javascript
// Works for elements added after page load
let todoList = document.getElementById("todo-list");

todoList.addEventListener("click", function(event) {
    let item = event.target.closest(".todo-item");
    
    if (!item) return;
    
    if (event.target.matches(".complete-btn")) {
        item.classList.toggle("completed");
    } else if (event.target.matches(".delete-btn")) {
        item.remove();
    }
});

// New items added later will also work!
```

### Using closest() for Nested Elements

```javascript
// Handle clicks on deeply nested elements
document.getElementById("table").addEventListener("click", function(event) {
    // Find closest matching ancestor
    let row = event.target.closest("tr");
    
    if (row && row.matches(".data-row")) {
        highlightRow(row);
    }
});
```

### Form Event Delegation

```javascript
// Delegate form events
document.getElementById("myForm").addEventListener("input", function(event) {
    let target = event.target;
    
    if (target.matches("input[type='email']")) {
        validateEmail(target);
    } else if (target.matches("input[type='password']")) {
        validatePassword(target);
    }
});
```

### Keyboard Event Delegation

```javascript
// Delegate keyboard events
document.addEventListener("keydown", function(event) {
    let target = event.target;
    
    if (target.matches(".search-input")) {
        if (event.key === "Enter") {
            performSearch(target.value);
        }
    } else if (target.matches(".number-input")) {
        if (!/[0-9]/.test(event.key) && event.key !== "Backspace") {
            event.preventDefault();
        }
    }
});
```

### Complex UI Example: Tabs

```javascript
// Tab component delegation
let tabContainer = document.querySelector(".tabs");

tabContainer.addEventListener("click", function(event) {
    let tab = event.target.closest(".tab");
    
    if (!tab) return;
    
    // Deactivate all tabs
    tabContainer.querySelectorAll(".tab").forEach(t => {
        t.classList.remove("active");
    });
    
    // Activate clicked tab
    tab.classList.add("active");
    
    // Show corresponding panel
    let panelId = tab.dataset.panel;
    document.getElementById(panelId).classList.add("active");
});
```

## Common Use Cases

1. **Lists**: Todo lists, shopping carts, menus
2. **Tables**: Row/cell interactions
3. **Forms**: Input validation, dynamic fields
4. **Navigation**: Menu items, breadcrumbs
5. **Interactive UI**: Tabs, accordions, modals
6. **Data tables**: Sorting, filtering, selection

## Common Mistakes

1. **Not checking event.target** - Always verify what was clicked
2. **Forgetting closest()** - Use for nested elements
3. **Not handling event.target null** - Check if target exists
4. **Over-delegating** - Some events don't bubble (focus, blur)
5. **Ignoring performance** - Don't delegate everything blindly

## Related Topics

- [[Add-Listener]] - Add event listeners
- [[Stop-Bubbling]] - Control event propagation
- [[Use-Capture]] - Use capture phase with delegation
- [[Create-Elements]] - Create dynamic content
- [[Add-Elements]] - Add elements to DOM
- [[Remove-Elements]] - Remove elements from DOM

## Quick Revision

| Method | Purpose |
|--------|---------|
| event.target | Element that triggered event |
| element.matches() | Check if element matches selector |
| element.closest() | Find closest matching ancestor |

**Pattern**:
```javascript
parent.addEventListener("event", function(event) {
    let target = event.target.closest(".child-selector");
    if (!target) return;
    // Handle event
});
```

**Best Practice**: Use event delegation for dynamic content; always check `event.target`; use `closest()` for nested elements; don't delegate events that don't bubble.
