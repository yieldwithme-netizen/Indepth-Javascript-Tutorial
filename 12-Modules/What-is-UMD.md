# What is UMD

## Definition

UMD (Universal Module Definition) is a JavaScript module format that allows code to work in multiple environments: as a CommonJS module in Node.js, as an AMD (Asynchronous Module Definition) module in browsers with require.js, or as a global variable in browsers without any module loader. UMD was created to provide a universal solution for writing modules that work across different module systems.

UMD modules are typically used when creating libraries that need to be consumed in various environments without requiring users to adopt a specific module system. It's a pattern rather than a formal specification.

## UMD Pattern

The basic UMD pattern checks for different module systems and falls back to global variables:

```javascript
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD. Register as an anonymous module
    define([], factory);
  } else if (typeof module === 'object' && module.exports) {
    // Node. Does not work with strict CommonJS
    module.exports = factory();
  } else {
    // Browser globals
    root.MyLibrary = factory();
  }
}(typeof self !== 'undefined' ? self : this, function () {
  // Library code here
  function greet(name) {
    return `Hello, ${name}!`;
  }
  
  return {
    greet: greet
  };
}));
```

## Detailed Breakdown

### 1. AMD Support (require.js)

```javascript
// In a browser with require.js
define([], function() {
  function add(a, b) {
    return a + b;
  }
  
  return { add };
});
```

### 2. CommonJS Support (Node.js)

```javascript
// In Node.js
module.exports = {
  add: function(a, b) {
    return a + b;
  }
};
```

### 3. Browser Globals (script tag)

```html
<!-- Without any module loader -->
<script src="my-library.js"></script>
<script>
  console.log(MyLibrary.add(1, 2));
</script>
```

## Practical Example

Here's a complete UMD module for a utility library:

```javascript
// utils.umd.js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    define([], factory);
  } else if (typeof module === 'object' && module.exports) {
    module.exports = factory();
  } else {
    root.Utils = factory();
  }
}(typeof self !== 'undefined' ? self : this, function () {
  'use strict';
  
  var Utils = {
    isEmpty: function(value) {
      if (value === null || value === undefined) return true;
      if (typeof value === 'string' || Array.isArray(value)) {
        return value.length === 0;
      }
      if (typeof value === 'object') {
        return Object.keys(value).length === 0;
      }
      return false;
    },
    
    deepClone: function(obj) {
      if (obj === null || typeof obj !== 'object') {
        return obj;
      }
      
      var clone = Array.isArray(obj) ? [] : {};
      
      for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
          clone[key] = Utils.deepClone(obj[key]);
        }
      }
      
      return clone;
    },
    
    debounce: function(func, wait) {
      var timeout;
      return function() {
        var context = this;
        var args = arguments;
        clearTimeout(timeout);
        timeout = setTimeout(function() {
          func.apply(context, args);
        }, wait);
      };
    }
  };
  
  return Utils;
}));
```

## Using UMD Libraries

### In Node.js

```javascript
const Utils = require('./utils.umd.js');

console.log(Utils.isEmpty('')); // true
console.log(Utils.isEmpty({})); // true
console.log(Utils.deepClone({ a: 1 })); // { a: 1 }
```

### In AMD Environment (require.js)

```javascript
// main.js
define(['./utils.umd'], function(Utils) {
  console.log(Utils.isEmpty([])); // true
  
  var debouncedLog = Utils.debounce(function(msg) {
    console.log(msg);
  }, 300);
  
  debouncedLog('Hello');
});
```

### In Browser with script tag

```html
<!DOCTYPE html>
<html>
<head>
  <script src="utils.umd.js"></script>
</head>
<body>
  <script>
    console.log(Utils.isEmpty(null)); // true
    
    var handler = Utils.debounce(function() {
      console.log('Input changed');
    }, 250);
    
    document.getElementById('search').addEventListener('input', handler);
  </script>
</body>
</html>
```

## Building UMD Modules

### Using Webpack

```javascript
// webpack.config.js
module.exports = {
  output: {
    filename: 'my-library.js',
    library: 'MyLibrary',
    libraryTarget: 'umd',
    libraryExport: 'default',
    globalObject: 'this'
  }
};
```

### Using Rollup

```javascript
// rollup.config.js
export default {
  input: 'src/index.js',
  output: {
    file: 'dist/my-library.umd.js',
    format: 'umd',
    name: 'MyLibrary',
    globals: {}
  }
};
```

### Using Babel with UMD preset

```json
{
  "presets": [
    ["@babel/preset-env", {
      "modules": false
    }]
  ],
  "plugins": [
    ["@babel/plugin-transform-modules-umd", {
      "exactGlobals": true
    }]
  ]
}
```

## Common Use Cases

- **JavaScript libraries**: jQuery, Lodash, and many other libraries use UMD
- **Browser extensions**: Code that runs in both Node.js and browser contexts
- **Isomorphic applications**: Code shared between server and client
- **Legacy browser support**: When you need to support older browsers without module loaders
- **CDN distribution**: Libraries hosted on CDNs for direct browser use

## Common Mistakes

1. **Incorrect global object reference**
   ```javascript
   // Wrong: 'this' might be undefined in strict mode
   (function(root, factory) {
     // ...
   }(this, function() { ... }));
   
   // Correct: Use 'self' or 'typeof self'
   (function(root, factory) {
     // ...
   }(typeof self !== 'undefined' ? self : this, function() { ... }));
   ```

2. **Not handling all module systems**
   ```javascript
   // Incomplete UMD pattern
   (function(factory) {
     if (typeof module === 'object') {
       module.exports = factory();
     }
   }(function() { ... }));
   
   // Complete pattern handles AMD, CommonJS, and globals
   ```

3. **Missing 'use strict' directive**
   ```javascript
   // Better: Include strict mode
   (function(root, factory) {
     // ...
   }(this, function() {
     'use strict';
     // Library code
   }));
   ```

4. **Name conflicts with global variables**
   ```javascript
   // Risky: Library name might conflict
   root.Utils = factory();
   
   // Safer: Use a unique namespace
   root.MyCompanyUtils = factory();
   ```

## UMD vs Other Formats

| Format | Environment | Async | Tree Shaking | Complexity |
|--------|-------------|-------|--------------|------------|
| UMD | Universal | No | No | High |
| CommonJS | Node.js | No | No | Low |
| ES Modules | Both | Yes | Yes | Medium |
| AMD | Browser | Yes | No | Medium |
| IIFE | Browser | No | No | Low |

## Quick Revision Summary

- UMD (Universal Module Definition) works in Node.js, AMD, and browser environments
- Pattern checks for `define` (AMD), `module.exports` (CommonJS), then falls back to globals
- Primarily used for libraries that need to support multiple environments
- Use Webpack, Rollup, or Babel to bundle your code as UMD
- Less common now due to native ES Modules support in browsers and Node.js
- Still valuable when maximum compatibility is required

## Related Topics

- [[Modules]]
- [[CommonJS]]
- [[ES-Modules]]
- [[AMD]]
- [[Webpack]]
- [[Rollup]]
- [[NPM]]
- [[Build-Tools]]
