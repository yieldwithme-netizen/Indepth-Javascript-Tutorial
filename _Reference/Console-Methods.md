# Console Methods

## Definition

Console methods are used for **debugging and logging** in JavaScript.

## Basic Methods

```javascript
console.log("Hello");      // Standard output
console.info("Info");      // Information
console.warn("Warning");   // Warning
console.error("Error");    // Error
```

## Object Display

```javascript
const user = { name: "John", age: 30 };

console.log(user);        // Object
console.table([user]);    // Table format
console.dir(user);        // Expandable object
console.dirxml(user);     // XML format
```

## Grouping

```javascript
console.group("Group 1");
console.log("Item 1");
console.log("Item 2");
console.groupEnd();
```

## Timing

```javascript
console.time("Timer");
// ... code ...
console.timeEnd("Timer"); // Shows elapsed time
```

## Counting

```javascript
console.count("Label");  // Label: 1
console.count("Label");  // Label: 2
console.countReset("Label");
```

## Quick Revision

- `console.log()` - standard output
- `console.table()` - table format
- `console.time()` - timing
- `console.group()` - grouping
- `console.error()` - errors

---

## Related Topics

- [[What-is-Console]] - [[What-is-Console|Console]]
- [[Console-Methods]] - [[Console-Methods|Console methods]]
- [[Debug-JavaScript]] - [[Debug-JavaScript|Debugging]]
- [[Use-Chrome-DevTools]] - [[Use-Chrome-DevTools|DevTools]]
