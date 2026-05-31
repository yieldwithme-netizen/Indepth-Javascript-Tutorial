# setTimeout

## Definition

`setTimeout` runs code **after a delay**.

## Basic Usage

```javascript
setTimeout(() => {
    console.log("After 1 second");
}, 1000);
```

## Cancel

```javascript
const id = setTimeout(() => {
    console.log("This won't run");
}, 1000);

clearTimeout(id);
```

## Quick Revision

- `setTimeout(fn, delay)` runs after delay
- `clearTimeout(id)` cancels
- Delay in milliseconds
- Returns timer ID

---

## Related Topics

- [[What-is-SetTimeout]] - [[What-is-SetTimeout|setTimeout]]
- [[setTimeout]] - [[setTimeout|setTimeout]]
- [[What-is-SetInterval]] - [[What-is-SetInterval|setInterval]]
- [[Clear-Timers]] - [[Clear-Timers|Clearing timers]]
