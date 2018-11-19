# Destroy <sup>[![Version Badge](http://versionbadg.es/stream-utils/destroy.svg)](https://www.npmjs.com/package/destroy)</sup>

销毁流。

此模块旨在确保流被销毁，处理不同的 API 和 Node.js bug。

## API

```js
var destroy = require('destroy')
```

### destroy(stream)

销毁给定的流。在大多数情况下，这与简单的 `stream.destroy()` 调用相同。给定流的规则如下：

  1. 如果 `stream` 是 `ReadStream` 实例，那么调用 `stream.destroy()`，并给 `open` 事件添加一个监听器，当其被触发时，调用 `stream.close()` 。这是针对 Node.js 的 bug，如果在 `open` 之前调用 `.destroy()` 将会泄露文件描述符。
  2. 如果 `stream` 不是 `Stream` 实例，则什么都不做。
  3. 如果 `stream` 拥有 `.destroy()` 方法，则调用它。

此函数返回传入的参数 `stream` 。

## 示例

```js
var destroy = require('destroy')

var fs = require('fs')
var stream = fs.createReadStream('package.json')

// ... and later
destroy(stream)
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)

<sub>亦可参考阮大神的 [Stream](http://javascript.ruanyifeng.com/nodejs/stream.html#toc26) 教程</sub>