# React

## 简述 React 的特性

- JSX 声明式的编写UI
- 单项数据流
- 可复用、组件化
- Virtual DOM 高性能

## 为什么要引入 React?

```jsx
import React from 'react'

function CompA() {
  // ...other code
  return <p>lorem pas sdk</p>
}
```

代码都没有用到 React，为什么要引入 React 呢？

实际上，上面的代码转化成:

```js
function A() {
  // ...other code
  return React.createElement("p", null, "lorem pas sdk");
}
```

因为从本质上讲，JSX 只是为 React.createElement(component, props, ...children) 函数提供的语法糖。

## 为什么要调用`super(props)`？`super(props)`与`super()`有什么区别？

- [Why Do We Write super(props)?](https://overreacted.io/why-do-we-write-super-props/)

```jsx
class Checkbox extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOn: true };
  }
  // ...
}
```

为什么要调用 super(props) ？ super 指的是父类的构造函数，这里为 React.Component 的实现。在调用父类的构造函数之前，是不能在 constructor 中使用 this 关键字的。

super(props)与 super() 有什么区别？React 会在构造函数执行完毕之后给 this.props 赋值。如果调用了 super() ，this.props 在 super() 和 构造函数结束之间仍是 undefined。

```jsx
class Button extends React.Component {
  constructor(props) {
    super(); // 没有传 props
    console.log(props);      // {}
    console.log(this.props); // undefined 
  }
  // ...
}
```

## React中是如何区分Class和Function组件的？

-[How Does React Tell a Class from a Function?](https://overreacted.io/how-does-react-tell-a-class-from-a-function/)

函数形式的组件：

```jsx
function Greeting() {
  return <p>Hello</p>;
}
```

类形式的组件：

```jsx
class Greeting extends React.Component {
  render() {
    return <p>Hello</p>;
  }
}
```

使用<Greeting /> 组件时，并不关心它是如何定义的：

```jsx
// 是类还是函数 —— 无所谓
<Greeting />

// React 内部：function 组件
const result = Greeting(props); // <p>Hello</p>

// React 内部：class组件
const instance = new Greeting(props); // Greeting {}
const result = instance.render(); // <p>Hello</p>
```

如果不继承于 React.Component，React 不会在原型上找到 isReactComponent，因此就不会把组件当做类处理。

## React元素有的$$typeof属性有什么作用？

- [Why Do React Elements Have a $$typeof Property?](https://overreacted.io/why-do-react-elements-have-typeof-property/)

```
// React 元素（elements）是一个 plain object
{
  type: 'marquee',
  props: {
    bgcolor: '#ffa7c4',
    children: 'hi',
  },
  key: null,
  ref: null,
  $$typeof: Symbol.for('react.element'),
}
```

为了防止XSS攻击，提高安全性。React 0.14使用Symbol标记每个React 元素（element），而JSON不支持Symbol。如果浏览器不支持 Symbols 怎么办。React仍然会加上 $$typeof 字段以保证一致性，但只是设置一个数字而已 —— 0xeac7（看起来像React）。

## React.Component内部的setState()是如何更新DOM？

React.Component类包含了DOM更新的逻辑吗？但是如果是这样的话，this.setState()又如何能在其他环境下使用呢？React Native app中的组件也是继承自React.Component。他们依然可以像我们在上面做的那样调用this.setState()，而且React Native渲染的是安卓和iOS原生的界面而不是DOM。

React.Component以某种未知的方式将处理状态（state）更新的任务委托给了特定平台的代码。react包仅仅是让你使用 React 的特性，但是它完全不知道这些特性是如何实现的。而渲染器包(react-dom、react-native等)提供了React特性的实现以及平台特定的逻辑。这其中的有些代码是共享的(“协调器”)，但是这就涉及到各个渲染器的实现细节了。

## React 事件处理方法需要 bind？

**直接 bind this**

```jsx
class Foo extends React.Component {
  handleClick () {
    this.setState({ xxx: aaa })
  }

  render() {
    return (
      <button onClick={this.handleClick.bind(this)}>
        Click me
      </button>
    )
  }
}
```

- 优点：写起来顺手，一口气就能把这个逻辑写完，不用移动光标到其他地方。
- 缺点：性能不太好，这种方式跟 react 内部帮你 bind 一样的，每次 render 都会进行 bind，而且如果有两个元素的事件处理函数式同一个，也还是要进行 bind，这样会多写点代码，而且进行两次 bind，性能不是太好。

**constuctor 手动 bind**

```jsx
class Foo extends React.Component {
  constuctor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this)
  }
  handleClick () {
    this.setState({ xxx: aaa })
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    )
  }
}
```

- 优点：相比于第一种性能更好，因为构造函数只执行一次，那么只会 bind 一次，而且如果有多个元素都需要调用这个函数，也不需要重复 bind，基本上解决了第一种的两个缺点。
- 缺点：没有明显缺点，硬要说的话就是太丑了，然后不顺手(我觉得丑，你觉得不丑就这么写就行了)。

**箭头函数**

```jsx
class Foo extends React.Component {
  handleClick () {
    this.setState({ xxx: aaa })
  }

  render() {
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    )
  }
}
```

- 优点：顺手，好看。
- 缺点：每次 render 都会重复创建函数，性能会差一点。

**public class fields**

这种 class fields还处于实验阶段，据我所知目前还没有被纳入标准，具体可见这里。

```jsx
class Foo extends React.Component {
  handleClick = () => {
    this.setState({ xxx: aaa })
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    )
  }
}
```

- 优点：好看，性能好。
- 缺点：没有明显缺点，如果硬要说可能就是要多装一个 babel 插件来支持这种语法。

## setState 是同步还是异步？

执行过程代码同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，所以表现出来有时是同步，有时是“异步”。

只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout/setInterval等原生 API 中都是同步的。简单的可以理解为被 React 控制的函数里面就会表现出“异步”，反之表现为同步。

**那为什么会出现异步的情况呢？**

为了做性能优化，将 state 的更新延缓到最后批量合并再去渲染对于应用的性能优化是有极大好处的，如果每次的状态改变都去重新渲染真实 dom，那么它将带来巨大的性能消耗。

**那如何在表现出异步的函数里可以准确拿到更新后的 state 呢？**

通过第二个参数 setState(partialState, callback) 中的 callback 拿到更新后的结果。

```js
this.setState((state) => {
    return { val: newVal }
})
```
