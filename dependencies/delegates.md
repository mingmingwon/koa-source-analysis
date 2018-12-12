# delegates <sup>[![Version Badge](http://versionbadg.es/tj/node-delegates.svg)](https://www.npmjs.com/package/delegates)</sup>

对象方法和访问器代理。

## 安装

```
$ npm install delegates
```

## 示例

```js
var delegate = require('delegates');

...

delegate(proto, 'request')
  .method('acceptsLanguages')
  .method('acceptsEncodings')
  .method('acceptsCharsets')
  .method('accepts')
  .method('is')
  .access('querystring')
  .access('idempotent')
  .access('socket')
  .access('length')
  .access('query')
  .access('search')
  .access('status')
  .access('method')
  .access('path')
  .access('body')
  .access('host')
  .access('url')
  .getter('subdomains')
  .getter('protocol')
  .getter('header')
  .getter('stale')
  .getter('fresh')
  .getter('secure')
  .getter('ips')
  .getter('ip')
```

# API

## Delegate(proto, prop)

创建一个代理器实例，用于配置给定 `proto` 对象（通常是原型对象）的 `prop` 属性。

## Delegate.auto(proto, targetProto, targetProp)

将 getter，setter，values 和方法从 targetProto 代理到 proto 。假设 targetProto 对象属于 proto 对象且具有 targetProp 属性。

## Delegate#method(name)

允许在宿主上访问给定的方法 `name` 。

## Delegate#getter(name)

在被代理对象上为 `name` 属性创建“getter”。

## Delegate#setter(name)

在被代理对象上为 `name` 属性创建“setter”。

## Delegate#access(name)

在被代理对象上为 `name` 属性创建“accessor”（例如：getter 和 setter）。

## Delegate#fluent(name)

一种特殊类型的“accessor”，适用于“fluent”API。当作为 getter 调用时，该方法返回预期值。当作为 setter 调用时，返回自身，因此可以链式调用。例如：

```js
delegate(proto, 'request')
  .fluent('query')

// getter
var q = request.query();

// setter (chainable)
request
  .query({ a: 1 })
  .query({ b: 2 });
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
