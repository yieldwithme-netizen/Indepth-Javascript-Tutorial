# What is V8 Engine?

## Definition

V8 is Google's **open-source [[JavaScript]] engine** that compiles and runs JavaScript code. It's used in Chrome, Edge, Opera, and Node.js.

## How V8 Works

```
JavaScript Source Code
        ↓
    Parser → Abstract Syntax Tree (AST)
        ↓
    Ignition (Interpreter) → Bytecode
        ↓
    Hot Code Detected?
        ↓ Yes
    TurboFan (Compiler) → Optimized Machine Code
```

## Key Components

| Component | Role |
|-----------|------|
| **Parser** | Converts JS to AST |
| **Ignition** | Interprets bytecode |
| **TurboFan** | Optimizes hot code |
| **Orinoco** | Garbage collector |

## V8 Optimization Techniques

### 1. Hidden Classes

```javascript
// V8 creates hidden classes for objects
function Point(x, y) {
    this.x = x;  // Hidden class C0
    this.y = y;  // Hidden class C1
}

// Same structure = same hidden class = faster
const p1 = new Point(1, 2);
const p2 = new Point(3, 4); // Reuses hidden classes
```

### 2. Inline Caching

```javascript
// V8 caches function call results
function getX(point) {
    return point.x;  // First call: slow
                     // Subsequent: fast (cached)
}

// Keep object shapes consistent
const point1 = { x: 1, y: 2 };
const point2 = { x: 3, y: 4 }; // Same shape
getX(point1); // Cache miss
getX(point2); // Cache hit!
```

### 3. JIT Compilation

```javascript
// Just-In-Time compilation
function add(a, b) {
    return a + b;
}

// First call: interpreted
add(1, 2);      // Slow

// After many calls: compiled
add(1, 2);      // Fast (optimized)
add(3, 4);      // Fast (reuses compiled code)
```

## V8 vs Other Engines

| Feature | V8 | SpiderMonkey | JavaScriptCore |
|---------|-----|--------------|----------------|
| Company | Google | Mozilla | Apple |
| Used in | Chrome, Node | Firefox | Safari |
| JIT | Ignition + TurboFan | IonMonkey + Warp | LLInt + DFG + FTL |
| Speed | Very Fast | Fast | Fast |

## Why V8 Matters

```javascript
// V8 enables:
1. Fast browser JavaScript
2. Server-side JavaScript (Node.js)
3. Desktop apps (Electron)
4. Mobile apps (React Native)
```

## Quick Revision

- V8 = Google's JavaScript engine
- Used in Chrome, Edge, Node.js
- Uses Ignition (interpreter) + TurboFan (compiler)
- Optimizations: Hidden Classes, Inline Caching, JIT
- Enables server-side JS (Node.js)

---

## Related Topics

- [[What-is-Browser-Engine]] - Browser engines overview
- [[What-is-JS-Runtime]] - Runtime environment
- [[What-is-Node]] - Node.js (uses V8)
- [[Measure-Performance]] - V8 performance