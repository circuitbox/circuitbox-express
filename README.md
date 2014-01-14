circuitbox-express
==================

[express](https://github.com/visionmedia/express) bindings for [circuitbox](https://github.com/oddjobsman/circuitbox)

[![build status](https://secure.travis-ci.org/oddjobsman/circuitbox-express.png)](http://travis-ci.org/oddjobsman/circuitbox-express)
[![dependency status](https://david-dm.org/oddjobsman/circuitbox-express.png)](https://david-dm.org/oddjobsman/circuitbox-express)

## Vision
- No lock-in to circuitbox/binding
- Spawn an express instance as singleton
- Config DSL to define and register middlewares as components
- Config DSL to define routes as components
- Async middleware
- Configuration file
- Error handling
- Logging & Diagnostics
- Montioring and Profiling
- Clustering

## Proposed Usage

```js
'use strict';

var circuitbox = require('circuitbox');

circuitbox.withBindings([
  'circuitbox-express'
]).create({
  modules: [
    function webModule(registry) {
      // custom middleware are defined as components
      registry.for('myMiddleware').use(function (deps) {
        return function (req, res, next) {
          // your middleware logic  
        };
      }).scopeWith(circuitbox.Scopes.prototype);

      // routes are registered with a naming convention of 'VERB path'
      registry.for('GET /foo').handledBy(function (deps) {
        return function (req, res) {
          // your route logic
          // this may well be in a separate file module
        };
      });

      /** 
       * creates an express instance
       * built-in express middleware need not be declared, they would be available by default
       * express instances are singleton scoped by default
       */
      registry.for('app')
        .expressInstance()
        .withMiddleware('bodyParser', 'methodOverride', 'myMiddleware')
        .dependsOn('port')
        .initializeWith(function (deps) {
          // perform any initialization required
        });
    }
  ]
}).done(function (cbox) {

  cbox.get('app').done(function (app) {
    app.listen(function () {

    });
  });
});
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Follow the [circuitbox coding conventions](http://github.com/oddjobsman/circuitbox/wiki/Coding-Conventions)
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create new Pull Request