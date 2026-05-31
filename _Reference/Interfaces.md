# Interfaces (TypeScript)

## Definition

Interfaces define the **shape of objects** in TypeScript.

## Basic Syntax

```typescript
interface User {
    name: string;
    age: number;
    email?: string; // optional
}

const user: User = {
    name: "John",
    age: 30
};
```

## Extending Interfaces

```typescript
interface Animal {
    name: string;
}

interface Dog extends Animal {
    breed: string;
}

const dog: Dog = {
    name: "Rex",
    breed: "Labrador"
};
```

## Quick Revision

- Interface = object shape
- Optional properties: `?`
- Extend with `extends`
- Use for type checking

---

## Related Topics

- [[What-is-Interface]] - [[What-is-Interface|Interfaces]]
- [[What-is-TypeScript]] - [[What-is-TypeScript|TypeScript]]
- [[What-is-TypeAnnotation]] - [[What-is-TypeAnnotation|Type annotations]]
- [[Types-vs-Interface]] - [[Types-vs-Interface|Types vs interfaces]]
