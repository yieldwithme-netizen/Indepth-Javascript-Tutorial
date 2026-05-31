# Date

## Definition

Date object handles **date and time** operations.

## Example

```javascript
const now = new Date();
console.log(now); // Current date/time

const specific = new Date('2024-01-15');
console.log(specific.getFullYear()); // 2024
console.log(specific.getMonth());    // 0 (January)
console.log(specific.getDate());     // 15

// Format
now.toDateString();  // "Mon Jan 15 2024"
now.toTimeString();  // "12:00:00 GMT"
now.toISOString();   // "2024-01-15T12:00:00.000Z"
```

## Quick Revision

- Date object for dates/times
- Methods: getFullYear, getMonth, getDate
- Format: toDateString, toISOString
- Months are 0-indexed

---

## Related Topics

- [[What-is-Date]] - [[What-is-Date|Date]]
- [[Date]] - [[Date|Date]]
- [[What-is-String]] - [[What-is-String|Strings]]
