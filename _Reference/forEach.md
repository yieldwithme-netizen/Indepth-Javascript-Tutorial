# forEach

## Definition

`forEach()` executes a function **for each array element**.

## Basic Syntax

```javascript
array.forEach(callback(element, index, array));
```

## Examples

```javascript
const fruits = ["apple", "banana", "orange"];

fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});
// 0: apple
// 1: banana
// 2: orange
```

## Quick Revision

- `forEach()` iterates array
- No return value (undefined)
- Cannot break/continue
- Use `for...of` if you need to break

---

## Related Topics

- [[What-is-Array]] - [[What-is-Array|Arrays]]
- [[forEach]] - [[forEach|forEach]]
- [[What-is-Map]] - [[What-is-Map|Map]]
- [[What-is-Filter]] - [[What-is-Filter|Filter]]
