# How to Convert Types

## To Number

```javascript
Number("5");        // 5
Number("hello");    // NaN
Number(true);       // 1
Number(false);      // 0
Number(null);       // 0
Number(undefined);  // NaN

parseInt("5.5");    // 5
parseInt("hello");  // NaN
parseFloat("5.5");  // 5.5

+"5";               // 5
+true;              // 1
+false;             // 0
```

## To String

```javascript
String(5);          // "5"
String(true);       // "true"
String(null);       // "null"
String(undefined);  // "undefined"

(5).toString();     // "5"
true.toString();    // "true"

`Hello ${5}`;       // "Hello 5"
```

## To Boolean

```javascript
Boolean(0);         // false
Boolean("");        // false
Boolean(null);      // false
Boolean(undefined); // false
Boolean(NaN);       // false

Boolean(5);         // true
Boolean("hello");   // true
Boolean([]);        // true
Boolean({});        // true
```

## Quick Reference Table

| Value | Number | String | Boolean |
|-------|--------|--------|---------|
| "5" | 5 | "5" | true |
| 5 | 5 | "5" | true |
| "" | 0 | "" | false |
| "0" | 0 | "0" | true |
| null | 0 | "null" | false |
| undefined | NaN | "undefined" | false |
| NaN | NaN | "NaN" | false |

## Quick Revision

- `Number()` - converts to number
- `String()` - converts to string
- `Boolean()` - converts to boolean
- Falsy values: false, 0, "", null, undefined, NaN
- Everything else is truthy

---

## Related Topics

- [[What-is-Type-Coercion]] - [[What-is-Type-Coercion|Type coercion]]
- [[What-is-DataType]] - [[What-is-DataType|Data types]]
- [[What-is-Primitive]] - [[What-is-Primitive|Primitives]]
- [[Avoid-Coercion]] - [[Avoid-Coercion|Avoiding coercion bugs]]
