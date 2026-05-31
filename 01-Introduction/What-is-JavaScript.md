# What is JavaScript?

## Definition

[[JavaScript]] is a **high-level, interpreted programming language** that makes web pages interactive. It's the programming language of the web.

## History

- **Created:** 1995 by Brendan Eich at Netscape
- **Original name:** Mocha → LiveScript → [[JavaScript]]
- **Standard:** ECMAScript (managed by TC39 committee)
- **Note:** [[JavaScript]] has nothing to do with Java (naming was a marketing trick)

## Where JavaScript Runs

| Environment | Engine | Use Case |
|-------------|--------|----------|
| Chrome | V8 | Browser |
| Firefox | SpiderMonkey | Browser |
| Safari | JavaScriptCore | Browser |
| Node.js | V8 | Server |
| Deno | V8 | Server |

## Key Features

```javascript
// 1. Interpreted (runs line by line)
console.log("Hello World");

// 2. Dynamically typed
let x = 10;      // number
x = "hello";     // now string - no error

// 3. Prototype-based
const person = { name: "John" };
const student = Object.create(person); // inherits from person

// 4. event-driven
button.addEventListener("click", () => {
    alert("Clicked!");
});

// 5. Single-threaded (with async capabilities)
setTimeout(() => console.log("Later"), 1000);
```

## What Can JavaScript Do?

| Capability | Example |
|------------|---------|
| Manipulate HTML/CSS | Change styles, add elements |
| Handle [[event]]s | Clicks, form submissions |
| Send network requests | Fetch data from APIs |
| Store data | LocalStorage, IndexedDB |
| Create animations | CSS animations, canvas |
| Build apps | React, Vue, Angular |
| Server-side | Node.js, Express |
| Mobile apps | React Native |
| Desktop apps | Electron |

## JavaScript vs Other Languages

| Feature | JavaScript | Python | Java | C++ |
|---------|------------|--------|------|-----|
| Typing | Dynamic | Dynamic | Static | Static |
| Execution | Interpreted | Interpreted | Compiled | Compiled |
| Learning Curve | Easy | Easy | Moderate | Hard |
| Primary Use | Web | General | Enterprise | Systems |

## Quick Revision

- [[JavaScript]] = language of the web
- Created in 1995, standardized as ECMAScript
- Runs in browsers AND servers (Node.js)
- Interpreted, dynamically typed, [[event]]-driven
- Can do frontend, backend, mobile, desktop

---

## Related Topics

- [[What-is-ES6]] - ECMAScript versioning
- [[What-is-Browser-Engine]] - How browsers run JS
- [[What-is-V8-Engine]] - Chrome's JavaScript engine
- [[What-is-DOM]] - How JS interacts with HTML
- [[Run-JS-Console]] - Running JS in browser console