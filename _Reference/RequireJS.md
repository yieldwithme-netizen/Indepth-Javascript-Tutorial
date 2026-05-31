# RequireJS

## Definition

RequireJS is a **JavaScript module loader** that loads modules asynchronously.

## Basic Usage

```javascript
// Define module
define(['jquery'], function($) {
    return {
        greet: function() {
            return 'Hello!';
        }
    };
});

// Load module
require(['myModule'], function(myModule) {
    myModule.greet();
});
```

## Quick Revision

- RequireJS = AMD module loader
- `define()` to define modules
- `require()` to load modules
- Async loading
- Legacy (use ES6 modules now)

---

## Related Topics

- [[What-is-RequireJS]] - [[What-is-RequireJS|RequireJS]]
- [[RequireJS]] - [[RequireJS|RequireJS]]
- [[What-is-AMD]] - [[What-is-AMD|AMD]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
