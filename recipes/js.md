# JS

## 请描述对同源策略的理解

- [issues/590](https://github.com/haizlin/fe-interview/issues/590)

出于浏览器的安全考虑,避免沾染其他域的恶意文件代码,只有协议,域名,端口都相同的文档才能被读写。

同源的三个要素

- 协议相同
- 端口相同
- 域名相同

## 为什么会有跨域问题？怎么解决跨域？

- [issues/150](https://github.com/haizlin/fe-interview/issues/150)

常用的跨域方式基本就是这三种：

- JSONP: 优点是可以兼容老浏览器，缺点是只能发送GET请求。通过script可以跨域的原理，执行服务端的回调函数。
- CORS: 优点简单方便，支持post请求，缺点是需要后端的配合,不支持老版浏览器。"跨域资源共享",设置'Access-Control-Allow-Origin=*。
- Server Proxy: 优点是前端正常发送ajax请求，缺点是后端会二次请求。nigix 或者webpack 代理 配置。

其他的跨域方式还有：location.hash、window.name、postMessage等方式，有时间也可以了解一下。

## window对象和document对象有什么区别？

- [issues/157](https://github.com/haizlin/fe-interview/issues/157)

**window对象**

代表浏览器中的一个打开的窗口或者框架，window对象会在或者每次出现时被自动创建，在客户端JavaScript中，Window对象是全局对象global，所有的表达式都在当前的环境中计算，要引用当前的窗口不需要特殊的语法，可以把那个窗口属性作为全局变量使用，例如：可以只写`document`，而不必写`window.document`。同样可以把窗口的对象方法当做函数来使用，如：只写`alert()`，而不必写`window.alert`.
window对象实现了核心JavaScript所定义的全局属性和方法。

**document对象**

代表整个HTML文档，可以用来访问页面中的所有元素 。

每一个载入浏览器的HTML文档都会成为document对象。document对象使我们可以使用脚本(js)中对HTML页面中的所有元素进行访问。
**document对象是window对象的一部分**可以通过window.document属性对其进行访问
HTMLDocument接口进行了扩展，定义HTML专用的属性和方法，很多属性和方法都是HTMLCollection对象，其中保存了对锚、表单、链接以及其他可脚本元素的引用。

## 简单介绍一下事件冒泡机制，如何阻止冒泡？

- [issues/179](https://github.com/haizlin/fe-interview/issues/179)

事件传播的过程分为捕获阶段、目标阶段和冒泡阶段。冒泡阶段是从目标到window对象的过程。事件默认是冒泡的，当父元素添加监听事件，点击子元素后，父元素上的事件会被触发，这就是典型的冒泡。为了防止触发可以通过evet.target来判读或者直接event.stopPropagation()阻止事件冒泡。

## 介绍一下常用的 ES6 特性以及带来的优势

## var、let、const 的比较

**let**

- 所声明的变量，只在let命令所在的代码块内有效
- 不存在变量提升，声明的变量一定要在声明后使用，否则报错
- 暂时性死区，在代码块内，使用let命令声明变量之前，该变量都是不可用的（即使外围有同名变量的定义）。

**const**

const声明一个只读的常量。一旦声明，常量的值就不能改变。const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。

对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。

对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。

如果真的想将对象冻结，应该使用Object.freeze方法。

## ES6 对象可以解构赋值，变量名与属性名不一致如何结构赋值？

```js
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```
