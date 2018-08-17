# 10 分钟解释 Javascript Async/Await

很长一段时间里 Javascript 开发者都必须依赖回调处理异步代码。因此，我们许多人都经历过回调地狱，并且当面对[这样的函数时]()我们会感到恐惧：

![callback-hell](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/async-await/callback-hell.jpg)

幸运的是，then（或者说 `then()`）带来了 Promises。它们为回调提供了更加有条理的替代方案，而且大多数社区都很快地转而使用它。

现在，随着最近增加的 Async/Await，编写 Javascript 代码将变得更好。

## 什么是 Async/Await

Async/Await 是一个期待已久的 Javascript 功能，它使得处理异步函数更加愉快和易懂。它构建于 Promise 之上，兼容现存的基于 Promise 的 APIs。

其名称来自于 `async` 和 `await`——这两个关键字将会帮助我们整理异步代码。

**Async——声明一个异步函数（`async function sayName(){...}`）**

- 自动将常规函数转换为 Promise
- 当调用 async 函数时，可以解析它们内部返回的任何内容
- Async 允许使用 `await`

**Await——暂停执行异步函数（var result = await someAsyncCall()）**

- 当被置于 Promise 调用之前，`await` 强制让其它的代码等待直到 Promise 结束并且返回结果
- Await 只可以处理 Promise，它不能处理回调
- Await 只能用于 `async` 内部

下面是一个简单的例子：

假设我们想从服务器获取一些 JSON 文件。我们将使用 [axios](https://github.com/mzabriskie/axios) 写一个函数，并且向 [https://tutorialzine.com/misc/files/example.json](https://tutorialzine.com/misc/files/example.json) 发送 HTTP GET 请求。我们得等待服务器的响应，所以，这个 HTTP 请求自然将是异步的。

下面我们可以看到相同的函数实现了两次。第一个是 Promise，第二个使用了 Async/Await。

```javascript
const url = 'https://tutorialzine.com/misc/files/example.json';

// Promise approach
function getJSON() {
  return new Promise((resolve, reject) => {
    axios.get(url).then(function(json) {
      resolve(json);
    });
  });
}

// Async/Await approach
async function getJSON() {
  let json = await axios.get(url);
  return json;
}
```

Async/Await 版本的代码非常干净，并且更加简短和易读。除了使用的语法，两个函数完全相同——它们都返回 Promise，并且解析了从 axios 获取的 JSON。我们可以像这样调用 async 函数：

```javascript
getJSON().then(function(result) {
  // ...
});
```

## Async/Await 会使 promise 过时么？

不，根本不会。当我们使用 Async/Await 时，其实依旧在使用 Promises。更好地理解 Promises 实际上对我们有长远的帮助，非常推荐使用。

有些情况下，使用 Async/Await 并不会优化代码，所以我们不得不回到 Promises 以求帮助。一个场景是：当我们需要完成多个独立的异步调用并且等待至它们全部完成。

如果我们试着使用 async 和 await，就会发生下面的事情：

```javascript
async function getABC() {
  let A = await getValueA(); // getValueA 2 秒完成
  let B = await getValueB(); // getValueB 4 秒完成
  let C = await getValueC(); // getValueC 3 秒完成

  return A * B * C;
}
```

每个 await 调用都会等待前一个调用返回结果。因此，当我们调用整个函数将会花费 9 秒（2 + 4 + 3）。

这不是最佳的解决方案，因为这三个变量 `A`、`B`、`C` 都是互相独立的。换句话说，获取 `B` 之前我们并不需要知道 `A` 的值。我们可以同时获取它们，这样可以减少几秒等待时间。

要同时发送所有的请求就需要 `Promise.all()`。这将确保后面计算时我们仍拥有所有的结果，但是异步调用将会被并行触发，而不是一个接一个。

```javascript
async function getABC() {
  let results = await Promise([getValueA, getValueB, getValueC]);

  return results.reduce((total, value) => total * value);
}
```

这种方式花费的时间更少。`getValueA` 和 `getValueC` 将会在 `getValueC` 结束时完成。我们将有效地合并最慢的请求（`getValueB` 4 秒）执行的时间，而不是时间的累加值。

## 在 Async/Await 中捕获错误

Async/Await 的另一个优点是允许我们在 try/catch 块中捕获任何错误，我们只需要这样包装 `await` 调用：

```javascript
async function doSomethingAsync() {
  try {
    let result = await someAsyncCall();
  } catch (error) {
    // ...
  }
}
```

catch 块可以处理由等待的异步操作引起或者我们在 try 块编写的任何可能失败的代码引起的错误。

如果情况需要，我们也可以在执行 async 函数时捕获错误。所有的 async 函数都会返回 Promise，在我们调用它时，可以简单地包含一个 `.catch()` 事件处理器：

```javascript
async function doSomethingAsync(){
  let result = await someAsyncCall();
  return result;  
}

doSomethingAsync().
  .then(successHandler)
  .catch(errorHandler);
```

## 浏览器支持

Async/Await 已适用于大多数主流浏览器。除了 IE11 —— 其他所有厂商不需要扩展库就可以认可你的 async/await 代码。

![caniuse-async-await](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/async-await/caniuse-async-await.png)

Node 开发者也可以享受改进的异步流，只要他们使用 Node8 及以上版本。Node8 将在今年成为 LTS。

如果这个兼容性无法满足你，这里有几个 JS 转换库，像 [Babel](https://babeljs.io/docs/plugins/transform-async-to-generator/) 和 [TypeScript](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-3.html)，而且 Node.js 的 [asyncawait](https://github.com/yortus/asyncawait) 也提供了跨平台的版本。
