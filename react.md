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

当时，谷歌宣布了 Angular 2.x，但它带来了重大变化及不向后兼容。从 Angular 1 迁移到 2 就像迁移到不同的框架，因此，随着 React 承诺的执行速度的改善，使得它成为开发者基于尝试的东西。

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

使用 JSX 最大的好处就是你只于 Javascript 对象交互，而不是模板字符串。

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

