<p align="center">
  <img alt="6to5" src="http://i.imgur.com/hVl9KRw.png">
</p>

<p align="center">
  <a href="https://travis-ci.org/sebmck/6to5">
    <img alt="Travis Status" src="http://img.shields.io/travis/sebmck/6to5.svg?style=flat&amp;label=travis">
  </a>

  <a href="https://codeclimate.com/github/sebmck/6to5">
    <img alt="Code Climate Score" src="http://img.shields.io/codeclimate/github/sebmck/6to5.svg?style=flat">
  </a>

  <a href="https://codeclimate.com/github/sebmck/6to5">
    <img alt="Coverage" src="http://img.shields.io/codeclimate/coverage/github/sebmck/6to5.svg?style=flat">
  </a>

  <a href="https://david-dm.org/sebmck/6to5">
    <img alt="Dependency Status" src="http://img.shields.io/david/sebmck/6to5.svg?style=flat">
  </a>
</p>

**6to5** turns ES6 code into vanilla ES5, so you can use ES6 features **today.**

 - **Fast** - no redundant code added so your compiled code is as fast as possible.
 - **Extensible** - with a large range of [plugins](#plugins).
 - **Lossless** - **source map support** so you can debug your compiled code with ease.
 - **Compact** - maps directly to the equivalent ES5 with **no runtime**.
 - **Concise** - does not pollute scope with unneccesary variables.

## Installation

It's as easy as:

    $ npm install -g 6to5

## Plugins

 - [Browserify](https://github.com/sebmck/6to5-browserify)
 - [Brunch](https://github.com/es128/6to5-brunch)
 - [Connect](https://github.com/sebmck/6to5-connect)
 - [Gulp](https://github.com/sindresorhus/gulp-6to5)
 - [Grunt](https://github.com/sindresorhus/grunt-6to5)

## [Features](FEATURES.md)

 - [Array comprehension](FEATURES.md#array-comprehension)
 - [Arrow functions](FEATURES.md#arrow-functions)
 - [Block binding](FEATURES.md#block-binding)
 - [Classes](FEATURES.md#classes)
 - [Computed property names](FEATURES.md#computed-property-names)
 - [Constants](FEATURES.md#constants)
 - [Default parameters](FEATURES.md#default-parameters)
 - [Destructuring](FEATURES.md#destructuring)
 - [For-of](FEATURES.md#for-of)
 - [Modules](FEATURES.md#modules)
 - [Numeric literals](FEATURES.md#numeric-literals)
 - [Property method assignment](FEATURES.md#property-method-assignment)
 - [Property name shorthand](FEATURES.md#property-name-shorthand)
 - [Rest parameters](FEATURES.md#rest-parameters)
 - [Spread](FEATURES.md#spread)
 - [Template literals](FEATURES.md#template-literals)

To be implemented:

 - [Generators](FEATURES.md#generators)

## Usage

### CLI

Compile the file `script.js` and output it to stdout.

    $ 6to5 script.js

Compile the file `script.js` and output it to `script-compiled.js`.

    $ 6to5 script.js --out-file script-compiled.js

Compile the file `script.js` and output it to `script-compiled.js` and save a
source map to `script-compiled.js.map`.

    $ 6to5 script.js --source-maps --out-file script-compiled.js

Compile the file `script.js` and output it to `script-compiled.js` with a source
map embedded in a comment at the bottom.

    $ 6to5 script.js --source-maps-comment --out-file script-compiled.js

Compile the entire `src` directory and output it to the `lib` directory.

    $ 6to5 src --out-dir lib

#### Node

Launch a repl.

    $ 6to5-node

Evaluate code.

    $ 6to5-node -e "class Test { }"

Compile and run `test.js`.

    $ 6to5-node test

### Node

```javascript
var to5 = require("6to5");

to5.transform("code();", options);

to5.transformFileSync("filename.js", options);

to5.transformFile("filename.js", options, function (err, data) {

});
```

##### Options

```javascript
{
  // List of transformers to EXCLUDE
  // This is a camelised version of the name found in `features`
  // eg. "Arrow functions" is "arrowFunctions"
  blacklist: [],

  // List of transformers to ONLY use.
  // See `blacklist` for naming scheme.
  whitelist: [],

  // Append source map and comment to bottom of returned output.
  sourceMap: false,

  // Returns an object `{ code: "", map: {} }` instead of an appended string.
  sourceMapObject: false,

  // Filename for use in errors etc.
  filename: "unknown",

  // Format options
  // See https://github.com/Constellation/escodegen/wiki/API for options.
  format: {}
}
```

#### Require hook

All subsequent files required by node will be transformed by 6to5. An ES6
polyfill is also required negating the [polyfill caveat](#polyfill).

```javascript
require("6to5/register");
```

## Caveats

### Polyfill

6to5 does not include a runtime nor polyfill and it's up to the developer to
include one in compiled browser code.

A polyfill is included with 6to5 code that can be included in node like so:

```javascript
require("6to5/polyfill");
```

This is simply a wrapper around the
[es6-shim](https://github.com/paulmillr/es6-shim) and
[es6-symbol](https://github.com/medikoo/es6-symbol) polyfills.

When using the [require hook](#require-hook) the aforementioned polyfill is
automatically required.

If you're planning on using 6to5 output in the browser then it's up to you
to include polyfills. [es6-symbol](https://github.com/medikoo/es6-symbol#browser)
and [es6-shim](https://raw.githubusercontent.com/paulmillr/es6-shim/master/es6-shim.js)
fill the vast majority of polyfill concerns.

#### For-of

A polyfill is required for for-of functionality that implements `Symbol` and
adds `prototype[Symbol.iterator]` behaviour to built-ins. Using the polyfills
specified in [polyfill](#polyfill) suffices.

### Classes

Built-in classes such as `Date`, `Array` and `DOM` cannot be subclassed due to
limitations in ES5 implementations.

## Comparison to other transpilers

|                              | 6to5 | Traceur | esnext | es6now | es6-transpiler |
| ---------------------------- | ---- | ------- | ------ | ------ | -------------- |
| No runtime                   | ✓    |         |        |        | ✓              |
| Source maps                  | ✓    | ✓       | ✓      |        | ✓              |
| No compiler global pollution | ✓    |         | ✓      |        | ✓              |
| Array comprehension          | ✓    | ✓       | ✓      |        | ✓              |
| Arrow functions              | ✓    | ✓       | ✓      | ✓      | ✓              |
| Block binding                | ✓    | ✓       |        |        | ✓              |
| Classes                      | ✓    | ✓       | ✓      | ✓      | ✓              |
| Computed property names      | ✓    | ✓       | ✓      | ✓      | ✓              |
| Constants                    | ✓    | ✓       |        |        | ✓              |
| Default parameters           | ✓    | ✓       | ✓      | ✓      | ✓              |
| Destructuring                | ✓    | ✓       | ✓      | ✓      | ✓              |
| For-of                       | ✓    | ✓       | ✓      | ✓      | ✓              |
| Generator comprehension      |      | ✓       | ✓      |        | ✓              |
| Generators                   |      | ✓       | ✓      |        |                |
| Modules                      | ✓    | ✓       |        | ✓      |                |
| Property method assignment   | ✓    | ✓       | ✓      | ✓      | ✓              |
| Property name shorthand      | ✓    | ✓       | ✓      | ✓      | ✓              |
| Rest parameters              | ✓    | ✓       | ✓      | ✓      | ✓              |
| Spread                       | ✓    | ✓       | ✓      | ✓      | ✓              |
| Template literals            | ✓    | ✓       | ✓      | ✓      | ✓              |

#### Performance

To run benchmarks against various transpilers run:

    $ make bench

The following graphs are using the
[full benchmark fixture](https://github.com/sebmck/6to5/blob/master/benchmark/fixtures/all.js).

##### Output size (including runtime)

Lower is better.

![Output size graph](http://i.imgur.com/cHMdnM9.png)

##### Compile speed

Higher is better.

![Compile speed graph](http://i.imgur.com/YYx2R8O.png)

### [Traceur](https://github.com/google/traceur-compiler)

Traceur requires quite a bulky runtime (~75KB) and produces quite verbose code.
While this can be trimmed down by selectively building the runtime, it's an
unneccesary step when a runtime can be eliminated entirely.

### [esnext](https://github.com/esnext/esnext)

esnext is **slow**, painfully so. See [performance](#performance) for stats.

### [es6now](https://github.com/zenparsing/es6now)

es6now doesn't output sourcemaps. This is cited as a positive as line-to-line
mapping is the goal. This however obviously doesn't retain column mapping
resulting in the output code not being very pleasant.

### [es6-transpiler](https://github.com/termi/es6-transpiler)

The es6-transpiler compiler requires shims to operate which pollutes the global
scope resulting in possible collisions.

es6-transpiler maps line-by-line, just like es6now, this results in the same
issues such as lack of column information and unpleasant code output.
