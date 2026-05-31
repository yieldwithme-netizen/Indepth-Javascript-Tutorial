# What is ES6 (ECMAScript)?

## Definition

**ES6** (ECMAScript 2015) is the **6th major version** of the ECMAScript language specification. ECMAScript is the standard that [[JavaScript]] is based on.

## The Naming Confusion

```
ECMAScript = The standard/specification
JavaScript = The implementation (most popular)
```

Think of it like:
- ECMAScript = HTML specification
- [[JavaScript]] = Browsers implementing HTML

## Version History

| Version | Year | Key Features |
|---------|------|--------------|
| ES1 | 1997 | First release |
| ES3 | 1999 | Regular expressions, try/catch |
| ES5 | 2009 | Strict mode, JSON, [[Array]] methods |
| **ES6** | **2015** | **Arrow functions, classes, modules, promises** |
| ES2016 | 2016 | [[Array]].includes, exponentiation |
| ES2017 | 2017 | [[async]]/[[await]] |
| ES2018 | 2018 | Rest/spread properties |
| ES2019 | 2019 | [[Array]].flat, [[Object]].fromEntries |
| ES2020 | 2020 | Optional chaining, nullish coalescing |
| ES2021 | 2021 | [[Promise]].allSettled, [[String]].replaceAll |
| ES2022 | 2022 | Top-level [[await]], [[class]] fields |

## Why ES6 Matters

```javascript
// BEFORE ES5
var name = "John";
function greet(name) {
    return "Hello " + name;
}

// ES6 (Modern JavaScript)
const name = "John";
const greet = (name) => `Hello ${name}`;
```

## Key ES6 Features

```javascript
// 1. let and const (block scoping)
let x = 10;      // can be reassigned
const y = 20;    // cannot be reassigned

// 2. Arrow functions
const add = (a, b) => a + b;

// 3. Template literals
const msg = `Hello ${name}`;

// 4. Destructuring
const { name, age } = person;
const [first, second] = array;

// 5. Spread operator
const newArr = [...oldArr, newElement];

// 6. classes
class Animal {
    constructor(name) {
        this.name = name;
    }
}

// 7. Modules
export const greet = () => "Hi";
import { greet } from './utils.js';

// 8. Promises
const promise = new Promise((resolve, reject) => {
    resolve("Done!");
});
```

## Browser Support

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Arrow functions | 45+ | 22+ | 10+ | 12+ |
| [[let]]/[[const]] | 49+ | 44+ | 10+ | 12+ |
| Template literals | 41+ | 34+ | 9+ | 12+ |
| Classes | 49+ | 45+ | 9+ | 13+ |

**Note:** All modern browsers support ES6. For old browsers, use Babel to transpile.

## Quick Revision

- ES6 = ECMAScript 2015 = modern [[JavaScript]] standard
- Introduces: arrow functions, classes, modules, [[const]]/[[let]], promises
- All modern browsers support ES6
- Use Babel for older browser support
- ES6+ = all versions from ES6 onward

---

## Related Topics

- [[What-is-JavaScript]] - JavaScript overview
- [[Setup-VS-Code]] - Development environment
- [[What-is-Strict-Mode]] - ES5 feature
- [[Install-NodeJS]] - Running JS outside browser