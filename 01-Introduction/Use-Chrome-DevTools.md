# How to Use Chrome DevTools

## Opening DevTools

| Method | Shortcut |
|--------|----------|
| Right-click → Inspect | `Ctrl+Shift+I` (Windows) |
| Menu → More Tools → Developer Tools | `Cmd+Option+I` (Mac) |
| Direct shortcut | `F12` |

## The Console Tab

```javascript
// Basic logging
console.log("Hello");
console.warn("Warning!");
console.error("Error!");

// Table display
console.table([{name: "John", age: 30}, {name: "Jane", age: 25}]);

// Grouping
console.group("User Info");
console.log("Name: John");
console.log("Age: 30");
console.groupEnd();

// Timing
console.time("loop");
for(let i = 0; i < 1000; i++) {}
console.timeEnd("loop");

// Clear
console.clear();
```

## The Elements Tab

- **View HTML structure** of the page
- **Edit HTML** directly (double-click element)
- **Edit CSS** in Styles panel
- **Add/remove classes** in Elements panel
- **Computed styles** - see final CSS values

### Useful Features

| Feature | How to Use |
|---------|------------|
| Search element | `Ctrl+F` in Elements tab |
| Edit attribute | Double-click attribute |
| Add attribute | Right-click → Add attribute |
| Delete element | Select → `Delete` key |
| Hide element | Add `display: none` style |

## The Sources Tab

- **View files** loaded by the page
- **Set breakpoints** (click line number)
- **Debug [[JavaScript]]** step by step
- **Watch variables** in Watch panel
- **Call stack** - see [[function]] calls

### Debugging Workflow

```javascript
function calculateTotal(price, quantity) {
    debugger; // Execution stops here
    const total = price * quantity;
    return total;
}

// In Sources tab:
// 1. Click line number to set breakpoint
// 2. Use controls: Resume, Step Over, Step Into, Step Out
// 3. Watch variables in right panel
```

## The Network Tab

- **See all requests** (HTML, CSS, JS, images, API calls)
- **Status codes** (200 OK, 404 Not Found, 500 Error)
- **Timing** - how long each request took
- **Response** - what the server sent
- **Headers** - request/response headers

### Useful Filters

| Filter | Shows |
|--------|-------|
| `Fetch/XHR` | API calls only |
| `JS` | JavaScript files only |
| `CSS` | CSS files only |
| `Img` | Images only |

## The Application Tab

- **LocalStorage** - persistent key-value storage
- **SessionStorage** - tab-only storage
- **Cookies** - small data stored by browser
- **Cache** - cached files
- **Service Workers** - background scripts

## Quick Revision

- `F12` opens DevTools
- **Console**: Run JS, see logs
- **Elements**: Inspect/edit HTML/CSS
- **Sources**: Debug JS with breakpoints
- **Network**: Monitor requests
- **Application**: View storage

---

## Related Topics

- [[What-is-JavaScript]] - JS basics
- [[Run-JS-Console]] - Using the console
- [[Debug-JavaScript]] - Debugging techniques
- [[What-is-DOM]] - Understanding DOM