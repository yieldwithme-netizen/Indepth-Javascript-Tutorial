# What is a [[switch]] [[statement]]?

## Definition

A [[switch]] [[statement]] **executes different code** based on the value of an [[expression]].

## Basic Syntax

```javascript
switch (expression) {
    case value1:
        // runs if expression === value1
        break;
    case value2:
        // runs if expression === value2
        break;
    default:
        // runs if no case matches
}
```

## Example

```javascript
const day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of work week");
        break;
    case "Friday":
        console.log("End of work week");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend!");
        break;
    default:
        console.log("Midweek");
}
```

## Fall-Through

```javascript
// Without break, code "falls through" to next case
const month = "January";

switch (month) {
    case "January":
    case "March":
    case "May":
    case "July":
    case "August":
    case "October":
    case "December":
        console.log("31 days");
        break;
    case "April":
    case "June":
    case "September":
    case "November":
        console.log("30 days");
        break;
    case "February":
        console.log("28 or 29 days");
        break;
}
```

## [[switch]] vs [[if]]-[[else]]

```javascript
// Use switch for:
// - Single variable compared to multiple values
// - Many conditions (easier to read)

// Use if-else for:
// - Range checks (age > 18)
// - Complex conditions
// - Different variables
```

## Common Mistakes

```javascript
// ❌ Wrong: Missing break
switch (x) {
    case 1:
        console.log("one"); // falls through!
    case 2:
        console.log("two"); // also runs!
}

// ✅ Right: Always use break
switch (x) {
    case 1:
        console.log("one");
        break;
    case 2:
        console.log("two");
        break;
}

// ❌ Wrong: Using if-else for simple cases
if (color === "red") { }
else if (color === "blue") { }
else if (color === "green") { }

// ✅ Right: Use switch
switch (color) {
    case "red": break;
    case "blue": break;
    case "green": break;
}
```

## Quick Revision

- [[switch]] checks [[expression]] against multiple values
- Use `case` for each value
- Use `break` to exit (or falls through)
- `default` handles unmatched cases
- Best for single variable, multiple exact values

---

## Related Topics

- [[What-is-IfElse]] - [[if]]-[[else]] statements
- [[What-is-Ternary]] - [[ternary]] operator
- [[Write-Switch]] - Writing [[switch]]
- [[Write-IfElse]] - Writing [[if]]-[[else]]
