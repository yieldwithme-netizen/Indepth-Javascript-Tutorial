# What is a Browser Engine?

## Definition

A browser engine is the **core component** that takes your HTML, CSS, and [[JavaScript]] and **renders it as a visible web page**.

## How It Works

```
Your Code → Browser Engine → Pixels on Screen
```

### The Two Engines in Every Browser

| Engine | Purpose | Example |
|--------|---------|---------|
| **Rendering Engine** | Parses HTML/CSS, draws pixels | Blink, Gecko |
| **JavaScript Engine** | Parses and runs JS | V8, SpiderMonkey |

## Major Browser Engines

| Browser | JS Engine | Rendering Engine |
|---------|-----------|------------------|
| Chrome | V8 | Blink |
| Firefox | SpiderMonkey | Gecko |
| Safari | JavaScriptCore | WebKit |
| Edge | V8 | Blink |
| Opera | V8 | Blink |

## How the Rendering Engine Works

```
1. Parse HTML → DOM Tree
2. Parse CSS → CSSOM Tree
3. Combine → Render Tree
4. Layout (calculate positions)
5. Paint (draw pixels)
6. Composite (layer management)
```

## How the JavaScript Engine Works

```
1. Parse → Abstract Syntax Tree (AST)
2. Compile → Bytecode
3. Optimize → Machine Code
4. Execute
```

### V8 Engine Internals

```
Source Code
    ↓
Parser → AST (Abstract Syntax Tree)
    ↓
Ignition (Interpreter) → Bytecode
    ↓
TurboFan (Compiler) → Optimized Machine Code
```

## Why This Matters

```javascript
// Slow (causes reflow/repaint)
element.style.width = "100px";
element.style.height = "100px";
element.style.background = "red";

// Fast (batch changes)
element.style.cssText = "width:100px;height:100px;background:red";

// Even better (use CSS classes)
element.classList.add("box");
```

## Quick Revision

- Browser engine = renders web pages
- Two engines: Rendering (HTML/CSS) + JavaScript (JS)
- Chrome/Edge/Opera use V8 for JS
- Rendering: Parse → DOM → Render → Paint
- JavaScript: Parse → AST → Bytecode → Machine Code

---

## Related Topics

- [[What-is-V8-Engine]] - V8 deep dive
- [[What-is-DOM]] - Document Object Model
- [[What-is-JS-Runtime]] - Runtime environment
- [[Optimize-Rendering]] - Performance tips