# webpack-config-utils

Utilities to help your webpack config be easier to read

[![Build Status][build-badge]][build]
[![Code Coverage][coverage-badge]][coverage]
[![version][version-badge]][package]
[![downloads][downloads-badge]][npm-stat]
[![MIT License][license-badge]][LICENSE]
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors)

[![PRs Welcome][prs-badge]][prs]
[![Donate][donate-badge]][donate]
[![Code of Conduct][coc-badge]][coc]
[![Roadmap][roadmap-badge]][roadmap]
[![Examples][examples-badge]][examples]

## The problem

Webpack configuration is a JavaScript object which is awesomely declarative. However, often the webpack config file is
can easily turn into an imperative mess in the process of creating the configuration object.

## This solution

The goal of this project is to provide utilities to make it easier to compose your config object so it's easier for
people to read. It has some custom methods and also comes bundled with some other projects to expose some helpful
utility functions.

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and should
be installed as one of your project's `devDependencies`:

```
npm install --save-dev webpack-config-utils
```

## Usage

It is expected that you use this in your `webpack.config.js` file.

> Protip: You can name your config file `webpack.config.babel.js` and it'll be automagically transpiled! So you could
> use ES6 module imports rather than CommonJS requires. But this example will stick to CommonJS because we love you ❤️

```javascript
const webpack = require('webpack')
const {getIfUtils, removeEmpty} = require('webpack-config-utils')

const {ifProduction} = getIfUtils(process.env.NODE_ENV)

module.exports = {
  // ... your config
  plugins: removeEmpty([
    ifProduction(new webpack.optimize.DedupePlugin()),
    ifProduction(new webpack.LoaderOptionsPlugin({
      minimize: true,
      quiet: true,
    })),
    ifProduction(new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"',
      },
    })),
    ifProduction(new webpack.optimize.UglifyJsPlugin({
      compress: {
        screw_ie8: true,
        warnings: false,
      },
    })),
    new HtmlWebpackPlugin({
      template: './index.html',
      inject: 'head',
    }),
    ifProduction(new OfflinePlugin()),
  ]),
}
```

Then you'd invoke the webpack config with [`cross-env`][cross-env] in your `package.json` scripts (or with
[`p-s`][p-s]):

```js
{
  // your package.json stuff
  scripts: {
    "build:dev": "cross-env NODE_ENV=development webpack",
    "build:prod": "cross-env NODE_ENV=production webpack"
  }
}
```

---

Things get even better with webpack 2 because you can write your webpack config as a function that accepts an `env`
argument which you can set on the command line (which means you don't need `cross-env` 👍).

```javascript
const webpack = require('webpack')
const {getIfUtils, here} = require('webpack-config-utils')

module.exports = env => {
  const {ifDev} = getIfUtils(env)
  return {
    output: {
      // etc.
      pathinfo: ifDev(),
      path: here('dist'),
    },
    // etc.
  }
}
```

## API

### `here`

See the package details of [`here`](https://www.npmjs.com/package/here)

### `combineLoaders`

See the package details of [`webpack-combine-loaders`](https://www.npmjs.com/package/webpack-combine-loaders)

### `propIf`

This powers the methods returned from `getIfUtils` and `propIfNot`. It's actually just a simple ternary:

```
return getValue(add) ? value : alternate
```

Where `getValue` simply evaluates the given value

### `propIfNot`



### `removeEmpty`



### `getIfUtils`




## Inspiration



## Other Solutions



## Contributors

Thanks goes to these people ([emoji key][emojis]):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors][all-contributors] specification. Contributions of any kind welcome!

## LICENSE

MIT

[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[build-badge]: https://img.shields.io/travis/kentcdodds/webpack-config-utils.svg?style=flat-square
[build]: https://travis-ci.org/kentcdodds/webpack-config-utils
[coverage-badge]: https://img.shields.io/codecov/c/github/kentcdodds/webpack-config-utils.svg?style=flat-square
[coverage]: https://codecov.io/github/kentcdodds/webpack-config-utils
[version-badge]: https://img.shields.io/npm/v/webpack-config-utils.svg?style=flat-square
[package]: https://www.npmjs.com/package/webpack-config-utils
[downloads-badge]: https://img.shields.io/npm/dm/webpack-config-utils.svg?style=flat-square
[npm-stat]: http://npm-stat.com/charts.html?package=webpack-config-utils&from=2016-04-01
[license-badge]: https://img.shields.io/npm/l/webpack-config-utils.svg?style=flat-square
[license]: https://github.com/kentcdodds/webpack-config-utils/blob/master/other/LICENSE
[prs-badge]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[donate-badge]: https://img.shields.io/badge/$-support-green.svg?style=flat-square
[donate]: http://kcd.im/donate
[coc-badge]: https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]: https://github.com/kentcdodds/webpack-config-utils/blob/master/other/CODE_OF_CONDUCT.md
[roadmap-badge]: https://img.shields.io/badge/%F0%9F%93%94-roadmap-CD9523.svg?style=flat-square
[roadmap]: https://github.com/kentcdodds/webpack-config-utils/blob/master/other/ROADMAP.md
[examples-badge]: https://img.shields.io/badge/%F0%9F%92%A1-examples-8C8E93.svg?style=flat-square
[examples]: https://github.com/kentcdodds/webpack-config-utils/blob/master/other/EXAMPLES.md
[emojis]: https://github.com/kentcdodds/all-contributors#emoji-key
[all-contributors]: https://github.com/kentcdodds/all-contributors
[cross-env]: https://www.npmjs.com/package/cross-env
[p-s]: https://www.npmjs.com/package/p-s
