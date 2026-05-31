# Transpilation

## Definition

Transpilation is **converting modern JavaScript** to older versions for compatibility.

## Babel

```javascript
// Install
npm install --save-dev @babel/core @babel/preset-env

// .babelrc
{
    "presets": ["@babel/preset-env"]
}
```

## What It Does

```javascript
// Input (ES6)
const greet = (name) => `Hello, ${name}!`;

// Output (ES5)
var greet = function greet(name) {
    return "Hello, " + name + "!";
};
```

## Quick Revision

- Transpilation = code conversion
- Babel is the main tool
- Converts ES6+ to ES5
- Use for browser compatibility

---

## Related Topics

- [[What-is-Babel]] - [[What-is-Babel|Babel]]
- [[Transpilation]] - [[Transpilation|Transpilation]]
- [[Configure-Babel]] - [[Configure-Babel|Babel config]]
- [[What-is-ES6]] - [[What-is-ES6|ES6]]
