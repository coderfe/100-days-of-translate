# JavaScript 响应性的最佳解释

许多前端 JavaScript 框架（如 Angular、React 及 Vue）都有它们自己的响应式引擎。通过理解响应式是什么以及它如何工作，能够提升你的开发技巧，并且更高效地使用 JavaScript 框架。在下面的视频和文章中，我们构建了一些在 Vue 源码中可以看到的相同的响应式。

<iframe width="560" height="315" src="https://www.youtube.com/embed/7Cjb7Xj8fEI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

> 如果你在看视频而不是阅读这篇文章，查看[这个系列的下一个视频](https://www.vuemastery.com/courses/advanced-components/evan-you-on-proxies/)和 Vue 的创始人 Evan You 讨论响应式和代理。

## 💡 响应式系统

当你看到 Vue 的响应式系统第一次工作时，它看起来很神奇。拿这个简单的 Vue 应用来看：

```html
<div id="app">
  <div>Price: ${{ price }}</div>
  <div>Total: ${{ price * quantity }}</div>
  <div>Taxes: ${{ totalPriceWithTax }}</div>
</div>
```

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      price: 5.0,
      quantity: 2
    },
    computed: {
      totalPriceWithTax() {
        return this.price * this.quantity * 1.03;
      }
    }
  });
</script>
```

Somehow，Vue 只知道如果 `price` 发生变化，它应该做 3 件事：

- 更新页面中 `price` 的值
- 重新计算表达式 `price * quantity` 的值，并更新页面
- 再次调用 `totalPriceWithTax` 函数，并更新页面

但是等一下，我听到了你的疑惑，Vue 怎么知道当 `price` 变化时该更新什么，并且 Vue 是如何跟踪这一切的呢？

![javascript-reactivity](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/1.jpeg)

这通常不是 JavaScript 编程所做的工作。

如果你不是很明白，我们必须要解决的一个大问题是编程通常不以这种方式工作。例如，我运行这些代码：

```javascript
let price = 5;
let quantity = 2;
let total = price * quantity; // 10 right?
price = 20;
console.log(`total is ${total}`);
```

你认为它将输出什么？由于我们没有使用 Vue，它将输出 `10`：

```plain
>> total is 10
```

在 Vue 中我们在 `price` 或者 `quantity` 更新时想让 `total` 也得到更新。我们想要：

```plain
>> total is 40
```

不幸的是，JavaScript 是程序，不是响应式的，所以无法在现实中工作。为了使 `total` 可响应式，我们必须使用 JavaScript 来使事物表现得不同。

## ⚠ 问题

我们需要保存我们是如何计算 `total` 的，由此，当 `price` 或 `quantity` 发生变化时我们可以重新计算它的值。

## ✅ 解决方法

首先，我们需要告诉应用程序，“把我即将运行的代码它保存起来，下次我可能还需要你运行它”。然后我们运行代码，如果 `price` 或 `quantity` 变量更新时，再次运行存储过的代码。

![store-code](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/2.png)

我们可以通过记录函数来做到这一点，所以我们可以再次运行它：

```javascript
let price = 5;
let quantity = 2;
let total = 0;
let target = null;

target = function() {
  total = price * quantity;
};

record(); // Remember this in case we want to run it later
target(); // Also go ahead and run it
```

注意，我们在 `target` 变量内存储了匿名函数，然后调用了 `record` 函数。使用 ES6 箭头函数，我也可以这样写：

```javascript
target = () => {
  total = price * quantity;
};
```

`record` 的定义比较简单：

```javascript
let storage = []; // We'll store our target function in here

function record() {
  // target = () => { total = price * quantity }
  storage.psuh(target);
}
```

我们存储 `target`（在我们的例子中是 `{ total = price * quantity }`），因此我们可以在后面运行它，以及 `replay` 函数运行我们记录过的所有东西。

```javascript
function replay() {
  storage.forEach(run => run());
}
```

它将会遍历我们存储在 storage 数组中的所有匿名函数，并逐个执行。

然后在代码中，我们只要：

```javascript
price = 20;
console.log(total); // => 10
replay();
console.log(total); // => 40
```

足够简单吧？如果你需要多次阅读并掌握它，这里是完整的代码。仅供参考，如果你在想为什么，我是以一种特殊的方式编写这些代码。

```javascript
let price = 5;
let quantity = 2;
let total = 0;
let target = null;
let storage = [];

function record() {
  storage.push(target);
}

function replay() {
  storage.forEach(run => run());
}

target = () => {
  total = price * quantity;
};

record();
replay();

price = 20;
console.log(total); // => 10
replay();
console.log(total); // => 40
```

![entirety-code](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/2.png)
