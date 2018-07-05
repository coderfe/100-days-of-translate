# History API

> History API 是浏览器允许你与地址栏和导航历史交互的一种方式。

## 简介

History API 允许你与浏览器历史记录交互、触发浏览器导航方法以及改变地址栏的内容。

在和现代单页应用结合时尤其有用，在这种程序中，你不会去服务端请求新的页面，而页面总是相同的，改变的只是其中的内容。

运行在现代浏览器上的应用，无论是显式的还是在框架级别都不与 History API 交互，对用户来说都是一种糟糕的体验，因为后退和前进按钮会中断。

当导航应用程序时，视图会发生改变，但地址栏则不会。

同样，重新加载按钮也会终端：重新加载页面时，由于没有深度链接，将导致浏览器显示不同的页面。

History API 是在 HTML5 中引入的，现在所有现代浏览器都支持它。IE 从 IE 10 开始支持，如果你需要支持 IE9 或者更旧的浏览器，可以使用 [History.js](https://github.com/browserstate/history.js/) 库。

## 访问 History API

History API 在 `window` 对象上可用，所以你可以这样调用它：`window.history` 或者简单地 `history`，因为 `window` 是全局对象。

![history](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/history-api/1.png)

## 浏览历史

让我们从使用 History API 能够做的最简单的事情开始。

返回前一页：

```javascript
history.back();
```

这个方法将会回到会话历史记录的上一个记录。你可以进入到下一个页面：

```javascript
history.forward();
```

这就相当于使用浏览器的后退和前进按钮。

`go()` 允许你向前或向后导航多个级别。例如：

```javascript
history.go(-1); // 等价于 history.back()
history.go(-2); // 等价于调用 2 次 history.back()
history.go(1); // 等价于 history.forward()
history.go(3); //  等价于调用 3 次 history.forward()
```

想要知道历史记录中有多少条记录时，你可以调用：

```javascript
history.length;
```

## 向历史记录添加记录

使用 `pushState()` 你可以以编程的方式添加一条历史记录。你要传入 3 个参数。

第一个参数是可包含任意内容的对象（但是它存在大小限制，并且对象需要被序列化）。

第二个参数目前未被主流浏览器使用，你可以传入一个空字符串。

第三个参数是与新状态关联的 URL。需要注意的是 URL 需要和当前 URL 在同一域名下。

```javascript
const state = {
  foo: 'bar'
};
history.pushState(state, '', '/foo');
```

调用这个方法不会改变页面的内容，也不会引起任何浏览器行为，例如 `window.location`。

## 修改历史记录

`pushState()` 允许你想历史中添加新状态，`replaceState` 允许你编辑当前的状态。

```javascript
history.pushState({}, '', '/posts');
const state = { post: 'first' };
history.pushState(state, '', '/post/first');
const state = { post: 'second' };
history.replaceState(state, '', '/post/second');
```

如果你调用 `history.back()`，浏览器会直接到达 `/posts`，因为 `/post/first` 被 `/post/second` 替换了。

## 访问当前历史记录状态

访问属性 `history.state` 会返回当前状态对象（传递给 `pushState` 或 `replaceState` 的第一个参数）。

## `ONPOPSTATE` 事件

每次活动历史状态发生改变时，`window` 会调用这个事件，当前状态作为回调函数的参数。

```javascript
window.onpopstate = event => {
  console.log(event.state);
};
```

每次你调用 `history.back()`、`history.forward()` 和 `history.go()` 时，将会输出新状态对象（传递给 `pushState` 或 `replaceState` 的第一个参数）。
