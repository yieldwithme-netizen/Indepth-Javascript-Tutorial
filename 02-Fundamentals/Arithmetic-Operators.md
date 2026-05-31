# How to Use Arithmetic Operators

## Basic Operators

```javascript
// Addition
let sum = 5 + 3;      // 8

// Subtraction
let diff = 5 - 3;     // 2

// Multiplication
let product = 5 * 3;  // 15

// Division
let quotient = 6 / 3; // 2

// Modulus (remainder)
let remainder = 7 % 3; // 1

// Exponentiation
let power = 2 ** 10;  // 1024
```

## Increment/Decrement

```javascript
let x = 5;

// Increment
x++;     // x = 6 (post-increment)
++x;     // x = 7 (pre-increment)

// Decrement
x--;     // x = 6 (post-decrement)
--x;     // x = 5 (pre-decrement)
```

## Compound Assignment

```javascript
let x = 10;

x += 5;   // x = x + 5  (15)
x -= 3;   // x = x - 3  (12)
x *= 2;   // x = x * 2  (24)
x /= 4;   // x = x / 4  (6)
x %= 5;   // x = x % 5  (1)
x **= 2;  // x = x ** 2 (36)
```

## NaN Gotchas

```javascript
0 / 0;           // NaN
Infinity / Infinity; // NaN
"hello" * 5;     // NaN
undefined + 1;   // NaN
```

## Quick Revision

- `+` addition, `-` subtraction, `*` multiplication
- `/` division, `%` modulus, `**` exponentiation
- `++` increment, `--` decrement
- `+=`, `-=`, `*=`, `/=`, `%=`, `**=` compound assignment
- Invalid math returns `NaN`

---

## Related Topics

- [[What-is-Operator]] - [[What-is-Operator|Operators]] overview
- [[What-is-NaN]] - [[What-is-NaN|NaN]]
- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]]
- [[Arithmetic-Operators]] - [[Arithmetic-Operators|Arithmetic operators]]
