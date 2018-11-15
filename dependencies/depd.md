# depd <sup>[![Version Badge](http://versionbadg.es/dougwilson/nodejs-depd.svg)](https://www.npmjs.com/package/depd)</sup>

弃用一切

> With great modules comes great responsibility; mark things deprecated!

## 安装

直接使用 `npm` 安装：

```sh
$ npm install depd
```

还可以使用 [Browserify](http://browserify.org/) 或 [webpack](https://webpack.github.io/) 工具打包，但默认情况下，此模块将更改其 API，以便不再显示或追踪弃用。

## API

```js
var deprecate = require('depd')('my-module')
```

此库可以用来向用户显示弃用信息。

不是仅在弃用函数第一次调用时候发出警告，之后不再警告，而是该模块将在每个不同的调用地方第一次调用时发出警告，这使得它成为跨代码库提示用户废弃内容的理想选择。

弃用警告还包括弃用函数调用所在的文件和行信息。

**注意** 此库具有与 `debug` 模块类似的接口，并且使用调用文件来获取调用堆栈的边界，所以应该总是在每个文件中创建新的 `deprecate` 对象而不是仅在某一些文件中创建。

### depd(namespace)

使用给定的命名空间 `message` 创建一个新的弃用函数，它将显示进入文件该函数被调用的堆栈之前的调用地方。强烈建议使用模块名作为命名空间。

### deprecate(message)

调用此函数以显示弃用信息。每个唯一的调用地方仅显示一次。调用地方是指堆栈中与此调函数调用者不同文件中的第一次调用。

如果省略该信息参数 `message`，则会根据 `deprecate()` 的调用地方生成一个信息，将显示被调用函数的名字，类似于堆栈追踪中显示的名称。

### deprecate.function(fn, message)

调用此函数以包装给定的弃用函数 `fn`，在任何该函数调用时显示弃用信息。可以传递可选信息 `message` 以提供自定义信息。

### deprecate.property(obj, prop, message)

调用此函数以包装给定对象 `obj` 的属性 `prop`，在任何访问或设置该属性时显示弃用信息。可以传递可选信息 `message` 以提供自定义信息。

此方法必须在属性所属的对象上调用（而非从原型继承）。

如果属性是数据描述符，它将被转化为访问器描述符以显示弃用信息。

### process.on('deprecation', fn)

此模块可以轻松的捕获通过全局 `process` 触发的 "deprecation" 类型的错误。
如果没有对此类型的监听器，错误会被正常的写入 STDERR，但如果有，则错误不会被写入 STDERR，而只会被触发。
在监听器里，可以用不同的格式来编写错误信息或写入记录源。

该错误对象表示弃用，并且使用相同的规则写入 STDERR 仅会触发一次。此错误对象具有以下属性：

   - `message` - 库提供的信息
   - `name` - 总是 `'DeprecationError'`
   - `namespace` - 弃用的名称空间
   - `stack` - 弃用的调用堆栈

实例 `error.stack` 输出：

```
DeprecationError: my-cool-module deprecated oldfunction
    at Object.<anonymous> ([eval]-wrapper:6:22)
    at Module._compile (module.js:456:26)
    at evalScript (node.js:532:25)
    at startup (node.js:80:7)
    at node.js:902:3
```

### process.env.NO_DEPRECATION

作为被弃用模块的用户，环境变量 `NO_DEPRECATION` 提供一种快速禁止输出中弃用警告的解决方案。其格式类似于 `DEBUG`：

```sh
$ NO_DEPRECATION=my-module,othermod node app.js
```

这将禁止 "my-module" 和 "othermod" 中的弃用被输出。该值是逗号分隔的命名空间列表。禁止所有命名空间的警告，可以用 `*` 作为命名空间。

向 `node` 可执行文件提供参数 `--no-deprecation` 将禁止所有弃用（仅在 Node.js 0.8 或更高版本中可用）。

**注意** 这不会禁止绑定了 "deprecation" 事件监听器的弃用，只是禁止输出到 STDERR。

### process.env.TRACE_DEPRECATION

作为被弃用模块的用户，环境变量 `TRACE_DEPRECATION` 提供一种通过包含整个调用堆栈记录来获取弃用信息中更多详细定位信息的解决方案。其格式类似于 `NO_DEPRECATION`：

```sh
$ TRACE_DEPRECATION=my-module,othermod node app.js
```

这将包含 "my-module" 和 "othermod" 中被输出的弃用信息堆栈追踪。该值是逗号分隔的命名空间列表。追踪所有命名空间的警告，可以用 `*` 作为命名空间。

向 `node` 可执行文件提供参数 `--trace-deprecation` 将追踪所有弃用（仅在 Node.js 0.8 或更高版本中可用）。

**注意** 这不会追踪对被 `NO_DEPRECATION` 禁止的弃用。

## 显示

![message](https://github.com/dougwilson/nodejs-depd/raw/master/files/message.png)


当用户调用库中标记为已弃用的函数时，将看到以下信息被写入 STDERR（以给定的颜色，和 `debug` 模块的颜色和布局类似）：

```
bright cyan    bright yellow
|              |          reset       cyan
|              |          |           |
▼              ▼          ▼           ▼
my-cool-module deprecated oldfunction [eval]-wrapper:6:22
▲              ▲          ▲           ▲
|              |          |           |
namespace      |          |           location of mycoolmod.oldfunction() call
               |          deprecation message
               the word "deprecated"
```

如果用户将 STDERR 重定向到文件或不支持颜色的环境，他们将看到（与 `debug` 模块的布局类似）：

```
Sun, 15 Jun 2014 05:21:37 GMT my-cool-module deprecated oldfunction at [eval]-wrapper:6:22
▲                             ▲              ▲          ▲              ▲
|                             |              |          |              |
timestamp of message          namespace      |          |             location of mycoolmod.oldfunction() call
                                             |          deprecation message
                                             the word "deprecated"
```

## 示例

### 弃用对函数的所有调用

将在 STDERR 中显示 "my-cool-module" 中有关被弃用函数 "oldfunction" 的弃用信息。（原文中 "my-module" 为笔误，以下示例代码中的命名空间均是 "my-cool-module"，已提 [pull request](https://github.com/dougwilson/nodejs-depd/pull/31)）

```js
var deprecate = require('depd')('my-cool-module')

// message automatically derived from function name
// Object.oldfunction
exports.oldfunction = deprecate.function(function oldfunction () {
  // all calls to function are deprecated
})

// specific message
exports.oldfunction = deprecate.function(function () {
  // all calls to function are deprecated
}, 'oldfunction')
```

### 有条件的弃用函数调用

当调用少于 2 个参数时候，将在 STDERR 中显示 "my-cool-module" 中有关被弃用函数 "weirdfunction" 的弃用信息。

```js
var deprecate = require('depd')('my-cool-module')

exports.weirdfunction = function () {
  if (arguments.length < 2) {
    // calls with 0 or 1 args are deprecated
    deprecate('weirdfunction args < 2')
  }
}
```

当 "deprecate" 作为函数调用时，模块内每个调用地方警告都会被统计，可以根据不同的情况显示不同的弃用信息，用户仍然会收到所有的警告：

```js
var deprecate = require('depd')('my-cool-module')

exports.weirdfunction = function () {
  if (arguments.length < 2) {
    // calls with 0 or 1 args are deprecated
    deprecate('weirdfunction args < 2')
  } else if (typeof arguments[0] !== 'string') {
    // calls with non-string first argument are deprecated
    deprecate('weirdfunction non-string first arg')
  }
}
```

### 弃用属性访问

将在 STDERR 中显示 "my-cool-module" 中有关被弃用属性 "oldprop" 的弃用信息。当设置该值和获取该值时将显示弃用信息。

```js
var deprecate = require('depd')('my-cool-module')

exports.oldprop = 'something'

// message automatically derives from property name
deprecate.property(exports, 'oldprop')

// explicit message
deprecate.property(exports, 'oldprop', 'oldprop >= 0.10')
```

## 译者

[Jordan Wang](https://github.com/mingmingwon/)

<sub>注：原文语句蹩脚不畅，建议阅读示例理解</sub>
