# What is a Data Type?

## Definition

A data type defines **what kind of value** a [[What-is-Variable|variable]] can hold and what operations can be performed on it.

## JavaScript Data Types

### Primitive Types (7)

| Type | Example | Description |
|------|---------|-------------|
| `string` | `"Hello"`, `'Hi'`, `` `Hi` `` | Text |
| `number` | `42`, `3.14`, `Infinity`, `NaN` | Numbers |
| `boolean` | `true`, `false` | True/false |
| `undefined` | `undefined` | Uninitialized |
| `null` | `null` | Intentional empty |
| `symbol` | `Symbol('id')` | Unique identifier |
| `bigint` | `123n` | Large integers |

### Reference Types (Objects)

| Type | Example | Description |
|------|---------|-------------|
| `object` | `{name: "John"}` | Key-value pairs |
| `array` | `[1, 2, 3]` | Ordered list |
| `function` | `function() {}` | Callable object |
| `date` | `new Date()` | Date/time |
| `regexp` | `/pattern/` | Pattern matching |

## Checking Data Types

```javascript
typeof "Hello";      // "string"
typeof 42;           // "number"
typeof true;         // "boolean"
typeof undefined;    // "undefined"
typeof null;         // "object" (bug!)
typeof Symbol();     // "symbol"
typeof 123n;         // "bigint"
typeof {};           // "object"
typeof [];           // "object"
typeof function(){}; // "function"
```

## Type Coercion

```javascript
// Implicit (automatic)
"5" + 3;     // "53" (number → string)
"5" - 3;     // 2 (string → number)
"5" == 5;    // true (loose equality)
"5" === 5;   // false (strict equality)

// Explicit (manual)
Number("5");     // 5
String(5);       // "5"
Boolean(0);      // false
Boolean("hello"); // true
```

## Quick Revision

- 7 primitives: [[What-is-Primitive|string]], [[What-is-Primitive|number]], [[What-is-Primitive|boolean]], [[What-is-Undefined|undefined]], [[What-is-Null|null]], [[What-is-Primitive|symbol]], [[What-is-Primitive|bigint]]
- Reference types: object, array, function
- Use `typeof` to check type
- `null` typeof returns "object" (historical bug)
- Prefer `===` over `==` to avoid coercion

---

## Related Topics

- [[What-is-Primitive]] - Primitive types deep dive
- [[What-is-Type-Coercion]] - Type coercion
- [[Type-Conversion]] - Converting types
- [[What-is-Undefined]] - undefined
- [[What-is-Null]] - null