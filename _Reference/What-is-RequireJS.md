# What-is-RequireJS

## Definition

RequireJS is a **module loader** for AMD modules.

## Example

```javascript
define(['jquery'], function($) {
    return {
        greet: function() { return 'Hello!'; }
    };
});

require(['myModule'], function(myModule) {
    myModule.greet();
});
```

## Quick Revision

- RequireJS = AMD loader
- define() to define modules
- require() to load modules
- Legacy (use ES modules now)

---

## Related Topics

- [[What-is-RequireJS]] - [[What-is-RequireJS|RequireJS]]
- [[What-is-RequireJS]] - [[What-is-RequireJS|RequireJS]]
- [[RequireJS]] - [[RequireJS|RequireJS]]
- [[What-is-AMD]] - [[What-is-AMD|AMD]]
