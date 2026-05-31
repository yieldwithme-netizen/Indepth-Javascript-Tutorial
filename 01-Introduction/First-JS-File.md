# How to Create Your First JavaScript File

## Step 1: Create a File

Create a new file called `hello.js`:

```javascript
// This is a comment
// Comments are ignored by JavaScript

// Print to console
console.log("Hello, World!");

// Alert (only in browser)
// alert("Hello!");

// variable
let message = "JavaScript is fun!";
console.log(message);

// function
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet("John"));
```

## Step 2: Run the File

### Using Node.js

```bash
# Open terminal in file directory
node hello.js

# Output:
# Hello, World!
# JavaScript is fun!
# Hello, John!
```

### Using Browser

1. Create `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First JS</title>
</head>
<body>
    <h1>Check the Console!</h1>
    <script src="hello.js"></script>
</body>
</html>
```

2. Open `index.html` in browser
3. Press `F12` → Console tab
4. See output

## File Naming Rules

| Rule | Example | Why |
|------|---------|-----|
| Use .js extension | `hello.js` | Identifies as [[JavaScript]] |
| No spaces | `my-file.js` | Avoids issues |
| Lowercase | `app.js` | Convention |
| No special chars | `app1.js` ✓ | `app@1.js` ✗ |

## Code Structure

```javascript
// 1. Comments (documentation)
// Single line comment

/* 
   Multi-line 
   comment
*/

// 2. variables (store data)
let name = "John";
const age = 30;

// 3. functions (reusable code)
function calculateTotal(price, tax) {
    return price + (price * tax);
}

// 4. Execution (run code)
const total = calculateTotal(100, 0.1);
console.log(`Total: $${total}`);
```

## Common Mistakes

```javascript
// ❌ Wrong: Missing semicolons (optional but recommended)
let x = 5
let y = 10

// ✅ Right: Use semicolons
let x = 5;
let y = 10;

// ❌ Wrong: Using var
var name = "John";

// ✅ Right: Use let/const
const name = "John";

// ❌ Wrong: console.log without quotes
console.log(Hello);

// ✅ Right: Strings need quotes
console.log("Hello");
```

## Quick Revision

1. Create file with `.js` extension
2. Write [[JavaScript]] code
3. Run with `node filename.js` (Node.js)
4. Or link to HTML with `<script src="filename.js"></script>`
5. Use `[[console]].log()` to see output

---

## Related Topics

- [[What-is-JavaScript]] - JS basics
- [[Link-JS-HTML]] - Connecting JS to HTML
- [[Setup-VS-Code]] - Editor setup
- [[Run-JS-Console]] - Running in browser
- [[What-is-Node]] - Node.js basics