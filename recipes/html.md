# HTML

## 页面导入样式时，使用link和@import有什么区别？

- [issue/1](https://github.com/haizlin/fe-interview/issues/1)

区别：

- link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
- link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
- 所以会出现一开始没有css样式，闪烁一下出现样式后的页面(网速慢的情况下)
- link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
- link支持使用Javascript控制DOM去改变样式；而@import不支持。

在html设计制作中，css有四种引入方式：

**方式一： 内联样式**

内联样式，也叫行内样式，指的是直接在 HTML 标签中的 style 属性中添加 CSS。示例：

```html
<div style="display: none;background:red"></div>
```

这通常是个很糟糕的书写方式，它只能改变当前标签的样式，如果想要多个`<div>`拥有相同的样式，你不得不重复地为每个`<div>`添加相同的样式，如果想要修改一种样式，又不得不修改所有的 style 中的代码。很显然，内联方式引入 CSS 代码会导致 HTML 代码变得冗长，且使得网页难以维护。

**方式二： 嵌入样式**

嵌入方式指的是在 HTML 头部中的`<style>`标签下书写 CSS 代码。示例：

```html
<head>
    <style>

    .content {
        background: red;
    }

    </style>
</head>
```

嵌入方式的 CSS 只对当前的网页有效。因为 CSS 代码是在 HTML 文件中，所以会使得代码比较集中，当我们写模板网页时这通常比较有利。因为查看模板代码的人可以一目了然地查看 HTML 结构和 CSS 样式。因为嵌入的 CSS 只对当前页面有效，所以当多个页面需要引入相同的 CSS 代码时，这样写会导致代码冗余，也不利于维护。

**方式三：链接样式**

链接方式指的是使用 HTML 头部的 标签引入外部的 CSS 文件。示例：

```html
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
```

这是最常见的也是最推荐的引入 CSS 的方式。使用这种方式，所有的 CSS 代码只存在于单独的 CSS 文件中，所以具有良好的可维护性。并且所有的 CSS 代码只存在于 CSS 文件中，CSS 文件会在第一次加载时引入，以后切换页面时只需加载 HTML 文件即可。

**方式四：导入样式**

导入方式指的是使用 CSS 规则引入外部 CSS 文件。示例：

```html
<style>
    @import url(style.css);
</style>
```

或者写在css样式中

```css
@charset "utf-8";
@import url(style.css);
*{ margin:0; padding:0;}
.notice-link a{ color:#999;}
```

## HTML全局属性(global attribute)有哪些（包含H5）？

- [issues/7](https://github.com/haizlin/fe-interview/issues/7)
- [Global attributes - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

全局属性：用于任何HTML5元素的属性

- accesskey: 设置快捷键
- autocapitalize: 控制输入的内容是否需要大写，以及如何大写
- class: 为元素设置类标识
- contenteditable: 指定元素内容是否可编辑
- contextmenu: 自定义鼠标右键弹出上下文菜单内容（仅firefox支持）
- data-*: 为元素增加自定义属性
- dir：设置元素文本方向（默认ltr；rtl）
- draggable: 设置元素是否可拖拽
- dropzone: 设置元素拖放类型（copy|move|link,H5新属性，主流均不支持）
- exportparts: Used to transitively export shadow parts from a nested shadow tree into a containing light tree.
- hidden: 规定元素仍未或不在相关
- id: 元素id，文档内唯一
- inputmode: Provides a hint to browsers as to the type of virtual keyboard configuration to use when editing this element or its contents. Used primarily on `<input>` elements, but is usable on any element while in contenteditable mode.
- is
- itemid
- itemprop
- itemref
- itemscope
- itemtype
- lang: 元素内容的语言
- part
- slot
- spellcheck: 是否启动拼写和语法检查
- style: 行内css样式
- tabindex: 设置元素可以获得焦点，通过tab导航
- title: 规定元素有关的额外信息
- translate: 元素和子孙节点内容是否需要本地化（均不支持）

## 元素的alt属性和title属性有什么区别？

- [issues/50](https://github.com/haizlin/fe-interview/issues/50)

**alt属性**
最常见用在 `<img>` 标签上，`alt` 属性是一个必需的属性，它规定在图像无法显示时的替代文本。

场景：

- 网速太慢
- `src` 属性中的错误
- 浏览器禁用图像
- 用户使用的是屏幕阅读器

用法：

用法：`alt` 属性只能用在 `img`、`area` 和 `input` 元素中（包括 `applet` 元素）。对于 `input` 元素，`alt` 属性意在用来替换提交按钮的图片。比如：

```
<input type="image" src="image.gif" alt="Submit" />
```

语法：
规定图像的替代文本

`alt` 文本的使用原则：

- 如果图像包含信息 - 使用 `alt` 描述图像
- 如果图像在 `a` 元素中 - 使用 `alt` 描述目标链接
- 如果图像仅供装饰 - 使用 `alt=""`

**title属性**

`title` 属性规定关于元素的额外信息。

这些信息通常会在鼠标移到元素上时显示一段工具提示文本（tooltip text）。

提示：`title` 属性常与 `form` 以及 `a` 元素一同使用，以提供关于输入格式和链接目标的信息。同时它也是 `abbr` 和 `acronym` 元素的必需属性。当然 `title` 属性是比较广泛使用的，可以用在除了`base`，`basefont`，`head`，`html`，`meta`，`param`，`script` 和 `title` 之外的所有标签。但是并不是必须的。

`title` 属性有一个很好的用途，即为链接添加描述性文字，特别是当连接本身并不是十分清楚的表达了链接的目的。这样就使得访问者知道那些链接将会带他们到什么地方，他们就不会加载一个可能完全不感兴趣的页面。另外一个潜在的应用就是为图像提供额外的说明信息，比如日期或者其他非本质的信息。

## 标签语义化的好处有哪些？

- [issues/31](https://github.com/haizlin/fe-interview/issues/31)

- 有利于SEO，有利于浏览器识别
- 方便协作、代码维护和重构

## 常见的浏览器内核都有哪些？并介绍下你对内核的理解

- [issues/34](https://github.com/haizlin/fe-interview/issues/34)

内核主要分为渲染引擎和 JS 引擎。前者负责页面的渲染，后者负责执行解析 JavaScript。后来，由于 JS 引擎越来越独立，现在所说的浏览器内核大都指渲染引擎。

主流的内核：

- Trident: 由微软开发，即我们熟知的 IE 内核
- Gecko: 使用 C++ 开发的渲染引擎，包括了 SpiderMonkey 即我们熟悉的 FireFox
- Webkit: Chrome 和 Safari 使用的内核，Opera 也开始使用。

Edge 也开始转投 Google Chromium。

## 怎样在页面上实现一个圆形的可点击区域？

- [issues/58](https://github.com/haizlin/fe-interview/issues/58)
- [HTML <area><map>标签及在实际开发中的应用](https://www.zhangxinxu.com/wordpress/2017/05/html-area-map/)

- DOM 元素配合 `border-radius: 50%` 即可实现圆形点击区域。[例子](https://codepen.io/Konata9/pen/zgNJVy?editors=1111)
- 利用 `<map>` 和 `<area>` 标签设置圆形点击区域。参考文章:[HTML 标签及在实际开发中的应用](https://www.zhangxinxu.com/wordpress/2017/05/html-area-map/)
- 利用 SVG 作出圆形，然后添加点击事件。
- 如果在 `canvas` 上，就需要画出圆形，然后计算鼠标的坐标是否落在圆内。
