# How to Use Logical Operators

## AND (&&)

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

## OR (||)

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

## NOT (!)

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
false && console.log("Won't run");

// OR: stops at first true
true || console.log("Won't run");

// Default values
const name = userInput || "Anonymous";

// Conditional execution
isActive && renderDashboard();
```

## Quick Revision

- `&&` = AND (both must be true)
- `||` = OR (at least one must be true)
- `!` = NOT (flips value)
- Short-circuit: `&&` stops at false, `||` stops at true
- Falsy: false, 0, "", null, undefined, NaN

---

## Related Topics

- [[What-is-Logical]] - [[What-is-Logical|Logical operators]] overview
- [[What-is-Comparison]] - [[What-is-Comparison|Comparison operators]]
- [[What-is-Operator]] - [[What-is-Operator|Operators]]
- [[Use-OptionalChaining]] - [[Use-OptionalChaining|Optional chaining]]
