Transpiles JavaScript modules from one format to another.

It supports from:
 
 - es6, 
 - cjs, 
 - amd, 
 - steal
 
to 

 - amd, 
 - steal, 
 - cjs.

Currently, it can not transpile to ES6 module syntax.

## Install

    > npm install transpile --save-dev

## Use

`transpile.to` transpiles from one format to another format. `transpile.able`
lets you know if a transpile is possible.

### Formats

Formats are specified by strings like:

 - "es6" - ES6 Module syntax like `import Point from "math";`
 - "cjs" - CommonJS syntax like `var _ = require('underscore');`
 - "amd" - [Asynchronous Module Definition](https://github.com/amdjs/amdjs-api/wiki/AMD) 
         syntax like `define(['jquery'],function($){});`
 - "steal" - steal syntax like `steal('jquery', function($){})`


### `transpile.to(load, format) -> transpiledSource`

Transpiles from the `load`'s format to the specified format. If
the `load` does not specify a format, `"es6"` modules are assumed. Returns
the transpiled source.

Example:

```js
var transpile = require('transpile');
var res = transpile.to({
  name: "my/module",
  source: "var foo = require('foo')",
  metdata: {format: "cjs"}
}, "amd")

res //-> "define("my/module", function(require, exports, module) { ... "
```
    
A load is an object in the shape of 
an [ES6 Load Record](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-load-records-and-loadrequest-objects) like:

```js
{
  name: "moduleName",
  source: "source code",
  metadata: {format: "formatName"}
}
```

### `transpile.able(fromFormat, toFormat) -> transpiledPath`

Returns the path used to transpile 
from `fromFormat` to `toFormat`. If transpiling is not possible, `null` will be
returned.

Example:

```js
var res = transpile.able("steal","cjs");
res //-> ["steal","amd"];
```

This means that a module will be converted from "steal" to "amd" and then
to "cjs".


## Test

Install mocha globally:

    > npm install -g mocha
    
Run:

	> mocha