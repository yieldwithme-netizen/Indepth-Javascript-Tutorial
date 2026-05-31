# How to Run JavaScript in Browser Console

## Opening the Console

| Browser | Shortcut            |
| ------- | ------------------- |
| Chrome  | `F12` → Console tab |
| Firefox | `F12` → Console tab |
| Safari  | `Cmd+Option+C`      |
| Edge    | `F12` → Console tab |

## Basic Operations

```javascript
// Math
2 + 2;              // 4
10 * 5;             // 50
100 / 3;            // 33.333...
2 ** 10;            // 1024 (power)

// Strings
"Hello" + " World"; // "Hello World"
"Hello".length;     // 5
"Hello".toUpperCase(); // "HELLO"

// variables
let name = "John";
const age = 30;
console.log(name, age);

// functions
function greet(name) {
    return `Hello, ${name}!`;
}
greet("Jane"); // "Hello, Jane!"

// Arrow functions
const add = (a, b) => a + b;
add(5, 3); // 8
```

## [[console]] Methods

```javascript
// Log types
console.log("Normal message");
console.info("Info message");
console.warn("Warning message");
console.error("Error message");

// Object display
const user = { name: "John", age: 30 };
console.log(user);
console.table([user]);      // Table format
console.dir(user);          // Expandable object
console.dirxml(user);       // XML format

// Grouping
console.group("Group 1");
console.log("Item 1");
console.log("Item 2");
console.groupEnd();

// Timing
console.time("Timer");
// ... code ...
console.timeEnd("Timer");   // Shows elapsed time

// Count
console.count("Label");
console.count("Label");     // Shows count

// Clear
console.clear();
```

## Manipulating the Page

```javascript
// Change background color
document.body.style.background = "lightblue";

// Change text
document.querySelector("h1").textContent = "New Title";

// Hide element
document.querySelector(".ad").style.display = "none";

// Add element
const div = document.createElement("div");
div.textContent = "I was added via console!";
document.body.appendChild(div);
```

## Inspecting Elements

```javascript
// Select element
const el = document.querySelector("#my-id");

// See properties
console.log(el);
console.log(el.innerHTML);
console.log(el.style);
console.log(el.classList);

// Get computed styles
getComputedStyle(el);
```

## Network Requests

```javascript
// Fetch data
fetch("https://api.github.com/users/octocat")
    .then(res => res.json())
    .then(data => console.table(data));
```

## Quick Revision

- `F12` → Console tab opens console
- Run any [[JavaScript]] code directly
- `[[console]].log()` to output values
- Can manipulate page elements
- Great for testing and [[debugging]]

---

## Related Topics

- [[Use-Chrome-DevTools]] - Full DevTools guide
- [[What-is-JavaScript]] - JS basics
- [[First-JS-File]] - Creating JS files
- [[Debug-JavaScript]] - Debugging techniques