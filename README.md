Express middleware to provide consistently parseable query parameters to universal applications.
====================

[![Build Status][travis-image]][travis-url]

[![Dependency Status][gemnasium-image]][gemnasium-url]
[![NPM version][npm-version-image]][npm-url]
[![NPM downloads][npm-downloads-image]][npm-url]
[![MIT License][license-image]][license-url]
[![Sauce Test Status][saucelabs-image]][saucelabs-url]


# Introduction

Normally, query parsing is handled by your server. For universal applications using something like `react-router`, this can be a problem when a different path is taken to parse query strings on the client vs the server.

For example, a Node express server will parse a malformed query like `?key=foo%%` using the native Node.js `querystring` module to `{ key: 'foo%%' }`. However, a client using `decodeURIComponent` will throw an error like `URIError: URI malformed` when encountering the same query.

This middleware solves this problem by validating each query parameter via `decodeURIComponent` and providing a callback to take action on the server when invalid params are detected. The default behavior when no callback is provided is to log and then redirect to the same path with unparseable params removed.

This package is tested in *both* Node.js and browsers to ensure the same behavior in both runtimes.

# Usage

```es6
import express from 'express';
import queryValidator from 'express-universal-query-validator';

const app = express();

// Mount the middleware
app.use(queryValidator());

// Other route handlers
// ...

app.listen(process.env.PORT);
console.info(`Server listening on ${process.env.PORT} and handling invalid query parameters`);

```

## Configuration

[`queryValidator`](API.md/# queryValidator) accepts a callback that is executed when invalid params are detected, and a configuration object.

See [API.md](API.md) for detailed documentation.

## Examples

```es6

function invalidParamHandler(req, res, next, context) {
    const { error, oldQuery, nextQuery, droppedParams } = context;

    // Do what you wish
}

app.use(queryValidator(invalidParamHandler));

```


# Development

In lieu of a formal style guide, please ensure PRs follow the conventions present, and have been properly linted and tested. Feel free to open issues to discuss.

Be aware this module is tested in both browser and node runtimes.

## Available tasks

### Build and test
Runs all tests, static analysis, and bundle for distribution
```shell
$ npm start
```

### Test
Runs browser and node tests
```shell
$ npm test
```

Runs browser tests via PhantomJS only
```shell
$ npm run test:browser
```

Runs browser tests via SauceLabs only
```shell
$ SAUCELABS=true npm run test:browser
```

Runs node tests only
```shell
$ npm run test:node
```

### Docs
Regenerate `API.md` docs from JSDoc comments
```shell
$ npm run docs
```



[npm-url]: https://npmjs.org/package/express-universal-query-validator
[npm-version-image]: http://img.shields.io/npm/v/express-universal-query-validator.svg?style=flat-square
[npm-downloads-image]: http://img.shields.io/npm/dm/express-universal-query-validator.svg?style=flat-square

[gemnasium-image]: https://img.shields.io/gemnasium/wework/express-universal-query-validator.svg?style=flat-square
[gemnasium-url]: https://gemnasium.com/wework/express-universal-query-validator

[travis-url]:https://travis-ci.org/wework/express-universal-query-validator
[travis-image]: https://img.shields.io/travis/wework/express-universal-query-validator?style=flat-square

[saucelabs-image]:https://saucelabs.com/browser-matrix/wework-query-valid.svg
[saucelabs-url]:https://saucelabs.com/u/wework-query-valid

[license-url]: LICENSE
[license-image]: http://img.shields.io/badge/license-MIT-000000.svg?style=flat-square

