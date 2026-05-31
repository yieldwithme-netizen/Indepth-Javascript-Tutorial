# What is an Operator?

## Definition

An operator is a **symbol** that performs operations on values (operands).

## Operator Types

### 1. Arithmetic Operators

```javascript
let a = 10, b = 3;

a + b;   // 13 (addition)
a - b;   // 7 (subtraction)
a * b;   // 30 (multiplication)
a / b;   // 3.333... (division)
a % b;   // 1 (modulus/remainder)
a ** b;  // 1000 (exponentiation)
```

### 2. Assignment Operators

```javascript
let x = 10;    // =  (assignment)
x += 5;       // x = x + 5  (15)
x -= 3;       // x = x - 3  (12)
x *= 2;       // x = x * 2  (24)
x /= 4;       // x = x / 4  (6)
x %= 5;       // x = x % 5  (1)
x **= 2;      // x = x ** 2 (36)
```

### 3. Comparison Operators

```javascript
5 == "5";     // true (loose equality)
5 === "5";    // false (strict equality)
5 != "5";     // false
5 !== "5";    // true
5 > 3;        // true
5 < 3;        // false
5 >= 5;       // true
5 <= 4;       // false
```

### 4. Logical Operators

```javascript
true && false;  // false (AND)
true || false;  // true (OR)
!true;          // false (NOT)
```

### 5. Unary Operators

```javascript
let x = 5;
-x;         // -5 (negation)
+x;         // 5 (positive)
++x;        // 6 (increment)
--x;        // 4 (decrement)
typeof x;   // "number"
delete obj.prop; // delete property
```

### 6. Ternary Operator

```javascript
let age = 20;
let status = age >= 18 ? "adult" : "minor";
```

## Operator Precedence

```javascript
// Order of operations (highest to lowest)
1. ()         - Grouping
2. **         - Exponentiation
3. *, /, %    - Multiplication, Division, Modulus
4. +, -       - Addition, Subtraction
5. <, >       - Comparison
6. ==, ===    - Equality
7. &&         - Logical AND
8. ||         - Logical OR
9. ? :        - Ternary
10. =         - Assignment
```

## Quick Revision

- Operators perform operations on values
- Arithmetic: `+`, `-`, `*`, `/`, `%`, `**`
- Assignment: `=`, `+=`, `-=`, etc.
- Comparison: `==`, `===`, `!=`, `!==`
- Logical: `&&`, `||`, `!`
- Always use `===` over `==`

---

## Related Topics

- [[Arithmetic-Operators]] - Arithmetic deep dive
- [[Use-Comparison]] - Comparison operators
- [[Use-Logical]] - Logical operators
- [[What-is-Ternary]] - Ternary operator