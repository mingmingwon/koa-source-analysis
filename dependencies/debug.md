# debug <sup>[![Version Badge](http://versionbadg.es/visionmedia/debug.svg)](https://www.npmjs.com/package/debug)</sup>

<img src="https://user-images.githubusercontent.com/71256/29091486-fa38524c-7c37-11e7-895f-e7ec8e1039b6.png" width="647">

一个微型的 JavaScript 调试工具，以 Node.js 核心的调试技术为模型。适用于 Node.js 和 Web Browser。

## 安装

```bash
$ npm install debug
```

## 使用

`debug` 暴露了一个函数；只需将模块名称传给此函数，它将返回一个对 `console.error` 封装后的函数，用以传递调试语句。这将允许你切换模块的不同部分以及整个模块的调试输出。

示例 [app.js](https://github.com/visionmedia/debug/blob/master/examples/node/app.js)：

```js
var debug = require('debug')('http')
  , http = require('http')
  , name = 'My App';

// fake app

debug('booting %o', name);

http.createServer(function(req, res){
  debug(req.method + ' ' + req.url);
  res.end('hello\n');
}).listen(3000, function(){
  debug('listening');
});

// fake worker of some kind

require('./worker');
```

示例 [worker.js](https://github.com/visionmedia/debug/blob/master/examples/node/_worker.js)：

```js
var a = require('debug')('worker:a')
  , b = require('debug')('worker:b');

function work() {
  a('doing lots of uninteresting work');
  setTimeout(work, Math.random() * 1000);
}

work();

function workb() {
  b('doing some work');
  setTimeout(workb, Math.random() * 2000);
}

workb();
```

使用 `DEBUG` 环境变量启用这些以空格或逗号分隔的名字。

这里有些例子：

<img src="(https://user-images.githubusercontent.com/71256/29091703-a6302cdc-7c38-11e7-8304-7c0b3bc600cd.png" width="647">
<img src="https://user-images.githubusercontent.com/71256/29091700-a62a6888-7c38-11e7-800b-db911291ca2b.png" width="647">
<img src="https://user-images.githubusercontent.com/71256/29091701-a62ea114-7c38-11e7-826a-2692bedca740.png" width="647">

### Windows 命令提示符说明

##### CMD

在 Windows 上，使用 `set` 命令设置环境变量。

```cmd
set DEBUG=*,-not_this
```

例子：

```cmd
set DEBUG=* & node app.js
```

##### PowerShell (Visual Studio Code 默认使用)

PowerShell 设置环境变量的语法和 CMD 不同。

```cmd
$env:DEBUG="*,-not_this"
```

例子：

```cmd
$env:DEBUG='app';node app.js
```

然后，正常运行要调试的程序。

npm 脚本例子：

```js
"windowsDebug": "@powershell -Command $env:DEBUG='*';node app.js",
```

## 命名空间颜色

每个调试实例都有一个根据其命名空间生成的颜色。这样有助于在可视化解析调试输出时识别调试行属于哪个调试实例。

#### Node.js

在 Node.js 中，stderr 是 TTY 时启用颜色。安装 debug 模块的同时，也应该安装 [`supports-color`](https://npmjs.org/supports-color) 模块，否则调试输出只会使用少量的基本颜色。

<img src="https://user-images.githubusercontent.com/71256/29092181-47f6a9e6-7c3a-11e7-9a14-1928d8a711cd.png" width="647">

#### Web Browser

在 Web 解析器能解析 `%c` 格式化选项时启用颜色。解析器包括 WebKit，Firefox（[31 版本以上](https://hacks.mozilla.org/2014/05/editable-box-model-multiple-selection-sublime-text-keys-much-more-firefox-developer-tools-episode-31/)）和 Firefox（任何版本）的 Firebug 插件。

<img src="https://user-images.githubusercontent.com/71256/29092033-b65f9f2e-7c39-11e7-8e32-f6f0d8e865c1.png" width="647">

## 毫秒差

在开发应用程序时，查看一个 `debug()` 调用和下一个 `debug()` 调用之间的耗时是很有用的。假设在请求资源之前调用一下 `debug()`，在得到资源之后也调用一下 `debug()`，那么 `+NNNms` 表示请求资源所消耗的时间。

<img src="https://user-images.githubusercontent.com/71256/29091486-fa38524c-7c37-11e7-895f-e7ec8e1039b6.png" width="647">

当 stdout 不是 TTY 时，使用 `Date#toISOString()` 对于记录调试信息更有用，如下所示：

<img src="https://user-images.githubusercontent.com/71256/29091956-6bd78372-7c39-11e7-8c55-c948396d6edd.png" width="647">

## 规则

如果在一个或多个库中使用它，则应使用库的名称，以便开发人员可以根据需要切换调试而无需猜测名称。如果有多个调试器，则应在其前面加上库的名称，并使用 `:` 来区分功能。例如，Connect 中的 "bodyParser" 将是 "connect:bodyParser"。如果在名称的末尾添加 "*"，则无论 DEBUG 环境变量的设置如何，都将始终启用它。可以将其用作正常输出也可以用作调试输出。

## 通配符

`*` 字符可以用作通配符。假设库中有名字为 "connect:bodyParser"，"connect:compress"，"connect:session" 的调试器，不必像这样 `DEBUG=connect:bodyParser,connect:compress,connect:session` 列出所有的调试器，只需 `DEBUG=connect:*` 这样即可，或使用 `DEBUG=*` 运行所有模块。

还可以通过添加前缀 "-" 字符来排除特定的调试器。例如，`DEBUG=*,-connect:*` 将包含除了以 "connect:" 开头的所有调试器。

## 环境变量

在运行 Node.js 时，可以设置一些环境变量来改变调试输出的行为：

| 名字                | 作用                               |
|---------------------|------------------------------------|
| `DEBUG`             | 启用/禁用特定的调试命名空间。      |
| `DEBUG_HIDE_DATE`   | 隐藏调试输出中的日期（非TTY）。  |
| `DEBUG_COLORS`      | 是否在调试输出中使用颜色。         |
| `DEBUG_DEPTH`       | 对象检查深度。                     |
| `DEBUG_SHOW_HIDDEN` | 显示被检查对象的隐藏属性。         |

**注意：** 以 `DEBUG_` 开头的环境变量最终被转换为 Options 对象，并与 `%o`/`%O` 格式符一起使用。查看 Node.js 文档中的 [`util.inspect()`](https://nodejs.org/api/util.html#util_util_inspect_object_options) 获取完整 Options 列表。

## 格式符

调试使用 [printf-style](https://wikipedia.org/wiki/Printf_format_string) 格式，以下是官方支持的格式符：

| 格式符    | 展示                            |
|-----------|---------------------------------|
| `%O`      | 多行打印一个对象。              |
| `%o`      | 单行打印一个对象。              |
| `%s`      | 字符串。                        |
| `%d`      | 数字 (包含整型和浮点型)。       |
| `%j`      | JSON。如果包含循环引用，则使用 '[Circular]' 字符串代替。|
| `%%`      | 单个百分号（'%'）。不消耗参数。 |


### 自定义格式符

可以通过扩展 `debug.formatters` 对象来添加自定义格式符。例如，如果想要添加一个支持将 Buffer 类型展示为 hex 形式的格式符 `%h`，可以这样做：

```js
const createDebug = require('debug')
createDebug.formatters.h = (v) => {
  return v.toString('hex')
}

// …elsewhere
const debug = createDebug('foo')
debug('this is hex: %h', new Buffer('hello world'))
// foo this is hex: 68656c6c6f20776f726c6421 +0ms
```

## 浏览器支持

可以使用 [browserify](https://github.com/substack/node-browserify) 构建浏览器端就绪的脚本，或者如果不想自己构建的话，可以只使用 [browserify-as-a-service](https://wzrd.in/) [build](https://wzrd.in/standalone/debug@latest)。 

Debug 的启用状态目前由 `localStorage` 持久化。考虑这种存在 `worker:a` 和 `worker:b` 并希望同时调试两者的情况。可以使用 `localStorage.debug` 来启用此操作：

```js
localStorage.debug = 'worker:*'
```

然后刷新页面。

```js
a = debug('worker:a');
b = debug('worker:b');

setInterval(function(){
  a('doing some work');
}, 1000);

setInterval(function(){
  b('doing some work');
}, 1200);
```

## 输出流

默认情况下，`debug` 会输出到 stderr，但是可以通过重写 `log` 方法来为每个命名空间配置不同的输出： 

示例 [stdout.js](https://github.com/visionmedia/debug/blob/master/examples/node/stdout.js)：

```js
var debug = require('debug');
var error = debug('app:error');

// by default stderr is used
error('goes to stderr!');

var log = debug('app:log');
// set this namespace to log via console.log
log.log = console.log.bind(console); // don't forget to bind to console!
log('goes to stdout');
error('still goes to stderr!');

// set all output to go via console.info
// overrides all per-namespace log settings
debug.log = console.info.bind(console);
error('now goes to stdout via console.info');
log('still goes to stdout, but via console.info now');
```

## 扩展

可以很容易的扩展调试器：

```js
const log = require('debug')('auth');

//creates new debug instance with extended namespace
const logSign = log.extend('sign');
const logLogin = log.extend('login');

log('hello'); // auth hello
logSign('hello'); //auth:sign hello
logLogin('hello'); //auth:login hello
```

## 动态设置 

可以通过调用 `enable()` 方法动态的启用调试：

```js
let debug = require('debug');

console.log(1, debug.enabled('test'));

debug.enable('test');
console.log(2, debug.enabled('test'));

debug.disable();
console.log(3, debug.enabled('test'));
```

输出 :   
```cmd
1 false
2 true
3 false
```

使用：

`enable(namespaces)`

namespaces 可以包括由冒号分隔的模式和通配符。

注意，调用 `enable()` 会完全覆盖先前设置的 DEBUG 环境变量：

```
$ DEBUG=foo node -e 'var dbg = require("debug"); dbg.enable("bar"); console.log(dbg.enabled("foo"))'
=> false
```

`disable()`

将会禁用所有名称空间。函数返回当前启用（和跳过）的命名空间。 如果不知道启动的内容想要暂时禁用调试时，它就很有用。

例如：

```js
let debug = require('debug');
debug.enable('foo:*,-foo:bar');
let namespaces = debug.disable();
debug.enable(namespaces);
```

注意：不能保证字符串与初始启用字符串相同，但在语义上它们将是相同的。

## 检查调试目标是否启用调试

在创建调试实例后，可以通过检查 `enabled` 属性判断其是否启用调试：

```js
const debug = require('debug')('http');

if (debug.enabled) {
  // do stuff...
}
```

还可以手动切换此属性以强制启用或禁用调试实例。

## 作者

- TJ Holowaychuk
- Nathan Rajlich
- Andrew Rhyne

## 译者

[Jordan Wang](https://github.com/mingmingwon/)
