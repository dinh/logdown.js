<img src="http://rawgit.com/caiogondim/logdown.js/master/img/icon.svg">

<h1 align="center">logdown.js</h1>

<div align="center">
<img src="http://travis-ci.org/caiogondim/logdown.js.svg?branch=master" alt="Travis CI"> <img src="http://img.badgesize.io/caiogondim/logdown.js/master/dist/logdown.min.js?compression=gzip" alt="lib size"> <img src="http://img.shields.io/npm/dm/logdown.svg" alt="Downloads per month">
</div>

<br>

#### THIS VERSION (v3) IS UNDER DEVELOPMENT

`logdown` is a debug utility for the browser and the server with Markdown support, providing a single interface and a similar behavior between the browser and the server.

It doesn't have any dependencies for the browser version and it is less than 2K gzipped.

You can see it in action in the [example page](//caiogondim.github.io/logdown.js) or in the preview
below.

## Installation

```bash
$ npm install --save logdown
```

## Preview

Out-of-the box colors work well on both light and dark themes.

<h3 align="center">Browser DevTools dark</h3>
<img src="http://rawgit.com/caiogondim/logdown.js/master/img/browser-preview-dark.png">

<h3 align="center">Browser DevTools light</h3>
<img src="http://rawgit.com/caiogondim/logdown.js/master/img/browser-preview-light.png">

<h3 align="center">Node</h3>
<img src="http://rawgit.com/caiogondim/logdown.js/master/img/node-preview.png">

## Usage

`logdown` exports a function. For the simplest use case, pass the name of your module and
it will return a decorated `console`.

```js
const logdown = require('logdown')
const logger = logdown('foo')
```

Or in a more idiomatic way:

```js
const logger = require('logdown')('foo')
```

Just like [debug.js](https://github.com/visionmedia/debug) and node core's [debuglog](https://nodejs.org/docs/latest/api/util.html#util_util_debuglog_section), the enviroment variable `NODE_DEBUG` is used to decide which
module will print debug information.

```js
$ NODE_DEBUG=foo node example/node.js
```

### Logging

After creating your object, you can use the regular `log`, `warn`, `info` and `error` methods as we
have on `console`, but now with Markdown support. If a method is not provided by `logdown`, it will
just delegate to the original `console` object or `opts.logger` if passed.

```js
logger.log('lorem *ipsum*')
logger.info('dolor _sit_ amet')
logger.warn('consectetur `adipiscing` elit')
```

As the native APIs, multiple arguments are supported.

```js
logger.log('lorem', '*ipsum*')
logger.info('dolor _sit_', 'amet')
logger.warn('consectetur', '`adipiscing` elit')
```

### Options

The following options can be used for configuration.

#### `prefix`

- Type: `String`

```js
const logger = logdown('foo')
logger.log('Lorem ipsum')
```

```js
var fooBarLogger = logdown('foo:bar')
fooBarLogger.log('Lorem ipsum')

var fooQuzLogger = logdown('foo:quz')
fooQuzLogger.log('Lorem Ipsum')
```

#### `markdown`

- Type: `Boolean`
- Default: `true`

If setted to `false`, markdown will not be parsed.

```js
var logger = logdown({ markdown: false })
logger.log('Lorem *ipsum*') // Will not parse the markdown
```

For Markdown, the following mark-up is supported:

```js
// Bold with "*"" between words
logger.log('lorem *ipsum*')

// Italic with "_" between words
logger.log('lorem _ipsum_')

// Code with ` (backtick) between words
logger.log('lorem `ipsum`')
```

#### `prefixColor`

- type: `String`
- default: next value after last used on the `logdown.prefixColors` array.

Hex value for a custom color.

```js
const logger1 = logdown('foo', { prefixColor: '#FF0000' }) // red prefix
const logger2 = logdown('bar', { prefixColor: '#00FF00' }) // green prefix
const logger3 = logdown('quz', { prefixColor: '#0000FF' }) // blue prefix
```

#### `logger`

- type: 'Object'
- default: `console`

Custom logger. On Node it's possible to instantiate a new `console` setting it's output to a
different stream other than `stdout` and `stderr`.

```js
const output = fs.createWriteStream('./stdout.log');
const errorOutput = fs.createWriteStream('./stderr.log');
const fileLogger =  new Console(output, errorOutput)

const logger = logdown('foo', {
  logger: fileLogger
})
```

## State

### `isEnabled`

- type: 'Boolean'
- default: default value is derived from `localStorage.debug` on browser and from env var `NODE_DEBUG` on node.

Used to enable/disable a given instance at runtime.

```js
// Prevents `logger` to output debug info
logger.state.isEnabled = false
```

## Enabling/disabling instances

`logdown` is compatible with Node.js
[util.debuglog](https://nodejs.org/docs/latest/api/util.html#util_util_debuglog_section) and
[debug.js](https://github.com/visionmedia/debug) as it uses the `NODE_DEBUG` enviroment variable to
control which instances are enabled to output debug info.

For the browser use `localStorage.debug`.

```bash
NODE_DEBUG=foo node foo.js # will disable the instance with *foo* prefix
```

```js
localStorage.debug = 'foo'
```

Multiple instances should be separated by comma

```bash
NODE_DEBUG=foo,bar node foo.js # will disable the instance with *foo* prefix
```

```js
localStorage.debug = 'foo,bar'
```

### Wildcards

Wildcard `*` is supported.

```bash
NODE_DEBUG=* node foo.js # enables all instances
NODE_DEBUG=foo* node foo.js # enables all instances with a prefix starting with *foo*
```

Use `-` to do a negation.

```bash
# enables all instances but the one with *foo* prefix
NODE_DEBUG=*,-foo node foo.js

# disables all intances with foo in the prefix, but don't disable *foobar*
NODE_DEBUG=*foo*,-foobar node foo.js
```

## Conventions

If you're using this in one or more of your libraries, you should use the name of your library so
that developers may toggle debugging as desired without guessing names. If you have more than one
debuggers you should prefix them with your library name and use ":" to separate features. For
example "bodyParser" from Connect would then be "connect:bodyParser".

## FAQ

[Link](doc/faq.md)

## Sponsor

If you found this library useful and are willing to donate, transfer some
bitcoins to `1BqqKiZA8Tq43CdukdBEwCdDD42jxuX9UY`.

## Credits

- [debug.js](https://github.com/visionmedia/debug) for some pieces of copied documentation
- [Node core's debuglog](https://nodejs.org/docs/latest/api/util.html#util_util_debuglog_section)

---

[caiogondim.com](https://caiogondim.com) &nbsp;&middot;&nbsp;
GitHub [@caiogondim](https://github.com/caiogondim) &nbsp;&middot;&nbsp;
Twitter [@caio_gondim](https://twitter.com/caio_gondim)
