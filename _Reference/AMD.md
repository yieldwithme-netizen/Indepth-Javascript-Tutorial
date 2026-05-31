# AMD (Asynchronous Module Definition)

## Definition
AMD (Asynchronous Module Definition) is a module format designed for browsers that loads modules asynchronously. It's the foundation for RequireJS and was popular before ES6 modules became standard.

## Basic Syntax

### Defining a Module
```javascript
// defining a module
define('myModule', [], function() {
  var privateVar = 'I am private';
  
  function privateMethod() {
    console.log(privateVar);
  }
  
  return {
    publicMethod: function() {
      privateMethod();
    },
    publicVar: 'I am public'
  };
});
```

### Loading a Module
```javascript
// loading a module
require(['myModule'], function(myModule) {
  console.log(myModule.publicVar); // 'I am public'
  myModule.publicMethod(); // 'I am private'
});
```

## Dependencies

### With Dependencies
```javascript
define('userService', ['jquery', 'lodash'], function($, _) {
  function getUser(id) {
    return $.ajax({
      url: '/api/users/' + id,
      dataType: 'json'
    });
  }
  
  function formatUser(user) {
    return _.pick(user, ['name', 'email']);
  }
  
  return {
    getUser: getUser,
    formatUser: formatUser
  };
});
```

### Require with Config
```javascript
require.config({
  baseUrl: '/js',
  paths: {
    'jquery': 'vendor/jquery.min',
    'lodash': 'vendor/lodash.min',
    'app': 'app'
  },
  shim: {
    'vendor/bootstrap': {
      deps: ['jquery']
    }
  }
});

require(['app/main'], function(main) {
  main.init();
});
```

## Practical Examples

### Module with Private State
```javascript
define('counter', [], function() {
  var count = 0;
  
  return {
    increment: function() {
      count++;
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    getCount: function() {
      return count;
    }
  };
});
```

### Module Factory Pattern
```javascript
define('greeter', [], function() {
  return function(name, greeting) {
    this.name = name;
    this.greeting = greeting || 'Hello';
    
    this.greet = function() {
      return this.greeting + ', ' + this.name + '!';
    };
  };
});
```

## Common Use Cases
- Legacy browser support (IE8 and below)
- Large-scale applications needing lazy loading
- Projects with complex dependency management
- jQuery plugins and libraries
- Enterprise applications with RequireJS

## Common Mistakes
- Circular dependencies causing issues
- Not specifying dependencies properly
- Overcomplicating module structure
- Mixing AMD with CommonJS without proper configuration
- Not using RequireJS optimizer for production

## Quick Revision Summary
- `define(name, deps, factory)` - defines a module
- `require(deps, callback)` - loads modules
- Asynchronous loading prevents blocking
- RequireJS is the most popular implementation
- Being replaced by ES6 modules in modern development

## Related Topics
- [[Modules-ES6]]
- [[CommonJS]]
- [[RequireJS]]
- [[Node-Modules]]
- [[Bundlers]]
