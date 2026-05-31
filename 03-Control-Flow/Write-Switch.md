# How to Write Switch Statements

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

## Examples

```javascript
// Day of week
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
// Without break, code falls through
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
```

## Quick Revision

- Switch checks expression against multiple values
- Use `case` for each value
- Use `break` to exit (or falls through)
- `default` handles unmatched cases
- Best for single variable, multiple exact values

---

## Related Topics

- [[What-is-Switch]] - [[What-is-Switch|Switch]] overview
- [[Write-IfElse]] - [[Write-IfElse|Writing if-else]]
- [[What-is-Ternary]] - [[What-is-Ternary|Ternary operator]]
- [[Write-Ternary]] - [[Write-Ternary|Writing ternary]]
