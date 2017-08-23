title: 初窥React
date: 2016-06-22 09:47:31
categories:
- React
tags:
- start
---

React这么火，对不对。
我就先看看，免得孤陋寡闻了。

http://www.ruanyifeng.com/blog/2015/03/react.html
阮神的一年前的文章

就先看一下这个吧。

<!--more-->

## HTML模板

```
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      // ** Our code goes here! **
    </script>
  </body>
</html>
```
=
这里的script type="text/babel"作用：
基本思路是给它一个不正确的值, 然后让浏览器不把里面的内容当成 javascript 代码.
然后在自己的框架中去取这个标签的内容, 自己去解析, 如果你愿意你可以把它写成
`<script type="text/helloworld">` 也是可以的.
=

首先，最后一个 `<script>` 标签的 type 属性为 text/babel 。
这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。
凡是使用 JSX 的地方，都要加上 type="text/babel" 。


三个库： `react.js` `react-dom.js` `browser.js`

browser的作用是将JSX语法转为JavaScript语法，这一步耗时长，上线前，应在服务器完成


## ReactDOM.render()

**用于将模板转化为HTML语言，并插入指定的DOM节点**

例如：
`ReactDOM.render( a,b )`
a为模板，b为指定的DOM节点

## JSX语法

JSX语法允许HTML和Javascript混写，遇到`<`就执行HTML解析，遇到`{`就执行Javascript解析
JSX允许直接在模板插入Javascript变量， 如果这个变量是一个数组，则会展开这个数组的所有成员

例如：
```
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example'));
```

4.组件

`React.createClass()`用于生成一个组件类

```
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello {this.props.name}</h1>;
  }});

ReactDOM.render(
  <HelloMessage name="John" />,
  document.getElementById('example'));
```
**每个组件类都有自己的render方法，用于输出自己。**
这里可以理解为自定义标签。
另外，有2个规范：
1.类名首字母大写
2.组件类只能包含一个顶层标签（即返回的时候不能返回多个并列的标签）

组件的属性可以在this.props对象上获取

另外还得注意： 就是 class 属性需要写成 className ，for 属性需要写成 htmlFor ，这是因为 class 和 for 是 JavaScript 的保留字。

## this.props.children

(this.props对象的属性)与(组件的属性)一一对应，但是有个例外：就是`this.props.children`,表示所有组件的子节点

`this.props.children`的值有三种可能：如果当前组件没有子节点，它就是`undefined`;如果有一个子节点，数据类型是`object`；如果有多个子节点，数据类型就是`array`。所以，处理`this.props.children`的时候要小心。

React提供一个工具方法`React.Children`来处理`this.props.children`。我们可以用`React.Children.map`来遍历子节点，而不用担心`this.props.children`的数据类型是`undefined`还是`object`

https://facebook.github.io/react/docs/top-level-api.html#react.children


## PropTypes

用来验证组件实例的属性是否符合要求：
```
var MyTitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },

  render: function() {
     return <h1> {this.props.title} </h1>;
   }});
```
如果你给MyTiltle标签设置一个title=123
虽然能正常运行，但是控制台会给出错误提示

更多PropTypes设置：https://facebook.github.io/react/docs/reusable-components.html

`getDefaultProps`方法可以用来设置组件属性的默认值


## 获取真实的DOM节点

组件并不是真实的DOM节点，而是存在于内存中的一种虚拟结构，叫做虚拟DOM。
只有当它插入到文档当中，才会变成真实的DOM

根据React的设计，所有的DOM变动，都先在虚拟DOM上发生，然后再将实际发生变动的部分，反映在真实DOM上，这种算法叫做DOM diff，它可以极大提高网页的性能表现。

有时需要从组件获取真实DOM的节点，这时就要用到`ref`属性

例如：
组件MyComponent的子节点有一个文本输入框，用于获取用户的输入。这时就必须获取真实的DOM节点，虚拟DOM是拿不到用户输入的。为了做到这一点，文本输入框必须有一个ref属性，然后this.refs.[refName]就会返回这个真实的DOM节点。

需要注意的是，由于`this.refs.[refName]`属性获取的是真实DOM，所以必须等到虚拟DOM插入文档以后，才能使用这个属性，否则会报错。

可以通过为组件指定Click事件的回调函数，确保了只有等到真实DOM发生Click事件之后，才会读取`this.refs.[refName]`属性。

React组件支持很多事件，除了Click事件以外，还有KeyDown、Copy、Scroll等，完整的事件清单请查看
https://facebook.github.io/react/docs/events.html#supported-events

##　this.state

`getInitialState`方法用于定义初始状态，也就是一个对象。
这个对象可以通过`this.state`属性读取
`this.setState`方法就用来修改状态，每次修改都会自动调用`this.render`方法，再次渲染组件

由于`this.props`和`this.state`都用于描述组件的特性，可能会产生混淆。
一个简单的区分方法是，`this.props`表示那些一旦定义，就不再改变的特性，
而`this.state`是会随着用户互动而产生变化的特性。

## 表单

用户在表单填入的内容，属于用户跟组件的互动，所以不能用`this.props`读取

`this.props`读取的是定义的组件的属性，而不是渲染的HTML的属性

```
var Input = React.createClass({
  getInitialState: function() {
    return {value: 'Hello!'};
  },
  handleChange: function(event) {
    this.setState({value: event.target.value});
  },
  render: function () {
    var value = this.state.value;
    return (
      <div>
        <input type="text" value={value} onChange={this.handleChange} />
        <p>{value}</p>
      </div>
    );
  }});

ReactDOM.render(<Input/>, document.body);
```

上面代码中，文本输入框的值，不能用`this.props.value`读取，而要定义一个`onChange`事件的回调函数，通过`event.target.value`读取用户输入的值。textarea元素、select元素、radio元素都属于这种情况，更多介绍请参考：
https://facebook.github.io/react/docs/forms.html

## 组件的生命周期

Mounting: 已插入真实DOM
Updating: 正在被重新渲染
Unmounting: 已移除真实DOM

React为每个状态都提供了两种处理函数，will函数在进入状态之前调用，did函数在进入状态之后调用，三种状态共计五种处理函数。

```
componentWillMount()
componentDidMount()
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
componentWillUnmount()
```


此外，React还提供两种特殊状态的处理函数。

```
componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用
```

具体参考：https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods


注意style的设置方式：

不能写成
```
style="opacity:{this.state.opacity};"
```
而要写成
```
style={{opacity: this.state.opacity}}
```
这是因为 React 组件样式是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象。
https://facebook.github.io/react/tips/inline-styles.html

## AJAX

组件的数据来源，通常是通过Ajax请求从服务器获取，可以使用componentDidMount方法设置Ajax请求，等到请求成功，再用 this.setState方法重新渲染UI



参考资料：
http://www.ruanyifeng.com/blog/2015/03/react.html










































