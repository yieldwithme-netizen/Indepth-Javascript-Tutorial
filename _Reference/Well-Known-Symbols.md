# Well-Known Symbols

Well-known symbols are built-in Symbol values that allow you to customize object behavior. They are used to define special actions like iteration, type conversion, and more.

## Symbol.iterator

Defines the default iteration behavior for an object.

```javascript
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;
    return {
      next() {
        if (current <= end) {
          return { value: current++, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

const range = new Range(1, 5);
for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}
```

## Symbol.toPrimitive

Controls how an object is converted to a primitive value.

```javascript
class Temperature {
  constructor(celsius) {
    this.celsius = celsius;
  }

  [Symbol.toPrimitive](hint) {
    if (hint === 'number') return this.celsius;
    if (hint === 'string') return `${this.celsius}°C`;
    return this.celsius; // default
  }
}

const temp = new Temperature(100);
console.log(+temp);      // 100
console.log(`${temp}`);  // 100°C
console.log(temp + 10);  // 110
```

## Symbol.hasInstance

Customizes `instanceof` behavior.

```javascript
class EvenNumber {
  static [Symbol.hasInstance](value) {
    return typeof value === 'number' && value % 2 === 0;
  }
}

console.log(4 instanceof EvenNumber);   // true
console.log(3 instanceof EvenNumber);   // false
```

## Symbol.toStringTag

Customizes the default toString behavior.

```javascript
class Collection {
  get [Symbol.toStringTag]() {
    return 'Collection';
  }
}

const col = new Collection();
console.log(Object.prototype.toString.call(col)); // [object Collection]
```

## Symbol.species

Controls the constructor used when creating derived objects.

```javascript
class MyArray extends Array {
  static get [Symbol.species]() {
    return Array;
  }
}

const original = new MyArray(1, 2, 3);
const mapped = original.map(x => x * 2);
console.log(mapped instanceof MyArray); // false
console.log(mapped instanceof Array);   // true
```

## Common Use Cases

- Creating custom iterables
- Overriding type coercion
- Customizing `instanceof`
- Extending built-in classes safely
- Defining custom tags for objects

## Common Mistakes

- Forgetting to return an iterator from `Symbol.iterator`
- Not checking `hint` in `Symbol.toPrimitive`
- Using well-known symbols incorrectly in class hierarchies
- Not returning a new instance from `Symbol.species`

## Related Topics

- [[Symbol]]
- [[Iterators]]
- [[Type Coercion]]
- [[Classes]]
- [[Prototypes]]

## Quick Revision

- Well-known symbols are built-in Symbol values with special meanings
- `Symbol.iterator` makes objects iterable
- `Symbol.toPrimitive` controls type conversion
- `Symbol.hasInstance` customizes `instanceof`
- `Symbol.toStringTag` changes `Object.prototype.toString`
- `Symbol.species` controls constructor for derived objects
