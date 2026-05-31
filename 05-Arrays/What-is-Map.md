# What-is-Map (Arrays)

## Definition

`map()` creates a **new array** by transforming each element.

## Syntax

```javascript
const newArray = array.map(callback(element, index, array));
```

## Examples

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
const strings = numbers.map(num => `Number: ${num}`);
```

## Quick Revision

- `map()` transforms each element
- Returns new array
- Same length as original
- Use for data transformation

---

## Related Topics

- [[What-is-MapES6]] - [[What-is-MapES6|Map overview]]
- [[Use-Map]] - [[Use-Map|Using map]]
- [[What-is-HOF]] - [[What-is-HOF|Higher-order functions]]
