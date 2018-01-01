# Koa tree router

[![Build Status](https://travis-ci.org/steambap/koa-tree-router.svg?branch=master)](https://travis-ci.org/steambap/koa-tree-router)

Koa tree router is a high performance router for Koa.

## Features

- Fast. Up to 12 times faster than Koa-router. [Benchmark](benchmark/index.js)

- Express routing using `router.get`, `router.put`, `router.post`, etc.

- Support for 405 method not allowed

- Multiple middlewares per route

## How does it work?

The router relies on a tree structure which makes heavy use of *common prefixes*, it is basically a *compact* [*prefix tree*](https://en.wikipedia.org/wiki/Trie) (or just [*Radix tree*](https://en.wikipedia.org/wiki/Radix_tree)).

This module's tree implementation is based on [julienschmidt/httprouter](https://github.com/julienschmidt/httprouter).

## Usage

```JS
const Koa = require("koa");
const Router = require("koa-tree-router");

const app = new Koa();
const router = new Router();
router.get("/", function(ctx) {
  ctx.body = "hello, world";
});

app.use(router.routes());

app.listen(8080);
```

## API

#### Router([options])
Instance a new router.  
You can pass a middleware with the option `onMethodNotAllowed`.
```js
const router = require('koa-tree-router')({
  onMethodNotAllowed(ctx){
    ctx.body = "not allowed"
  }
})
```

#### on(method, path, middleware)
Register a new route.
```js
router.on('GET', '/example', (ctx) => {
  // your code
})
```

#### Shorthand methods
If you want tp get expressive, here is what you can do:
```js
router.get(path, middleware)
router.delete(path, middleware)
router.head(path, middleware)
router.patch(path, middleware)
router.post(path, middleware)
router.put(path, middleware)
router.options(path, middleware)
router.trace(path, middleware)
router.connect(path, middleware)
```

If you need a route that supports *all* methods you can use the `all` api.
```js
router.all(path, middleware)
```

#### routes
Returns router middleware.

```JS
app.use(router.routes());
```

#### ctx.params
This object contains key-value pairs of named route parameters.

```JS
router.get("/user/:name", function() {
  // your code
});
// GET /user/1
ctx.params.name
// => "1"
```