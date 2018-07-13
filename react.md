# React 简介

> React 是 Javascript 库，其目标在于简化可视化界面的开发。学习它为何如此受欢迎，以及它解决了什么问题。

## React 简介

### 什么是 React

React 是一个 Javascript 库，其目标在于简化可视化界面的开发。

由 Facebook 开发并在 2013 年公布于世，它驱动着世界上使用最广泛的代码，为 Facebook 和 Instagram 以及其他许多软件公司提供支持。

Its primary goal is to make it easy to reason about an interface and its state in any point in time, by dividing the UI into a collection of components.

React 被用来构建单页 Web 应用，其中，有许多库和框架都先于 React 诞生。

### React 为何如此流行

React 已经风靡 Web 前端开发的世界。为什么呢？

#### 比其它替代方案简单

React 面世的时候，Ember.js 和 Angular 1.x 作为框架是优秀的选择。这两者在代码中都增加了许多约定，对于移植现有应用来说根本不方便。React 很容易集成到现有应用中，因为他们只有在 Facebook 上这么做才能将其引进现有代码库。同时，这 2 个框架也带来了许多不足，因为 React 只选择实现视图层，而不是整个 MVC。

#### 时间恰好

当时，谷歌宣布了 Angular 2.x，但它带来了重大变化及不向后兼容。从 Angular 1 迁移到 2 就像迁移到不同的框架，因此，随着 React 承诺的执行速度的改善，使得它成为开发者跃跃欲试的东西。

#### Facebook 的支持

由 Faceboo 支持显然对一个项目非常有益，但不保证，因为你能看到许多由 Facebook 和 Google 支持的失败的开源项目。

#### React 真的那么简单么？

尽管我说过 React 比其替代框架更简单，深入 React 仍然是很复杂的，但大多数是因为和 React 集成的配套技术，例如 Redux、Relay 或者 [GraphQL](https://flaviocopes.com/graphql)。

React 内部只有很少的 API。

React 中没有比这些概念更多的了：

- Components
- JSX
- State
- Props

## JSX

许多开发者，包括写这篇文章的人，第一眼看到 JSX 是非常可怕的，并且会迅速驳斥 React。

尽管它们说 JSX 不是必须的，使用没有 JSX 的 React 是痛苦的。

我花了几年时间偶尔看一下它才开始消化 JSX，现在我更喜欢它胜过替代方案，即：使用模板。

使用 JSX 最大的好处就是你只与 Javascript 对象交互，而不是模板字符串。

> JSX 不是嵌入式的 HTML

许多针对 React 初学者的教程喜欢推迟 JSX 的介绍，因为他们假设读者没有它会更好，但是现在我是 JSX 的粉丝，我会立即跳到这个部分。

这里是你如何定义一个包含字符串的 h1 标记：

```jsx
const element = <h1>Hello, world!</h1>;
```

它看起来像是奇怪的 HTML 和 Javascript 的混合，但实际上它全部是 Javascript。

看上去是 HTML，实际上是用来定义组件和它们在标记中的定位的语法糖。

在 JSX 表达式内，可以轻松添加属性：

```jsx
const myId = 'test';
const element = <h1 id={myId}>Hello, world!</h1>;
```

你只需要注意当属性有横线（`-`）时会被转换成小驼峰形式，这是 2 种特殊情况：

- `class` 变成 `className`
- `for` 变成 `htmlFor`

因为它们是 Javascript 中的关键字。

这是一段 JSX 片段，在 `div` 标记中包裹着两个组件：

```jsx
<div>
  <BlogPostList />
  <Sidebar />
</div>
```

一个标签通常需要闭合，因为这是比 HTML 更加 XML 的语法（如果你记得 XHTML，则是很相似的，但是自那以后 HTML 弱语法胜出了）。在这种情况下使用了自闭合标签。

React 引入 JSX 不再是 React 独有的技术。

## React 组件

### 什么是 React 组件

一个组件即是一个独立的接口。举个例子，在一个典型的博客主页面，你可能会找到 Sidebar 组件 和 Blog Post Lists 组件。它们又有组件本身组成，所以你可能会有一个 Blog post 组件的列表，每篇博客文章对应一个组件，而且每个组件拥有自身的属性。

![components](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/react/1.png)

React 使得这些非常简单：一切皆是组件。

甚至纯 HTML 标记本身也是组件，而且默认情况下它们已经被添加进来了。

下面这 2 行代码是等价的，因为它们做了同样的事情。一个有 JSX，另一个没有，都是在 id 为 `app` 的元素中注入 `<h1>Hello World!</h1>`。

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<h1>Hello World!</h1>, document.getElementById('id'));

ReactDOM.render(
  React.DOM.h1('Hello World!'),
  document.getElementById(null, 'app')
);
```

看，`React.DOM` 给了我们一个 `h1` 组件。其它的 HTML 标记适用么？全部适用！你可以通过在浏览器控制台键入 `React.DOM` 来检查它提供了什么：

![react.dom](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/react/2.png)

（截图省略，列表太长）

内建组件非常好，但你的需求很快就会超过它们。React 擅长的是通过构建自定义组件来组成我们的 UI。

### 自定义组件

在 React 种有两种方式定义组件。

无状态组件不管理内部状态，它只是一个函数：

```javascript
const BlogPostExcerpt = () => {
  return (
    <div>
      <h1>Title</h1>
      <p>Description</p>
    </div>
  );
};
```

有的状态组件是一个 class，它管理自身属性中的状态。

```javascript
import React, { Component } from 'react';

class BlogPostExcerpt extends Component {
  render() {
    return (
      <div>
        <h1>Title</h1>
        <p>Description</p>
      </div>
    );
  }
}
```

它们是等价的因为现在还没有状态管理（将在下几篇文章中引入）。

这是第三种语法，它使用了 `ES5`/`ES2015` 语法，没有 classes：

```javascript
import React from 'react';

React.createClass({
  render() {
    return (
      <div>
        <h1>Title</h1>
        <p>Description</p>
      </div>
    );
  }
});
```

你在现代 `> ES6` 的代码库中几乎看不到这种语法。

Props 是组件获取它们自身属性的方式。从顶层组件开始，每个子组件都从其父组件获取它的 props。在无状态组件中，它获取的 props 都是传递进来的，通过在函数参数中添加 `props` 就可以使用。

```javascript
const BlopPostExcerpt = props => {
  return (
    <div>
      <div>{props.title}</div>
      <p>{props.description}</p>
    </div>
  );
};
```

在有状态组件中，props 是默认传递的。不需要做任何特别的处理，在组件实例中可以通过 `this.props` 访问。

```javascript
import React, { Component } from 'react';

class BlogPostExcerpt extends Component {
  redner() {
    return (
      <div>
        <h1>{this.props.title}</h1>
        <p>{this.props.description}</p>
      </div>
    );
  }
}
```

将 props 向下传递给子组件是在应用程序中传值的好方法。组件既可以拥有数据（状态），也可以通过 props 接收数据。

以下情况会变得复杂：

- 你需要访问好几层以下的子组件的状态
- 你需要访问一个毫无关联的组件的状态

[Redux](https://flaviocopes.com/redux/) 传统上非常流行，这也是许多教程中包含它的原因。

最近 React（16.3.0 版本）引入了 **Context API**，它使得 Redux 在这种简单的情况下有些冗余。

我们将在稍后讨论 Context API。

Redux 已经有用，如果你：

- 出于一些原因，要将数据迁移出应用程序
- 创建复杂的 reducers 和 actions 来以任何你想要的方式操作数据

但是现在 Redux 不再是 React 应用程序的“必需品”了。




