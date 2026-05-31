# What is null?

## Definition

`null` is an **intentional absence** of any value. You explicitly assign it to indicate "no value".

## When to Use null

```javascript
// 1. Reset a variable
let user = getUser();
user = null; // Clear user

// 2. Empty DOM element
const element = document.getElementById("box");
element.innerHTML = null; // Clear content

// 3. Default value
function greet(name) {
    name = name || null;
    return `Hello, ${name || "Guest"}!`;
}

// 5. Event handler cleanup
button.onclick = null; // Remove handler
```

## null vs undefined

| null | undefined |
|------|-----------|
| Assigned intentionally | Default state |
| Means "empty" | Means "not assigned" |
| `typeof null` → "object" | `typeof undefined` → "undefined" |
| `== undefined` → true | `== null` → true |

```javascript
// null is an assignment
let x = null;

// undefined is default
let y;

// They're equal with ==
console.log(null == undefined);  // true
console.log(null === undefined); // false
```

## null gotchas

```javascript
// typeof null is "object" (historical bug)
console.log(typeof null); // "object"

// Convert to number
console.log(Number(null));     // 0
console.log(Number(undefined)); // NaN

// Check for null
if (value === null) {
    console.log("Is null");
}

// Check for null or undefined
if (value == null) {
    console.log("Is null or undefined");
}
```

## Quick Revision

- `null` = intentional empty value
- Assigned manually, not automatic
- `typeof null` returns "object" (bug)
- Use `=== null` to check (not `==`)
- Use for clearing values, empty states

---

## Related Topics

- [[What-is-Undefined]] - undefined
- [[What-is-Primitive]] - Primitive types
- [[What-is-DataType]] - Data types
- [[What-is-Nullish]] - Nullish coalescing