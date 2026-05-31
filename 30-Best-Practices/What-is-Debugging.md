# What is Debugging?

## Definition

Debugging is **finding and fixing errors** in your code.

## Console Methods

```javascript
console.log("Basic log");
console.warn("Warning");
console.error("Error");
console.table([{ name: "John" }, { name: "Jane" }]);
console.dir(document.body);
console.time("timer");
// ... code ...
console.timeEnd("timer");
console.group("Group");
console.log("Item 1");
console.groupEnd();
```

## Breakpoints

```javascript
// In code
debugger; // Execution pauses here

// In DevTools
// 1. Open Sources tab
// 2. Click line number
// 3. Use controls: Resume, Step Over, Step Into, Step Out
```

## Common Debugging Techniques

```javascript
// 1. Isolate the problem
console.log("Step 1");
console.log("Step 2");

// 2. Check values
console.log("variable:", variable);

// 3. Use debugger
debugger;

// 4. Check error message
try {
    riskyCode();
} catch (error) {
    console.log(error.message);
    console.log(error.stack);
}
```

## Quick Revision

- Debugging = finding and fixing errors
- Use console.log() for quick checks
- Use debugger for breakpoints
- Check error messages and stack traces
- Isolate the problem

---

## Related Topics

- [[What-is-Debugging]] - Debugging overview
- [[Debug-JavaScript]] - Debugging techniques
- [[Use-Chrome-DevTools]] - DevTools
- [[What-is-Error]] - Errors
- [[Handle-AsyncErrors]] - Async errors
