# Frameworks

JavaScript frameworks are pre-written libraries that provide structure, tools, and conventions for building applications. They handle common tasks so developers can focus on business logic.

## What is a Framework?

A framework provides:
- Architecture and project structure
- Built-in tools and utilities
- Conventions and best practices
- Performance optimizations
- Community support

## Popular Frontend Frameworks

### React

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

export default Counter;
```

### Vue.js

```javascript
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0
    };
  },
  methods: {
    increment() {
      this.count++;
    }
  }
};
</script>
```

### Angular

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <div>
      <p>Count: {{ count }}</p>
      <button (click)="increment()">Increment</button>
    </div>
  `
})
export class CounterComponent {
  count = 0;

  increment() {
    this.count++;
  }
}
```

## Popular Backend Frameworks

### Express.js (Node.js)

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.json({ message: 'Hello World' });
});

app.listen(3000);
```

### Django (Python)

```python
from django.http import JsonResponse

def hello(request):
    return JsonResponse({'message': 'Hello World'})
```

## Framework vs Library

```javascript
// Library - You call the code
import { useState } from 'react';
const [count, setCount] = useState(0);

// Framework - The framework calls your code
// Angular components are called by Angular
```

## Common Use Cases

- Single Page Applications (SPAs)
- Server-side rendering (SSR)
- Mobile app development
- API development
- Enterprise applications

## Common Mistakes

- Choosing a framework without understanding the project needs
- Over-engineering simple applications
- Not learning the fundamentals before using frameworks
- Ignoring framework updates and security patches
- Mixing multiple frameworks unnecessarily

## Related Topics

- [[React]]
- [[Vue.js]]
- [[Angular]]
- [[Node.js]]
- [[Express.js]]
- [[SPA Architecture]]

## Quick Revision

- Frameworks provide structure and tools for building apps
- React, Vue, Angular are popular frontend frameworks
- Express.js is a popular Node.js backend framework
- Frameworks call your code; libraries are called by your code
- Choose frameworks based on project requirements
