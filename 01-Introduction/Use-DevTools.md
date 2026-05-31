# How to Use Chrome DevTools

## Opening DevTools

| Method | Shortcut |
|--------|----------|
| Right-click → Inspect | `Ctrl+Shift+I` (Windows) |
| Menu → More Tools → Developer Tools | `Cmd+Option+I` (Mac) |
| Direct shortcut | `F12` |

## Console Tab

```javascript
// Basic logging
console.log("Hello");
console.warn("Warning!");
console.error("Error!");

// Table display
console.table([{name: "John", age: 30}]);

// Grouping
console.group("User Info");
console.log("Name: John");
console.groupEnd();

// Timing
console.time("loop");
for(let i = 0; i < 1000; i++) {}
console.timeEnd("loop");

// Clear
console.clear();
```

## Elements Tab

- View and edit [[What-is-DOM|HTML structure]]
- Modify [[What-is-Style|CSS styles]] directly
- Add/remove [[What-is-ClassList|CSS classes]]
- Edit [[What-is-InnerHTML|HTML content]]

## Sources Tab (Debugging)

- Set breakpoints (click line number)
- Debug [[What-is-Function|JavaScript]] step by step
- Watch variables
- Use `debugger` keyword

```javascript
function calculateTotal(price, quantity) {
    debugger; // Execution stops here
    const total = price * quantity;
    return total;
}
```

## Network Tab

- See all HTTP requests
- Check [[What-is-Fetch|fetch]] calls
- View status codes (200, 404, 500)
- Monitor [[What-is-API|API]] responses

## Application Tab

- [[What-is-LocalStorage|LocalStorage]]
- [[What-is-SessionStorage|SessionStorage]]
- Cookies
- Cache

## Quick Revision

- `F12` opens DevTools
- **Console**: Run [[What-is-JavaScript|JavaScript]], see logs
- **Elements**: Inspect [[What-is-DOM|DOM]]
- **Sources**: Debug with breakpoints
- **Network**: Monitor [[Make-HTTP|requests]]
- **Application**: View storage

---

## Related Topics

- [[What-is-JavaScript]] - [[What-is-JavaScript|JavaScript]] basics
- [[Run-JS-Console]] - Running in console
- [[Debug-JavaScript]] - [[Debug-JavaScript|Debugging]] techniques
- [[What-is-DOM]] - [[What-is-DOM|DOM]] overview
