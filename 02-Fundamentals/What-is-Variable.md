# What is a Variable?

## Definition

A variable is a **named container** that stores a value. Think of it as a labeled box where you can put data.

## Variable Keywords

| Keyword | Scope | Reassignable | Hoisted | Can Redeclare |
|---------|-------|--------------|---------|---------------|
| `var` | Function | ✅ | ✅ | ✅ |
| `let` | Block | ✅ | ❌ | ❌ |
| `const` | Block | ❌ | ❌ | ❌ |

## Naming Rules

```javascript
// ✅ Valid names
let userName = "John";
let _private = 10;
let $element = "div";
let camelCase = true;
let UPPER_CASE = 100;
let name2 = "Jane";

// ❌ Invalid names
let 2names = "bad";      // Can't start with number
let my-name = "bad";     // No hyphens
let my name = "bad";     // No spaces
let class = "bad";       // Reserved word
```

## Declaration vs Assignment

```javascript
// Declaration (creating the variable)
let name;
console.log(name); // undefined

// Assignment (storing a value)
name = "John";
console.log(name); // "John"

// Declaration + Assignment
let age = 30;
console.log(age); // 30
```

## Quick Revision

- Variable = named container for data
- Use `const` by default, `let` when reassigning
- Never use `var` in modern JavaScript
- Names: camelCase, start with letter/_/$
- Declaration creates, assignment stores

---

## Related Topics

- [[Declare-Var]] - Using var
- [[Declare-Let]] - Using let
- [[Declare-Const]] - Using const
- [[What-is-Hoisting]] - Hoisting concept
- [[What-is-DataType]] - Data types