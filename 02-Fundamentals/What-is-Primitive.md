# What is a Primitive Type?

## Definition

A primitive type is a **basic data type** that is **immutable** (cannot be changed) and stored by **value**.

## All Primitive Types

```javascript
// 1. String
let str1 = "Hello";       // double quotes
let str2 = 'Hello';       // single quotes
let str3 = `Hello`;       // template literal

// 2. Number
let int = 42;             // integer
let float = 3.14;         // decimal
let infinity = Infinity;  // infinity
let notNum = NaN;         // not a number

// 3. Boolean
let isTrue = true;
let isFalse = false;

// 4. Undefined
let x;                    // declared but not assigned
console.log(x);           // undefined

// 5. Null
let empty = null;         // intentional empty value

// 6. Symbol
let sym = Symbol('id');   // unique identifier

// 7. BigInt
let big = 123n;           // large integer
let huge = BigInt(9007199254740991);
```

## Primitive vs Reference

| Feature | Primitive | Reference |
|---------|-----------|-----------|
| Stored by | Value | Reference |
| Mutable | No | Yes |
| Comparison | By value | By reference |
| Examples | string, number | object, array |

```javascript
// Primitive - copied by value
let a = 10;
let b = a;      // b gets copy of value
b = 20;
console.log(a); // 10 (unchanged)

// Reference - copied by reference
let obj1 = { x: 10 };
let obj2 = obj1;   // obj2 points to same object
obj2.x = 20;
console.log(obj1.x); // 20 (changed!)
```

## Primitive Properties

```javascript
// Primitives have wrapper objects
let str = "Hello";
console.log(str.length);    // 5 (wrapper object)
console.log(str.toUpperCase()); // "HELLO"

// But they're not objects
let num = 42;
console.log(typeof num); // "number"
console.log(num instanceof Number); // false
```

## Quick Revision

- Primitives: string, number, boolean, null, undefined, symbol, bigint
- Stored by value, not reference
- Immutable (methods return new values)
- Have temporary wrapper objects
- Compare by value with `===`

---

## Related Topics

- [[What-is-DataType]] - Data types overview
- [[What-is-Undefined]] - undefined
- [[What-is-Null]] - null
- [[What-is-NaN]] - NaN
- [[Check-NaN]] - Checking for NaN