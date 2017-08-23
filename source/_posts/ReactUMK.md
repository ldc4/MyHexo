---
title: React不得不熟记的知识点
categories:
  - React
tags:
  - review
date: 2017-8-22 13:38:09
---

说明:
本文主要内容出自于官方文档，不建议初学者阅读此文。
主要用于快速复习React基础知识点。

<!-- more -->

## React Element

一个`React Element`:
```
const element = (
     <h1 className="hey">demo</h1>
);
// 或
const element = React.createElement(
     'h1',
     {className:'hey'},
     'demo'
);
```

本质：
```
const element = {
     type: 'h1',
     props:{
          className: 'hey',
          children: 'demo'
     }
};
```

## React Component

函数式组件 和 类组件
```
function Welcome(props){
     return <h1>hello,{props.name}</h1>
}

class Welcome extends React.Component {
     render() {
          return <h1>hello.{this.props.name}</h1>
     }
}
```

也可以使用React.createClass来创建类组件。

组件只能返回一个根元素。

All React components must act like pure functions with respect to their props.
(所有的React组件关于props必须使用纯函数)

state是类组件的一个特性，函数式组件没有state

>1.使用setState()去改变state，直接赋值是不会立即渲染的

>2.setState中如果有计算的话，就放到函数中：
>```
this.setState((preState, props) => ({
     counter: preState.counter + props.increment
}));
```
>3.setState是以合并的方式更新状态的

React是单向数据流，state是各自隔离的，组件间互不干扰。

## 事件

```
<button onClick={clickFunc}>test</button>
```

不能通过返回false来阻止事件传播，必须显示调用：`e.preventDefault()`;

另外事件需要绑定this,不然会报错。

在CreateReactAPP中，可以通过`属性初始化`语法来规避这个`this.clickFunc.bind(this)`:

```
clickFunc = () => {
     console.log('this value', this);
}
```

还有一种方式：（但是这样在传给子组件的时候，可能引起重新渲染）
```
<button onClick={(ev, arg1, arg2,……) => {this.handleClick(ev, arg1, arg2,……)}}/>

handleClick(ev, arg1, arg,……) {
  //code
}
```

事件传参：
```
<button onClick={this.handleClick.bind(this, props0, props1, ...}></button>

handleClick(porps0, props1, ..., event) {
  // your code here
}
```

*官网推荐使用绑定`this`和`属性初始化`语法。*

## 条件渲染

简短的条件渲染方式：

```
{ flag > 0 &&
     // 你要渲染的代码
}
```

```
{ flag > 0 ? (
     // 你要渲染的代码1
): (
     // 你要渲染的代码2
)}
```

`return null; `表示不渲染任何东西

```
const arr = [a,b,c];
const items = arr.map((item) => (
     <li>{item.name}</li>
));
```

上面的代码可以直接放在`<ul>{items}</ul>`就可以展示

```
<li key={item.toString()}>
     {item.name}
</li>
```
key用于React表示数组的某一项的状态（变更，添加，删除）。
一般使用唯一的物理编号ID，如果没有，也可以使用索引。
不建议使用索引，性能问题。

在调用map方法中使用key，key仅仅是用于提示React，如果你确实需要这些key值，请自行设置一个props传给组件。

## 受控组件与非受控组件

Controlled Component （受控组件）:
```
<form onSubmit={}>
     <input name="a" type="text" value="b" onChange={}/>
     <textarea value={} onChange={}/>
     <select value={} onChange={}>
          <option value="x">x</option>
          <option value="y">x</option>
          <option value="z" selected>x</option>
     </select>
</form>
```

Uncotrolled Component （非受控组件）:
```
<input defualtValue="weedust" type="text" ref={(input) => this.input = input} />
```
通过ref得到DOM的值。defaultValue/defaultChecked用于设置初始值。

Element|Value property|Change callback|New value in the callback
-|-|-|-
`<input type="text" />`|value="string"|onChange|event.target.value
`<input type="checkbox" />`|checked={boolean}|onChange|event.target.checked
`<input type="radio" />`|checked={boolean}|onChange|event.target.checked
`<textarea />`|value="string"|onChange|event.target.value
`<select />`|value="option value"|onChange|event.target.value

feature|uncontrolled|controlled
-|-|-
one-time value retrieval (e.g. on submit)|✅ |✅
instant field validation|❌|✅
conditionally disabling submit button|❌ |✅
enforcing input format|❌|✅
several inputs for one piece of data|❌|✅
dynamic inputs|❌|✅

上表请参考：https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/

controlled表现为`push`, uncontrolled表现`pull`

A form element becomes “controlled” if you set its value via a prop. That’s all.

## React理念

1.把UI划分成组件层次

2.用React创建一个静态版本

3.定义UI状态的最小（完整）表示
>判断是不是状态：
>它是通过 props 从父级传来的吗？如果是，他可能不是 state。
>它随着时间推移不变吗？如果是，它可能不是 state。
>你能够根据组件中任何其他的 state 或 props 把它计算出来吗？如果是，它不是 state。

4.确定你的state应该位于哪里
>确定每一个需要这个 state 来渲染的组件。
>找到一个公共所有者组件(一个在层级上高于所有其他需要这个 state 的组件的组件)
>这个公共所有者组件或另一个层级更高的组件应该拥有这个 state。
>如果你没有找到可以拥有这个state的组件，创建一个仅用来保存状态的组件并把它加入比这个公共所有者组件层级更高的地方。

5.添加反向数据流
>就是回调setState改变状态。
 
## 进阶

*关于JSX：其实没有什么难度，就是XML和JS混写* ?
 
### JSX In Depth

```
<MyButton color="blue" shadowSize={2}>
    Click Me
</MyButton>
// 等价于
React.createElement( MyButton, {color: 'blue', shadowSize: 2}, 'Click Me' )
```

由于JSX只是语法糖，所以在使用React的时候不能只导入MyButton，也要导入React：
```
import React from 'react';
import MyButton from './MyButton';
```
 
可以使用`.`符号
```
import React from 'react';
const MyComponents = {
    DatePicker: function DatePicker(props) {
         return <div>Imagine a {props.color} datepicker here.</div>;
     }
}
 
function BlueDatePicker() {
     return <MyComponents.DatePicker color="blue" />;
}
```

自定义的React组件，务必使用大写字母。
 
使用React组件的时候，可以用变量来替代组件名，
但是不能直接写在JSX中，需要另外定义一个变量。例如：

```
const components = {
    photo: PhotoStory, video: VideoStory
};
function Story(props) {
    const SpecificStory = components[props.storyType];
    return <SpecificStory story={props.story} />;
} 
```

React组件的属性是可以接受表达式的，用大括号`{}`。但是`if`和`for`不是表达式，所以不能直接用在JSX中。
 
当你通过`{'xxx'}`传递一个字符串字面量时，它是HTML的转义字符。
 
如果只是一个单属性名，没有值，默认是`true`。不推荐这样使用，因为ES6中不是这种表达方式。容易产生混淆。
 
可以在属性中使用扩展运算符： `{...props}`

React组件内容，在React组件中可以通过`props.children`来访问。

在JSX中，React组件内容是HTML转义的，而且去掉了空白符差异，连续多个空白符只算作一个，也就是空格、回车是等价的。

组件是可以嵌套的。但是组件的最外层只能是一个节点。

组件内容可以使用JS表达式{}，常用于渲染列表。甚至还可以传递一个函数。

`{}`中，`boolean`, `null`, `undefined` 不会渲染。但是可以`{ flag && <Xxx />}`。`0`还是会渲染的，所以不能用`{ xx.length && <Xxx />}`，而要使用`{ xx.length > 0 && <Xxx />}`
想要呈现`null`,`undefined`可以使用 `{String(null)}`
 
### Typechecking With PropTypes
 
react提供内置的属性检查机制。
随着项目越来越大，可以通过类型检查机制，更好的排错。
不过，个人认为，项目初期，没必要限定类型。因为需求字段经常变动，待项目逐步完善趋于稳定的时候，再把类型加上。
 
```
import PropTypes from 'prop-types';

MyComponent.propTypes = {
    // 你可以将属性声明为以下 JS 原生类型
    optionalArray: PropTypes.array,
    optionalBool: PropTypes.bool,
    optionalFunc: PropTypes.func,
    optionalNumber: PropTypes.number,
    optionalObject: PropTypes.object,
    optionalString: PropTypes.string,
    optionalSymbol: PropTypes.symbol,

    // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
    optionalNode: PropTypes.node,

    // 一个 React 元素
    optionalElement: PropTypes.element,

    // 你也可以声明属性为某个类的实例，这里使用 JS 的
    // instanceof 操作符实现。
    optionalMessage: PropTypes.instanceOf(Message),

    // 你也可以限制你的属性值是某个特定值之一
    optionalEnum: PropTypes.oneOf(['News', 'Photos']),

    // 限制它为列举类型之一的对象
    optionalUnion: PropTypes.oneOfType([
        PropTypes.string,
        PropTypes.number,
        PropTypes.instanceOf(Message)
    ]),

    // 一个指定元素类型的数组
    optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

    // 一个指定类型的对象
    optionalObjectOf: PropTypes.objectOf(PropTypes.number),

    // 一个指定属性及其类型的对象
    optionalObjectWithShape: PropTypes.shape({
        color: PropTypes.string,
        fontSize: PropTypes.number
    }),

    // 你也可以在任何 PropTypes 属性后面加上 `isRequired` 
    // 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
    requiredFunc: PropTypes.func.isRequired,

    // 任意类型的数据
    requiredAny: PropTypes.any.isRequired,

    // 你也可以指定一个自定义验证器。它应该在验证失败时返回
    // 一个 Error 对象而不是 `console.warn` 或抛出异常。
    // 不过在 `oneOfType` 中它不起作用。
    customProp: function(props, propName, componentName) {
        if (!/matchme/.test(props[propName])) {
            return new Error(
                'Invalid prop `' + propName + '` supplied to' +
                ' `' + componentName + '`. Validation failed.'
            );
        }
    },
    // 不过你可以提供一个自定义的 `arrayOf` 或 `objectOf` 
    // 验证器，它应该在验证失败时返回一个 Error 对象。 它被用
    // 于验证数组或对象的每个值。验证器前两个参数的第一个是数组
    // 或对象本身，第二个是它们对应的键。
    customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
        if (!/matchme/.test(propValue[key])) {
            return new Error(
                'Invalid prop `' + propFullName + '` supplied to' +
                ' `' + componentName + '`. Validation failed.'
            );
        }
    })
};    
```

默认的属性值可以用`defaultProps`
```
// 为属性指定默认值:
Greeting.defaultProps = {
name: 'Stranger'
};
```

### Refs and the DOM
 
React典型的数据流是props，父子组件通过props来通讯。
 
而有时候需要在这种数据流之外，强行修改子组件。React提供Refs来实现这个需求，但是前提是子组件必须是一个React Component或者一个DOM元素。
 
场景：
Managing focus, text selection, or media playback. 
(处理焦点、文本选择或媒体控制。)
Triggering imperative animations.
(触发强制动画。)
Integrating with third-party DOM libraries.
(集成第三方 DOM 库)
 
`ref`属性用在HTML标签上，接收一个底层DOM元素对象。

但是`ref`不能用在functional component...
You should convert the component to a class if you need a ref to it, just like you do when you need lifecycle methods or state.
 
但是，你可以在函数式组件内部使用 `ref`，只要它指向一个 DOM 元素或者 class 组件
```
function CustomTextInput(props) {
    // 这里必须声明 textInput，这样 ref 回调才可以引用它
    let textInput = null;

    function handleClick() {
        textInput.focus();
    }

    return (
        <div>
            <input
                type="text"
                ref={(input) => { textInput = input; }} />
            <input
                type="button"
                value="Focus the text input"
                onClick={handleClick}
            />
        </div>
    ); 
}
```

## Context

官方不推荐使用这个属性。

用法：

在上层组件中，通过
```
getChildContext() {
     return {}
}
```
定义`xxxx.childContextTypes`
来提供context属性。

下层组件通过 定义`xxx.contextTypes`来使用`this.context.abc`

如果子组件定义了`contextTypes`，生命周期钩子多了一个`context`参数

stateless functional components 也能使用`context`属性

props和state改变，会自动调用`getChildContext`。通过`this.setState`来更新`context`
 
那么问题来了，由于组件更新产生的新的context，如果有一个中间的父组件 的shouldComponentUpdate返回了false,那么接下来的子组件中的context是不会被更新的。
这么使用context的话，组件就失控了，所以没有一种可靠的方式来更新context。
 
### HOC
 
https://facebook.github.io/react/docs/higher-order-components.html
HOC = higher order component 高阶组件

高阶组件就是一个没有副作用的纯函数，该函数接受一个组件作为参数，并返回一个新的组件。
个人觉得就像是一个代理一样。
 
不要改变原始组件，而使用组合
 
类似与`connect()` `form()`这种方式得到的组件，就是HOC。
```
// React Redux's `connect`
const ConnectedComment = connect(commentSelector, commentActions)(Comment);

function connect(mapStateToProps, mapDispatchToProps){
     return function( Comp ){
          return class extends React.Component {
               render (){
                    return <Comp {...this.props} />
               }
          }
     }
}
```

约定：将不相关的props属性传递给包裹组件
约定：最大化使用组合
约定：包装显示名字以便于调试
 
注意事项：
不要再render函数中使用高阶组件
必须将静态方法做拷贝（可以使用hoist-non-react-statics来帮你自动处理）
Refs属性不能传递

### SyntheticEvent
 
React中的事件对象，是在w3c标准下包装的，是跨浏览器的。
 
获取浏览器native event ，使用 nativeEvent属性
 
SyntheticEvent拥有下面的属性：
```
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
DOMEventTarget target
number timeStamp
string type
```
 
由于在v0.14版本中，事件处理函数返回false不会再阻止事件传播, 所以必须得手动触发`e.stopPropagation()`和`e.preventDefault()` 方法。
 
SyntheticEvent是共享的。那就意味着在调用事件回调之后，SyntheticEvent对象将会被重用，并且所有属性会被置空。这是出于性能因素考虑的。 因此，您无法以异步方式访问事件。
 
如果您想以异步的方式访问事件的属性值，你必须在事件回调中调用`event.persist()`方法，这样会在池中删除合成事件，并且在用户代码中保留对事件的引用。
 
在事件名后面加Capture就能在事件捕获阶段注册事件处理函数。

---

## 参考文档

官方文档：https://facebook.github.io/react/docs/hello-world.html
中文文档：https://discountry.github.io/react/docs/hello-world.html









