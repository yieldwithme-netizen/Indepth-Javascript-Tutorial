# How to Use BOM Functions

## alert() - Show Message Box

```javascript
alert("Hello World!");

// Shows: [OK] button
// Returns: undefined
```

## confirm() - Ask Yes/No Question

```javascript
const answer = confirm("Do you want to delete?");

if (answer) {
    console.log("Deleted!");
} else {
    console.log("Cancelled!");
}

// Shows: [OK] [Cancel] buttons
// Returns: true (OK) or false (Cancel)
```

## prompt() - Get User Input

```javascript
const name = prompt("Enter your name:");

if (name) {
    console.log(`Hello, ${name}!`);
} else {
    console.log("No name entered");
}

// Shows: [OK] [Cancel] buttons with input field
// Returns: string (input) or null (Cancel)

// With default value
const age = prompt("Enter age:", "25");
```

## setTimeout() - Run Code Later

```javascript
// Run after 2 seconds
setTimeout(() => {
    console.log("2 seconds passed!");
}, 2000);

// With function name
function sayHello() {
    console.log("Hello!");
}
setTimeout(sayHello, 3000);

// Cancel timeout
const timer = setTimeout(() => {
    console.log("This won't run");
}, 5000);
clearTimeout(timer);
```

## setInterval() - Run Code Repeatedly

```javascript
// Run every 1 second
let count = 0;
const interval = setInterval(() => {
    count++;
    console.log(`Count: ${count}`);
    
    if (count >= 5) {
        clearInterval(interval); // Stop after 5
    }
}, 1000);

// Cancel interval
clearInterval(interval);
```

## Practical Examples

```javascript
// Countdown timer
function countdown(seconds) {
    const timer = setInterval(() => {
        console.log(seconds);
        seconds--;
        
        if (seconds < 0) {
            clearInterval(timer);
            console.log("Time's up!");
        }
    }, 1000);
}
countdown(5);

// Auto-hide alert after 3 seconds
const alertBox = document.getElementById("alert");
alertBox.style.display = "block";
setTimeout(() => {
    alertBox.style.display = "none";
}, 3000);

// Duplicate tab detection
if (window.name === "myWindow") {
    console.log("Tab already open!");
} else {
    window.name = "myWindow";
}
```

## Quick Revision

- `alert()` = show message
- `confirm()` = yes/no question → boolean
- `prompt()` = get input → [[string]] or null
- `setTimeout()` = run once after delay
- `setInterval()` = run repeatedly
- Use `clearTimeout()`/`clearInterval()` to cancel

---

## Related Topics

- [[What-is-BOM]] - BOM overview
- [[What-is-SetTimeout]] - Timer deep dive
- [[What-is-SetInterval]] - Interval deep dive
- [[Clear-Timers]] - Clearing timers
- [[Handle-Clicks]] - [[event]] handling