# What is undefined?

## Definition

`undefined` means a variable has been **declared but not assigned** a value.

## When undefined Occurs

```javascript
// 1. Declared but not assigned
let x;
console.log(x); // undefined

// 2. Function with no return
function test() {}
console.log(test()); // undefined

// 3. Missing function argument
function greet(name) {
    return `Hello, ${name}!`;
}
console.log(greet()); // "Hello, undefined!"

// 4. Object property that doesn't exist
const obj = { name: "John" };
console.log(obj.age); // undefined

// 5. Array index that doesn't exist
const arr = [1, 2, 3];
console.log(arr[10]); // undefined
```

## undefined vs null

| undefined | null |
|-----------|------|
| Automatic (default) | Manual (intentional) |
| Means "not assigned" | Means "empty" |
| typeof returns "undefined" | typeof returns "object" |

```javascript
let a;           // undefined (automatic)
let b = null;    // null (intentional)

// Check for undefined
if (a === undefined) {
    console.log("a is undefined");
}

// Check for null
if (b === null) {
    console.log("b is null");
}
```

## Common Mistakes

```javascript
// ❌ Wrong: Comparing with ==
if (x == undefined) { }  // Also catches null!

// ✅ Right: Use ===
if (x === undefined) { }  // Only catches undefined

// ❌ Wrong: Accessing undefined property
const user = { name: "John" };
console.log(user.address.city); // TypeError!

// ✅ Right: Use optional chaining
console.log(user.address?.city); // undefined
```

## Quick Revision

- `undefined` = variable declared but not assigned
- Automatic default value
- Check with `=== undefined`
- Don't use `==` (catches null too)
- Use optional chaining (`?.`) for safe access

---

## Related Topics

- [[What-is-Null]] - null value
- [[What-is-Primitive]] - Primitive types
- [[What-is-DataType]] - Data types
- [[Use-OptionalChaining]] - Optional chaining