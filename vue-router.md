# Vue Router

> 探讨 Vue 程序的一个重要部分：路由。

<!-- TOC -->

- [Vue Router](#vue-router)
  - [简介](#简介)
  - [安装](#安装)
  - [路由对象](#路由对象)
  - [定义路由](#定义路由)
  - [使用命名路由在路由的 push 和 replace 方法中传参](#使用命名路由在路由的-push-和-replace-方法中传参)
  - [用户点击 `router-link` 时发生了什么](#用户点击-router-link-时发生了什么)
  - [路由守卫](#路由守卫)
  - [动态路由](#动态路由)
  - [使用 Props](#使用-props)
  - [嵌套路由](#嵌套路由)

<!-- /TOC -->

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
import Vue from 'vue';
import VueRouter from 'vue-router';

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
this.$router.push('about'); //named route, see later
this.$router.push({ path: 'about' });
this.$router.push({ path: 'post', query: { post_slug: 'hello-world' } }); //using query parameters (post?post_slug=hello-world)
this.$router.replace({ path: 'about' });
```

`go()` 用来后退或者前进，接收一个正数或者负数以回到历史记录中。

```javascript
this.$router.go(-1); //go back 1 step
this.$router.go(1); //go forward 1 step
```

## 定义路由

我在这个例子中使用 [Vue 单文件组件](https://github.com/coderfe/100-days-of-translate/blob/master/vue-single-file-components.md)。

在模板中我使用了有 3 个 `router-link` 组件的 `nav` 标签，它们拥有一个标签（Home/Login/About）并且为其 `to` 属性分配了 URL。

Vue Router 会将匹配当前 URL 的内容放在 `router-view` 组件中。

```html
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/login">Login</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>
```

`router-link` 组件默认会渲染为 `a` 标签（你可以改变它）。每次通过点击链接或者改变 URL 使路由发生变化时，一个 `router-link-active` 类名会被添加到当前路由对应的元素上，允许你为其定义样式。

在 Javascript 的部分中，我们首先引入并安装了路由，然后定义了 3 个路由组件。

我们将其传递到 `router` 对象初始化过程中，并且把这个对象传递到 Vue 根实例中。

这里是代码：

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

const Home = {
  template: '<div>Home</div>'
};

const Login = {
  template: '<div>Login</div>'
};

const About = {
  template: '<div>About</div>'
};

const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/login',
      component: Login
    },
    {
      path: '/about',
      component: About
    }
  ]
});

new Vue({
  router
}).$mount('#app');
```

通常，在 Vue 程序中你可以通过这样来初始化并且挂载根程序：

```javascript
new Vue({
  render: h => h(App)
}).$mount('#app');
```

当你使用 Vue Router 时，不必传递 `render` 属性，而是使用 `router`：

```javascript
new Vue({
  router
}).$mount('#app');
```

它是下面这种方式的速记形式：

```javascript
new Vue({
  router: router
}).$mount('#app');
```

从这个例子中可以看出，我们传入 `routes` 数组到 `VueRouter` 的构造函数中。数组中的每个路由都有 `path` 和 `component` 参数。

如果你传入 `name` 参数，那你就拥有了一个**命名路由**。

## 使用命名路由在路由的 push 和 replace 方法中传参

还记得之前我们使用 Router 对象来 push 新的状态么？

```javascript
this.$router.push({ path: 'about' });
```

对于命名路由，我么可以这样为新路由传参：

```javascript
this.$router.push({ name: 'post', query: { post_slug: 'hello-world' } });
```

同样适用于 `replace()`：

```javascript
this.$router.replace({ name: 'post', query: { post_slug: 'hello-world' } });
```

## 用户点击 `router-link` 时发生了什么

应用程序会渲染与传入链接相匹配的 URL 的路由组件。

处理 URL 的新路由组件会被实例化并调用其守卫，旧的路由组件将会被销毁。

## 路由守卫

既然我们提到了**守卫**，那就介绍一下它。

你可以认为它们时生命周期钩子，这些函数会在应用程序执行的特定时间被调用。你可以跳转并更改路由的执行，重定向或者简单地取消请求。

你可以为通过为 router 添加 `beforeEach()` 和 `afterEach()` 回调函数来使用全局守卫。

- `beforeEach()` 会在导航确认之前被调用
- `beforeResolve()` 在 beforeEach 执行之后，并且所有组件的 `beforeRouterEnter` 和 `beforeRouterUpdate` 被调用之后，但是在导航确认之前执行。如果你想要最后的检查。
- `afterEach()` 会在导航确认之后调用

“导航确认之前”是什么意思？我们一会来了解。于此同时，将它认为“应用程序可以到达那个路由”。

用法是这样的：

```javascript
this.$router.beforeEach((to, from, next) => {
  // ...
});
```

```javascript
this.$router.afterEach((to, from) => {
  // ...
});
```

`to` 和 `from` 分别代表路由对象要进入的路由和即将离开的路由。

单文件路由组件也有守卫。

- `beforeRouteEnter(from, to, next)` 当前路由确认前被调用
- `beforeRouteUpdate(from, to, next)` 当路由改变但是管理它的组件仍然相同时调用（动态路由，参见一部分）
- `beforeRouteLeave(from, to, next)` 当路由离开时调用

我们提到过导航。要确定对路由的导航能否被确认，Vue Router 会执行一些检查：

- 在当前组件调用 `beforeRouteLeave`
- 调用 router `beforeEach` 守卫
- 在任何需要重用的组件中调用 `beforeRouteUpdate`，如果存在的话
- 在路由对象上调用 `beforeEach`（我没有提到，但是你可参考[这里](https://router.vuejs.org/guide/advanced/navigation-guards.html#per-route-guard)）
- 在我们要进入的组件中调用 `beforeRouteEnter`
- 调用 router 的 `beforeResolve()`
- 如果一切顺利，导航就会被确认
- 调用 router 的 `afterEach` 守卫

你可以使用特定路由守卫（在动态路由情况下的 `beforeRouteEnter` 和 `beforeRouteUpdate`）作为生命周期钩子，例如你可以开始**数据获取请求**。

## 动态路由

上面的例子展示了基于 URL 的不同视图，处理 `/`、`/home`、`/about` 路由。

一个常见的需求是处理动态路由，例如所有的帖子都位于 `/posts/`，每个帖子都有一个名称：

- `/posts/first`
- `/posts/another-post`
- `/posts/hello-world`

你可以通过使用动态部分来实现一点。

这些时静态部分：

```javascript
const router = new VueRouter({
  routes: [
    { path: '/', component: Home },
    { path: '/login', component: Login },
    { path: '/about', component: About }
  ]
});
```

我们添加动态部分来处理博客帖子：

```javascript
const router = new VueRouter({
  routes: [
    { path: '/', component: Home },
    { path: '/post/:post_slug', component: Post },
    { path: '/login', component: Login },
    { path: '/about', component: About }
  ]
});
```

注意 `:post_slug` 语法。这意味着你可以使用任何字符串，它将会映射到 `post_slug` 占位符。

你不限于使用这种语法。Vue 依赖[这个库]()解析动态路由，并且你可以和[正则表达式](https://github.com/coderfe/100-days-of-translate/blob/master/a-guide-to-javascript-regular-expressions.md)一起使用。

现在在 Post 路由组件内部我们可以使用 `$route` 来引用路由，而且可以通过 `$route.params.post_slug` 来使用帖子名称。

```javascript
const Post = {
  template: '<div>Post: {{ $route.params.post_slug }}</div>'
};
```

我们可以使用这个参数从后端加载数据。

你可以在同一个 URL 中拥有任意数量的动态部分：`/post/:author/:post_slug`。

还记得之前我们讨论的当用户导航到新的路由时会发生什么么？

在动态路由的情况下，所发生的有一些不同。

Vue 为了更加高效，它不会销毁当前路由组件并重新初始化，而是重用了当前实例。

当这种情况发生时，Vue 调用 `beforeRouteUpdate` 生命周期事件。在那里你可以执行任何你需要的操作：

```javascript
const Post = {
  template: '<div>Post: {{ $route.params.post_slug }}</div>',
  beforeRouteUpdate(to, from, next) {
    console.log(`Updating slug from ${from} to ${to}`);
    next();
  }
};
```

## 使用 Props

在这个例子中，我使用 `$route.params.*` 来访问路由数据。一个组件不应该与路由耦合的太紧，相反，我们可以使用 props：

```javascript
const Post = {
  props: ['post_slug'],
  template: '<div>Post: {{ post_slug }}</div>'
};

const router = new VueRouter({
  routes: [
    {
      path: '/post/:post_slug',
      component: Post,
      props: true
    }
  ]
});
```

注意传入到路由对象的 `props: true` 参数启用了此功能。

## 嵌套路由

之前我提到你可以在同一个 URL 中拥有任意数量的动态部分，例如这样：`/post/:author/:post_slug`。

那么，假设我们有一个 Author 组件来负责第一个动态部分：

```html
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(Router);

const Author = {
  template: '<div>Author: {{ $route.params.author}}</div>'
};

const router = new VueRouter({
  routes: [{ path: '/post/:author', component: Author }]
});

new Vue({
  router
}).$mount('#app');
</script>
```

我们可以在 Author 模板中插入第二个 `router-view` 组件实例：

```javascript
const Author = {
  template:
    '<div>Author: {{ $route.params.author}}<router-view></router-view></div>'
};
```

我们添加 Post 组件：

```javascript
const Post = {
  template: '<div>Post: {{ $route.params.post_slug }}</div>'
};
```

然后我们将在 Vue Router 配置中注入内部动态路由：

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/post/:author',
      component: Author,
      children: [{ path: ':post_slug', component: Post }]
    }
  ]
});
```
