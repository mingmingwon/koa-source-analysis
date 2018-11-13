# koa-convert <sup>[![Version Badge](http://versionbadg.es/koajs/convert.svg)](https://www.npmjs.com/package/koa-convert)</sup>

将 Koa 旧版（0.x 和 1.x）generator 中间件转换为现代版 promise 中间件（2.x）。

它还可以将现代版 promise 中间件转换回旧版的 generator 中间件（有助于帮助现代版中间件支持 koa 0.x 或 1.x）。

## 注意

路由器中间件是特例。因为它在内部重新实现了中间件组合，所以我们不能简单地转换它。

可以使用下面的包实现路由，它们已适用于 koa 2.x 版本：

* [koa-route@3.0.0](https://github.com/koajs/route/tree/next)
* [koa-simple-router](https://github.com/gyson/koa-simple-router)
* [koa-router@next](https://github.com/alexmingoia/koa-router/tree/master)
* [koa-66](https://github.com/menems/koa-66)

## 安装

```
$ npm install koa-convert
```

## 使用

```js
const Koa = require('koa') // koa v2.x
const convert = require('koa-convert')
const app = new Koa()

app.use(modernMiddleware)

app.use(convert(legacyMiddleware))

app.use(convert.compose(legacyMiddleware, modernMiddleware))

function * legacyMiddleware (next) {
  // before
  yield next
  // after
}

function modernMiddleware (ctx, next) {
  // before
  return next().then(() => {
    // after
  })
}
```

## 区分旧版和新版中间件

在 koa 0.x 和 1.x（没有实验标志）中 `app.use` 有一个断言，即所有（旧版）中间件必须是 generator 函数，并使用 `fn.constructor.name =='GeneratorFunction'` 进行检测，代码在 [这里](https://github.com/koajs/koa/blob/7fe29d92f1e826d9ce36029e1b9263b94cba8a7c/lib/application.js#L105)。

因此，可以通过 `fn.constructor.name == 'GeneratorFunction'` 来区分旧版和现代版中间件。

## 迁移

`app.use(legacyMiddleware)` 在 0.x 和 1.x 中无处不在（意思是很多地方使用），将它们全部手动更改为 `app.use(convert(legacyMiddleware))` 会很痛苦。

可以使用以下代码片段来简化迁移。

```js
const _use = app.use
app.use = x => _use.call(app, convert(x))
```

上面的代码片段将覆盖 `app.use` 方法，隐式的将所有的旧版 generator 中间件转化为现代版 promise 中间件。

因此，在 0.x 和 1.x 中可以同时拥有 `app.use(modernMiddleware)` 和 `app.use(legacyMiddleware)` ，而无需修改即可使用。

Complete example:

```js
const Koa = require('koa') // v2.x
const convert = require('koa-convert')
const app = new Koa()

// ---------- override app.use method ----------

const _use = app.use
app.use = x => _use.call(app, convert(x))

// ---------- end ----------

app.use(modernMiddleware)

// this will be converted to modern promise middleware implicitly
app.use(legacyMiddleware)

function * legacyMiddleware (next) {
  // before
  yield next
  // after
}

function modernMiddleware (ctx, next) {
  // before
  return next().then(() => {
    // after
  })
}
```

## API

#### `convert()`

将旧版 generator 中间件转换为现代版 promise 中间件。

```js
modernMiddleware = convert(legacyMiddleware)
```

#### `convert.compose()`

转换和组合多个中间件（可以混合旧版和现代版）并返回现代版 promise 中间件。

```js
composedModernMiddleware = convert.compose(legacyMiddleware, modernMiddleware)
// or
composedModernMiddleware = convert.compose([legacyMiddleware, modernMiddleware])
```

#### `convert.back()`

将现代版 promise 中间件转换为旧版 generator 中间件。

有助于帮助现代版 promise 中间件支持 koa 0.x 或 1.x。

```js
legacyMiddleware = convert.back(modernMiddleware)
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
