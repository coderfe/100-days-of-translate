# Vue Router

探讨 Vue 程序的一个重要部分：路由。

## 简介

在 Javascript Web 程序中，路由是将当前显示的内容和浏览器地址栏内容同步的部分。

换句话说，当你在页面点击某个内容是，它会使 URI 发生变化，而且它可以在你点击特定的 URL 时显示正确的视图。

传统的 Web 时围绕 URLs 构建的。当你点击确定的 URL 时，一个特定的页面会显示出来。

随着运行在浏览器内部的应用程序的引入，以及用户看到的变化，许多应用程序打破了这种交互，你必须使用浏览器的 [History API](https://flaviocopes.com/history-api/) 手动更新 URL。

当你需要在程序中同步 URLs 和 视图时，你就需要一个路由。这是一种常见的需求，现在多数现代框架都允许你管理路由。

Vue Router 库是 Vue.js 的发展方向。Vue 不强制使用这个库。你可以使用自己想用的任何通用路由库，或者也可以创建你自己的 History API 集成，但是使用 Vue Router 的好处在于：它是官方的。

这意味着它由维护 Vue 的同一些人维护着，所以你可以获得更加一致的集成，并且可以保证在未来，无论如何都会保持兼容。

## 安装

Vue Router 在 npm 上可用，其包名为 `vue-router`。

如果你通过 script 标签使用 Vue，你可以这样引入 Vue Router：

```html
<script src="https://unpkg.com/vue-router"></script>
```

> unpkg.com 是一个非常方便的工具，它可以通过一个简单的链接使每个 npm 包在浏览器中可用。

如果你是用 [Vue CLI]()，可以这样安装：

```shell
npm install vue-router

// or

yarn add vue-router
```

无论你是用 script 标记还是 Vue CLI 安装了 Vue Router，你现在可以在应用程序中导入它了。

你可以在 Vue 之后导入它，并且需要调用 `Vue.use(VueRouter)` 将其安装到程序中：

```javascript
import Vue from "vue";
import VueRouter from "vue-router";

Vue.use(VueRouter);
```

调用 `Vue.use()` 传递路由对象之后，你可以在程序中的任何组件中访问这些对象。

- `this.$router` 是路由对象
- `this.$route` 是当前路由对象

## 路由对象

当 Vue Router 安装在 Vue 根组件上时，你可以在任何组件中通过 `this.$router` 来访问路由对象，他提供了一些非常好用的功能。

我们可以将程序导航到新路由通过使用这些方法：

- `this.$router.push()`
- `this.$router.replace()`
- `this.$router.go()`

分别到对应 History API 的 `pushState`、`replaceState` 及 `go` 方法。

`push()` 用来计入一个新的路由，会在浏览器历史记录中添加一条记录。`replace()` 也是一样的用法，只是它不会生成新的记录。

用法示例：

```javascript
this.$router.push("about"); //named route, see later
this.$router.push({ path: "about" });
this.$router.push({ path: "post", query: { post_slug: "hello-world" } }); //using query parameters (post?post_slug=hello-world)
this.$router.replace({ path: "about" });
```

`go()` 用来后退或者前进，接收一个正数或者负数以回到历史记录中。

```javascript
this.$router.go(-1); //go back 1 step
this.$router.go(1); //go forward 1 step
```

## 定义路由
