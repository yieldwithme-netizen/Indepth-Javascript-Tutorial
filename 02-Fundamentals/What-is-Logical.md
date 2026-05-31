# What is a Logical Operator?

## Definition

Logical operators combine **boolean expressions** and return a [[What-is-DataType#boolean|boolean]] value.

## Three Logical Operators

### AND ([[What-is-Operator#Logical Operators|&&]])

```javascript
// Returns true if BOTH operands are true
true && true;    // true
true && false;   // false
false && true;   // false
false && false;  // false

// Practical use
const age = 25;
const hasLicense = true;
if (age >= 18 && hasLicense) {
    console.log("Can drive");
}
```

### OR ([[What-is-Operator#Logical Operators|||]])

```javascript
// Returns true if AT LEAST ONE operand is true
true || true;    // true
true || false;   // true
false || true;   // true
false || false;  // false

// Practical use
const isAdmin = false;
const isOwner = true;
if (isAdmin || isOwner) {
    console.log("Access granted");
}
```

### NOT ([[What-is-Operator#Logical Operators|!]])

```javascript
// Flips the boolean value
!true;    // false
!false;   // true

// Double NOT (converts to boolean)
!!true;   // true
!!0;      // false
!!"hello"; // true
```

## Short-Circuit Evaluation

```javascript
// AND: stops at first false
false && console.log("Won't run"); // Doesn't execute

// OR: stops at first true
true || console.log("Won't run"); // Doesn't execute

// Practical: default values
const name = userInput || "Anonymous";

// Practical: conditional execution
isActive && renderDashboard();
```

## Truthy and Falsy Values

```javascript
// Falsy values (convert to false)
false
0
-0
0n
""
null
undefined
NaN
document.all

// Everything else is truthy
"hello"    // truthy
42         // truthy
[]         // truthy
{}         // truthy
function(){} // truthy
```

## Quick Revision

- `&&` = AND (both must be true)
- `||` = OR (at least one must be true)
- `!` = NOT (flips value)
- Short-circuit: `&&` stops at false, `||` stops at true
- Falsy: false, 0, "", null, undefined, NaN

---

## Related Topics

- [[What-is-Operator]] - Operators overview
- [[Use-Logical]] - Using logical operators
- [[What-is-Comparison]] - Comparison operators
- [[Use-OptionalChaining]] - Optional chaining