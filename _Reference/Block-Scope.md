# Block Scope

## Definition
Block scope defines the accessibility of variables within a block of code (delimited by `{ }`). Variables declared with `let` and `const` are block-scoped, meaning they only exist within the block where they're declared.

## Code Examples

### var vs let vs const
```javascript
// var is function-scoped
function example() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10 (accessible outside block)
}

// let is block-scoped
function example2() {
  if (true) {
    let y = 10;
  }
  console.log(y); // ReferenceError: y is not defined
}

// const is block-scoped
function example3() {
  if (true) {
    const z = 10;
  }
  console.log(z); // ReferenceError: z is not defined
}
```

### Block Scope in Loops
```javascript
// var - shared across iterations
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// let - unique per iteration
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

### Nested Block Scope
```javascript
function outer() {
  const a = 1;

  if (true) {
    const b = 2;

    if (true) {
      const c = 3;
      console.log(a, b, c); // 1, 2, 3
    }

    console.log(a, b); // 1, 2
    // console.log(c); // ReferenceError
  }

  console.log(a); // 1
  // console.log(b, c); // ReferenceError
}
```

### Block Scope in Switch
```javascript
switch (value) {
  case 1:
    const x = 10; // Block scoped to this case
    break;
  case 2:
    // const x = 20; // SyntaxError: can't redeclare
    break;
}

// Use braces to create separate blocks
switch (value) {
  case 1: {
    const x = 10;
    break;
  }
  case 2: {
    const x = 20; // OK - different block
    break;
  }
}
```

### Block Scope in Functions
```javascript
function doSomething() {
  // Outer block
  const result = (() => {
    // Inner block (IIFE)
    const temp = 42;
    return temp * 2;
  })();

  console.log(result); // 84
  // console.log(temp); // ReferenceError
}
```

### Block Scope with try/catch
```javascript
try {
  const data = fetchData();
  process(data);
} catch (error) {
  // error is block-scoped to catch block
  console.error(error.message);
}

// console.log(error); // ReferenceError (if using let/const)
```

## Common Use Cases
- Loop variables
- Temporary variables
- Preventing variable hoisting
- Creating private scopes

## Common Mistakes
- **Assuming var is block-scoped**: `var` is function-scoped
- **Not using braces in switch**: Cases share scope without braces
- **Hoisting confusion**: `let`/`const` are hoisted but not initialized
- **TDZ errors**: Accessing `let`/`const` before declaration

## Related Topics
- [[Scope]]
- [[Closures]]
- [[Hoisting]]
- [[Let-Const]]
- [[Var]]

## Quick Revision
- `let` and `const` are block-scoped
- `var` is function-scoped, not block-scoped
- Each loop iteration creates new scope with `let`
- Use braces `{}` in switch cases for separate scopes
- Block scope prevents variable leaking
