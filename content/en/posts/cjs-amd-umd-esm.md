+++
date = '2025-04-01T15:42:05+11:00'
draft = false
title = 'CJS, AMD, UMD and ESM'
+++

# CJS
CommonJs used by Node.js on the server.

E.g.
```
// math.js
module.exports = {
    add,
    subtract
}
// app.js
const math = require('./math');
const fs = require('fs');
```

Synchronous module loading.

# AMD
Module system for browser-based apps. Asynchronous module loading, which
improves the browser performance.

```
define(['dependency1', 'dependency2'], function (dep1, dep2) {
        // Module code goes here
        return {
            method1: function() { /* code */ },
            method2: function() { /* code */ }
        };
});
```

# UMD
Supports both CJS and AMD. Allows modules to work in different environments. For
both the browser and server.

# ESM
Official module system of JS. Natively supported in modern browsers. Provides
more opportunities for optimisations such as static analysis and tree shaking.
Offers asynchronous module loading support.

`import`
`export`
