# lslint-loader [![Build Status](http://img.shields.io/travis/kryptt/lslint-loader.svg)](https://travis-ci.org/kryptt/lslint-loader)

> lslint loader for webpack

## Install

```console
$ npm install lslint-loader
```

## Usage

In your webpack configuration

```javascript
module.exports = {
  // ...
  module: {
    loaders: [
      {test: /\.ls$/, loader: "lslint-loader", exclude: /node_modules/}
    ]
  }
  // ...
}
```

When using with transpiling loaders (like `babel-loader`), make sure they are in correct order
(bottom to top). Otherwise files will be check after being processed by `babel-loader`

```javascript
module.exports = {
  // ...
  module: {
    loaders: [
      {test: /\.js$/, loader: "babel-loader", exclude: /node_modules/}
      {test: /\.js$/, loader: "lslint-loader", exclude: /node_modules/}
    ]
  }
  // ...
}
```

To be safe, you can use `preLoaders` section to check source files, not modified
by other loaders (like `babel-loader`)

```js
module.exports = {
  // ...
  module: {
    preLoaders: [
      {test: /\.js$/, loader: "lslint-loader", exclude: /node_modules/}
    ]
  }
  // ...
}
```

### Options

You can pass directly some [lslint options](http://lslint.org/docs/user-guide/command-line-interface) by

- Adding a query string to the loader for this loader usabe only

```js
{
  module: {
    preLoaders: [
      {
        test: /\.js$/,
        loader: "lslint-loader?{rules:[{semi:0}]}",
        exclude: /node_modules/,
      },
    ],
  },
}
```

- Adding an `lslint` entry in you webpack config for global options:

```js
module.exports = {
  lslint: {
    configFile: 'path/.lslintrc'
  }
}
```

**Note that you can use both method in order to benefit from global & specific options**

#### `formatter` (default: lslint stylish formatter)

Loader accepts a function that will have one argument: an array of lslint messages (object).
The function must return the output as a string.
You can use official lslint formatters.

```js
module.exports = {
  entry: "...",
  module: {
    // ...
  }
  lslint: {
    allow-class: no,
    allow-new: no,
    allow-return: no,
    allow-throw: no,
    allow-break: no,
    allow-continue: no,
    allow-while: no,
    allow-case: yes,
    allow-default: no,
    allow-null: no,
    allow-void: no,
    allow-this: no,
    allow-delete: no,
    allow-eval: no,
    enforce-pascal-case-class-name: yes
  }
}
```

#### Errors and Warning

**By default the loader will auto adjust error reporting depending
on lslint errors/warnings counts.**
You can still force this behavior by using `emitError` **or** `emitWarning` options:

##### `emitError` (default: `false`)

Loader will always return errors if this option is set to `true`.

```js
module.exports = {
  entry: "...",
  module: {
    // ...
  }
  lslint: {
    emitError: true
  }
}
```

##### `emitWarning` (default: `false`)

Loader will always return warnings if option is set to `true`.

#### `quiet` (default: `false`)

Loader will process and report errors only and ignore warnings if this option is set to true

```js
module.exports = {
  entry: "...",
  module: {
    // ...
  }
  lslint: {
    quiet: true
  }
}
```

##### `failOnWarning` (default: `false`)

Loader will cause the module build to fail if there are any lslint warnings.

```js
module.exports = {
  entry: "...",
  module: {
    // ...
  }
  lslint: {
    failOnWarning: true
  }
}
```

##### `failOnError` (default: `false`)

Loader will cause the module build to fail if there are any lslint errors.

```js
module.exports = {
  entry: "...",
  module: {
    // ...
  }
  lslint: {
    failOnError: true
  }
}
```

## Gotchas

### NoErrorsPlugin

`NoErrorsPlugin` prevents Webpack from outputting anything into a bundle. So even Lslint warnings
will fail the build. No matter what error settings are used for `lslint-loader`.

So if you want to see Lslint warnings in console during development using `WebpackDevServer`
remove `NoErrorsPlugin` from webpack config.

## [Changelog](CHANGELOG.md)

## [License](LICENSE)
