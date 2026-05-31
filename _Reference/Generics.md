# Generics (TypeScript)

## Definition

Generics create **reusable, type-safe** components.

## Basic Syntax

```typescript
function identity<T>(arg: T): T {
    return arg;
}

identity<string>("hello"); // "hello"
identity<number>(42); // 42
```

## Interfaces with Generics

```typescript
interface Box<T> {
    value: T;
}

const numBox: Box<number> = { value: 42 };
const strBox: Box<string> = { value: "hello" };
```

## Quick Revision

- Generics: `<T>` type parameter
- Creates reusable components
- Type-safe
- Use for: functions, interfaces, classes

---

## Related Topics

- [[What-is-Generic]] - [[What-is-Generic|Generics]]
- [[Generics]] - [[Generics|Generics]]
- [[What-is-TypeScript]] - [[What-is-TypeScript|TypeScript]]
- [[What-is-Interface]] - [[What-is-Interface|Interfaces]]
