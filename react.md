# React 简介

> React 是 Javascript 库，其目标在于简化可视化界面的开发。学习它为何如此受欢迎，以及它解决了什么问题。

<!-- TOC -->

- [React 简介](#react-简介)
  - [React 简介](#react-简介-1)
    - [什么是 React](#什么是-react)
    - [React 为何如此流行](#react-为何如此流行)
      - [比其它替代方案简单](#比其它替代方案简单)
      - [时间恰好](#时间恰好)
      - [Facebook 的支持](#facebook-的支持)
      - [React 真的那么简单么？](#react-真的那么简单么)
  - [JSX](#jsx)
  - [React 组件](#react-组件)
    - [什么是 React 组件](#什么是-react-组件)
    - [自定义组件](#自定义组件)
    - [Fragment](#fragment)
    - [PropTypes](#proptypes)
    - [我们可以使用什么类型](#我们可以使用什么类型)
    - [Requiring Properties](#requiring-properties)
    - [Props 的默认值](#props-的默认值)
    - [Props 是如何传递的](#props-是如何传递的)
    - [Children](#children)
    - [设置 state 的默认值](#设置-state-的默认值)
    - [访问 state](#访问-state)
    - [改变 state](#改变-state)
    - [为什么你应该总是使用 `setState`](#为什么你应该总是使用-setstate)
    - [state 是封装的](#state-是封装的)
    - [单向数据流](#单向数据流)
    - [在组件树上向上移动状态](#在组件树上向上移动状态)
  - [事件](#事件)
    - [事件处理器](#事件处理器)
    - [在方法中绑定 `this`](#在方法中绑定-this)
    - [事件参考](#事件参考)
      - [Clipboard](#clipboard)
      - [Composition](#composition)
      - [Keyboard](#keyboard)
      - [Focus](#focus)
      - [Form](#form)
      - [Mouse](#mouse)
      - [Selection](#selection)
      - [Touch](#touch)
      - [UI](#ui)
      - [Mouse Wheel](#mouse-wheel)
      - [Media](#media)
      - [Image](#image)
      - [Animation](#animation)
      - [Transition](#transition)
  - [React 声明式](#react-声明式)
    - [React 声明式](#react-声明式-1)
  - [Virtual DOM](#virtual-dom)
    - [“真实的” DOM](#真实的-dom)

<!-- /TOC -->

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

cccc

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

### Fragment

注意，我把返回值用一个 `div` 组件只能返回一个单独的元素，如果你有多个元素，你就需要将其包裹在另外一个容器标签中。

无论怎样这都将导致在页面中产生不必要的 `div` 标签。你可以通过使用 `React.Fragment` 来避免这种情况：

```javascript
import React, { Component } from 'react';

class BlogPostExcerpt extends Component {
  render() {
    return (
      <React.Fragment>
        <h1>{this.props.title}</h1>
        <p>{this.props.description}</p>
      </React.Fragment>
    );
  }
}
```

同时它有一个非常简短语法 `<></>`，仅在最近的版本中支持（Babel 7+）：

```javascript
import React, { Component } from 'react';

class BlogPostExcerpt extends Component {
  render() {
    return (
      <>
        <h1>{this.props.title}</h1>
        <p>{this.props.description}</p>
      </>
    );
  }
}
```

### PropTypes

由于 Javascript 是一门动态类型语言，我们没有办法在编译时规定变量的类型，而且当我们传递无效的类型时，它们将在运行时出错，或者虽然类型兼容，但并不是我们所期望的结果。

Flow 和 Typescript 能帮到许多，但是 React 有方法直接检查属性类型，甚至在代码运行之前，当我们传递了错误的值时，我们的工具（编辑器、linters）能够帮助我们检测到：

```javascript
import PropTypes from 'prop-types';
import React, { Component } from 'react';

class BlogPostExcerpt extends Component {
  render() {
    return (
      <div>
        <h1>{this.props.title}</h1>
        <p>{this.props.description}</p>
      </div>
    );
  }
}

BlogPostExcerpt.propTypes = {
  title: PropTypes.string,
  description: PropTypes.string
};

export default BlogPostExcerpt;
```

### 我们可以使用什么类型

这些是我们可接收的基本类型：

- PropTypes.array
- PropTypes.bool
- PropTypes.func
- PropTypes.number
- PropTypes.object
- PropTypes.string
- PropTypes.symbol

我们可以接收两种类型之一：

```javascript
PropTypes.oneOfType([PropTypes.string, PropTypes.number]);
```

我们可以接收许多值之一：

```javascript
PropTypes.oneOf(['Test1', 'Test2']);
```

我们可以接收一个类实例：

```javascript
PropTypes.instanceOf(Something);
```

我们可以接收任何 React 节点：

```javascript
PropTypes.node;
```

或者是任何类型：

```javascript
PropTypes.any;
```

数组有一种特殊的语法，我们可以用来接收特定类型的数组：

```javascript
PropTypes.arrayOf(PropTypes.string);
```

对象：

```javascript
PropTypes.shape({
  color: PropTypes.string,
  fontSize: PropTypes.number
});
```

### Requiring Properties

把 `isRequired` 设置为任何 PropTypes 的属性时，如果这个属性丢失，React 将会返回错误：

```javascript
PropTypes.arrayOf(PropTypes.string).isRequired;
PropTypes.string.isRequired;
```

### Props 的默认值

如果任意一个值不是必须的，我们就需要为其指定一个默认值，以免在组件初始化时丢失：

```javascript
BlogExcerpt.propTypes = {
  title: PropTypes.String,
  description: PropTypes.String
};

BlogExcerpt.defaultProps = {
  title: '',
  description: ''
};
```

一些像 [ESLint](https://flaviocopes.com/eslint/) 的工具能够强制定义组件中没有明确要求的 propTypes 的默认 props。

### Props 是如何传递的

在初始化组件时，以类似于 HTML 属性的方式传递 props：

```javascript
const desc = 'A description'

<BlogExcerpt title="A blog post" description={desc} />
```

我们将 title 作为普通字符串传递（这事儿我们只能以字符串来做），而 description 作为变量传递。

### Children

有一个特殊的 prop 是 `children`。它包含着传递到组件 `<body>` 中的任何值，例如：

```javascript
<BlogExcerpt title="A title" description="A description">
  Something
</BlogExcerpt>
```

在这个例子中，我们可以通过 `this.props.children` 来访问 `BlogExcerpt` 内部的 “Something”。

Props 允许组件从它的父组件接受属性，例如被“指示”打印一些数据，state 允许组件从它自身获取数据，并且独立于周围的环境。

记住：只有基于类的组件才有 state，所以如果你要在无状态（基于函数）的组件中管理状态，你首先要将其“升级”为 Class 组件：

```javascript
const BlogExcerpt = () => {
  return (
    <div>
      <h1>Title</h1>
      <p>Description</p>
    </div>
  );
};
```

变成：

```javascript
import React, { Component } from 'react';

class BlogExcerpt extends Component {
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

### 设置 state 的默认值

在组件的构造函数中初始化 `this.state`。例如 BlogExcerpt 组件可能会有一个 `clicked` 状态：

```javascript
class BlogExcerpt extends Component {
  constructor(props) {
    super(props);
    this.state = { clicked: false };
  }

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

### 访问 state

可以通过引用 `this.state.clicked` 来访问 `clicked` 状态：

```javascript
class BlogExcerpt extends Component {
  constructor(props) {
    super(props);
    this.state = { clicked: false };
  }

  render() {
    return (
      <div>
        <h1>Title</h1>
        <p>Description</p>
        <p>Clicked: {this.state.clicked}</p>
      </div>
    );
  }
}
```

### 改变 state

改变状态应该永不使用：

```javascript
this.state.clicked = false;
```

而是你应该总是使用 `setState()` 来代替，为其传递一个对象：

```javascript
this.setState({ clicked: false });
```

这个对象应该包含状态的子集或者超集。只有你传递的属性才会被改变，省略的部分会保持现有的状态。

### 为什么你应该总是使用 `setState`

原因是使用这种方法，React 就会知道 state 已经被改变了。然后它会开始一系列的事件，这些事件将会促使组件重新渲染以及任何 [DOM](https://flaviocopes.com/dom/) 更新。

### state 是封装的

一个组件的父组件是无法告诉其子组件是否为有状态或者无状态的，同样适用于组件的子组件。

有状态或无状态（基于类或函数）完全是实现细节，而其他组件不需要关心。

这就导致了单向数据流。

### 单向数据流

状态总是被组件所拥有。受此状态影响的数据只能影响其下方的组件：其子组件。

改变组件的状态永远不会影响它的父组件或者兄弟组件，或者任何其他组件：仅限于其子组件。

这也是状态经常被移动到组件树上的原因。

### 在组件树上向上移动状态

由于单向数据流原则，如果两个组件要共享一个状态，那么这个状态就需要向上移动到其共同的祖先。

许多时候最近的祖先是管理状态的好地方，但这不是强制性规定。

状态会通过 props 向下传递到需要它的组件上：

```javascript
class Converter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { currency: '€' };
  }

  render() {
    return (
      <div>
        <Display currency={this.state.currency} />
        <CurrencySwitcher currency={this.state.currency} />
      </div>
    );
  }
}
```

The state can be mutated by a child component by passing a mutating function down as a prop:

```javascript
class Converter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { currency: '€' };
  }

  handleChangeCurrency = event => {
    this.setState({
      currency: this.state.currency === '€' ? '$' : '?'
    });
  };

  render() {
    return (
      <div>
        <Display currency={this.state.currency} />
        <CurrencySwitcher
          currency={this.state.currency}
          handleChangeCurrency={this.handleChangeCurrency}
        />
      </div>
    );
  }
}

const CurrencySwicher = props => {
  return (
    <button onClick={props.handleChangeCurrency}>
      Current currency: {props.currency}. Change it!
    </button>
  );
};

const Display = props => {
  return <p>Current currency: {props.currency}</p>;
};
```

![currency](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/react/1.gif)

## 事件

React 提供了一种简单的方式管理事件。准备和 `addEventListener` 说再见 :)

在之前文章中关于状态的例子，你可以看到：

```javascript
const CurrencySwitch = props => {
  return (
    <button onClick={props.handleChangeCurrency}>
      Current currency: {props.currency}. Change it!
    </button>
  );
};
```

如果你使用 JavaScript 已经有一段时间了，这看起来像是普通的旧的 [JavaScript 事件处理器](https://flaviocopes.com/javascript-events/)，只是这次你在 JavaScript 中定义的一切，而不是 HTML，并且你传递的是一个函数，而非字符串。

事实上，事件名称有些许不同，因为在 React 中你使用了小驼峰形式，所以 `onclick` 变成 `onClick`，`onsubmit` 变成了 `onSubmit`。

参考：这是老式的 HTML 混合了 JavaScript 事件：

```html
<button onclick="handleChangeCurrency()">
  ...
</button>
```

### 事件处理器

将事件处理程序定义在组件类上是一种约定：

```javascript
class Converter extends React.Component {
  handleChangeCurrency = event => {
    this.setState({
      currency: this.state.currency === '€' ? '$' : '?'
    });
  };
}
```

所有的处理程序都接收一个事件对象，该事件对象跨浏览器，并且遵守 [W3C UI 事件规范](https://www.w3.org/TR/DOM-Level-3-Events/)。

### 在方法中绑定 `this`

不要忘记绑定方法。ES6 类的方法默认不绑定。这意味着除非你讲方法定义为箭头函数，否则 `this` 将是未定义的：

```javascript
class Converter extends React.Component {
  handleClick = event => {
    /*...*/
  };
  // ...
}
```

当使用带有 Babel 的属性初始值设定语法时（在 create-react-app 中默认启用）会绑定 `this`，否则，你需要在构造函数中手动绑定：

```javascript
class Converter extends React.Component {
  constructor(props) {
    super(props);
    this.hanleClick = this.handleClick.bind(this);
  }
  handleClick(e) {}
}
```

### 事件参考

有大量事件已经被支持，这里是一个摘要列表：

#### Clipboard

- onCopy
- onCut
- onPaste

#### Composition

- onCompositionEnd
- onCompositionStart
- onCompositionUpdate

#### Keyboard

- onKeyDown
- onKeyPress
- onKeyUp

#### Focus

- onFocus
- onBlur

#### Form

- onChange
- onInput
- onSubmit

#### Mouse

- onClick
- onContextMenu
- onDoubleClick
- onDrag
- onDragEnd
- onDragEnter
- onDragExit
- onDragLeave
- onDragOver
- onDragStart
- onDrop
- onMouseDown
- onMouseEnter
- onMouseOver
- onMouseLeave
- onMouseMove
- onMouseOut
- onMouseUp

#### Selection

- onSelect

#### Touch

- onTouchCancel
- onTouchEnd
- onTouchMove
- onTouchStart

#### UI

- onScroll

#### Mouse Wheel

- onWheel

#### Media

- onAbort
- onCanPlay
- onCanPlayThrough
- onDurationChange
- onEmptied
- onEncrypted
- onEnded
- onErroe
- onLoadedData
- onLoadedMetadata
- onLoadStart
- onPause
- onPlay
- onPlaying
- onProgress
- onRateChange
- onSeeked
- onStalled
- onSuspend
- onTimeUpdate
- onVolumeChange
- onWating

#### Image

- onLoad
- onError

#### Animation

- onAnimationStart
- onAnimationEnd
- onAnimationIteration

#### Transition

- onTransitionEnd

## React 声明式

你将会遇到将 React 描述为声明式构建 UIs 的方法的文章。

查看[声明式编程](https://flaviocopes.com/javascript-functional-programming/#declarative)来查看更多关于声明式编程。

### React 声明式

React 使得“声明式方法”非常流行，因此它也随着 React 一起渗透到了前端开发的世界。

它真的是一个非常新的概念，而且 React 为构建 UIs 带来了比 HTML 模板更多的声明式：即使不用直接接触 DOM，你也可以构建 Web 接口，你拥有一个事件系统，为不用去和实际的 DOM 事件交互。

例如，使用 jQuery 或者 DOM 事件在 DOM 中查找元素是一种迭代方法。

React 的声明式为我们抽象了这些东西。我们只需要告诉 React 我们需要以一种特定的方式来渲染一个组件，而且我们可以永远不和 DOM 交互。

## Virtual DOM

许多现存的，在 React 出现之前的框架，在每次更改时都会直接操作 DOM。

### “真实的” DOM

什么是 DOM，首先 DOM（Document Object Model）是页面的树表示，从 `<html>` 标签开始，向下进入每个子节点，称之为节点。

它保存在浏览器内存中，并且直接链接到你在页面中看到的内容。DOM 有一套 API 供你使用，可以访问每个单独的节点，筛选它们，修改它们。

API 你可能见过很多次，如果你没有使用 jQuery 提供的抽象 API：

```javascript
document.getElementById(id)
document.getElementsByTagName(name)
document.createElement(name)
parentNode.appendChild(node)
element.innerHTML
element.style.left
element.getAttribute()
element.setAttribute()
element.addEventListener()
window.content
window.onload
window.dump()
window.scrollTo()
```

React 保留了 DOM 表示的副本，用来解决 React 的渲染：Virtual DOM。
