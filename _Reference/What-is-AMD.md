# What-is-AMD

## Definition

AMD (Asynchronous Module Definition) loads **modules asynchronously**.

## Example

```javascript
// Define
define('myModule', ['dep1', 'dep2'], function(dep1, dep2) {
    return {
        doSomething: function() {}
    };
});

// Require
require(['myModule'], function(myModule) {
    myModule.doSomething();
});
```

## Quick Revision

- AMD = async module loading
- define() to define modules
- require() to load modules
- Legacy (use ES modules now)

---

## Related Topics

- [[What-is-AMD]] - [[What-is-AMD|AMD]]
- [[What-is-AMD]] - [[What-is-AMD|AMD]]
- [[AMD]] - [[AMD|AMD]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
