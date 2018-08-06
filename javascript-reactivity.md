# JavaScript 响应性的最佳解释

许多前端 JavaScript 框架（如 Angular、React 及 Vue）都有它们自己的响应式引擎。通过理解响应式是什么以及它如何工作，能够提升你的开发技巧，并且更高效地使用 JavaScript 框架。在下面的视频和文章中，我们构建了一些在 Vue 源码中可以看到的相同的响应式。

[![Youtube 视频](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/video-post.png)](https://youtu.be/7Cjb7Xj8fEI)

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

Vue 中我们在 `price` 或 `quantity` 更新时想让 `total` 也得到更新。我们想要：

```plain
>> total is 40
```

不幸的是，JavaScript 是程序，不是响应式的，所以无法在现实中工作。为了使 `total` 可响应式，我们必须使用 JavaScript 来使事物表现得不同。

## ⚠ 问题

我们需要保存是如何计算 `total` 的，由此，当 `price` 或 `quantity` 发生变化时我们可以重新计算它的值。

## ✅ 解决方法

首先，我们需要告诉应用程序，“把我即将运行的代码保存起来，下次我可能还需要你运行它”。然后我们运行代码，如果 `price` 或 `quantity` 变量更新时，再次运行存储过的代码。

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

它会遍历我们存储在 storage 数组中的所有匿名函数，并逐个执行。

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

![console](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/3.png)

## ⚠ 问题

我们可以根据需要继续记录目标，但是如果有一个强大的解决方案能够根据我们的应用程序伸缩，那将非常好。或许是一个 class 负责维护一系列的目标，在我们需要重新运行它们时，它会得到通知。

## ✅ 解决方案：依赖类

我们开始解决这个问题的一个方法，是把这些行为封装到它己的类中，实现了标准编程的观察者模式的一个**依赖类**。

因此，如果我们创建一个 JavaScript 类来管理我们的依赖（这更加接近 Vue 的处理方式），它看起来是这样的：

```javascript
class Dep {
  // Stands of dependency
  constructor() {
    // The targets that are dependent, and should be
    // run when notify() is called
    this.subscribers = [];
  }
  // This replaces our record function
  depend() {
    if (target && !this.subscribers.includes(target)) {
      // Only if there is a target & it's not already subscribed
      this.subscribers.push(target);
    }
  }
  // Replaces our replay function
  notify() {
    // Run our targets or observers
    this.subscribers.forEach(sub => sub());
  }
}
```

注意，现在我们把匿名函数存储在了 `subscribers`，而不是 `storage` 变量中。用 `depend` 函数代替 `record` 函数，用 `notify` 代替 `replay`。为了让这个可以运行：

```javascript
const dep = new Dep();

let price = 5;
let quantity = 2;
let total = 0;
let target = () => {
  total = price * quantity;
};
dep.depend();
target();

console.log(total); // 10 .. The right number
price = 20;
console.log(total); // 10 .. No longer the right number
dep.notify();
console.log(total); // 40 .. Now the right number
```

它仍然可以工作，而且现在我们的代码更能够复用。唯一有点奇怪的是 `target` 的初始化和运行。

## ⚠ 问题

以后我们为每个变量设置一个 Dep 类，并且良好的地封装了创建需要监听更新的匿名函数的行为。或许 `watcher` 函数可以用来处理这些行为。

与其这样调用：

```javascript
let target = () => {
  total = price * quantity;
};
dep.depend();
target();
```

（这是上面的代码）

我们可以这样调用：

```javascript
wacther(() => {
  total = price * quantity;
});
```

## ✅ 结局方案：观察者函数

在观察者函数内部我们可以做一些简单的事情：

```javascript
function watcher(myFunc) {
  target = myFunc; // Set as active target
  dep.depend(); // Add the active target as a dependency
  target(); // Call the target
  target = null; // Reset the target
}
```

正如你看到的，`wacther` 函数接受一个 `myFunc` 参数，把它设置为我们的全局 `target` 属性，调用 `dep.depend()` 把 target 添加为 subscribers，调用 `target()` 函数，然后重置 `target`。

现在我们运行下面的代码：

```javascript
price = 20;
console.log(total);
dep.depend();
console.log(total);
```

![console](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/3.png)

你可能会疑惑为什么把 `target` 时限为一个全局变量，而不是将其传递到我们需要它的函数中。对此是由原因的，在文章的最后答案会清晰可见。

## ⚠ 问题

我们只有一个 Dep 类，但我们想要的是每个变量拥有自己的 Dep 类。在我们进一步深入之前，让我先把这些东西设置为属性。

```javascript
let data = { price: 5, quantity: 2 };
```

我们假设一下，每个属性（`price` 和 `quantity`）都有它们子集的 Dep 类。

![price-quantity-dep](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/4.png)

现在我们运行：

```javascript
watcher(() => {
  total = data.price * quantity;
});
```

当访问 `data.price` 值时，我想 `price` 的 Dep 类能够把匿名函数（存储在 `target` 中）push 到它的 subscribers 数组中（通过调用 `dep.depend()`）。当访问 `data.quantity` 值时，我也希望 `quantity` 的 Dep 类能够把匿名函数（存储在 `target` 中）push 到它的 subscribers 数组中。

![data-price-quantity-dep](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/5.png)

如果有另外一个匿名函数，只访问 `data.price`，我想它只能 push `price` 属性到 Dep 类。

![data-price-dep](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/6.png)

我在什么时候想在 `price` 的 subscribers 上调用 `dep.notify()`？我想在 `price` 被设置的时候调用。在文章结束时，我能够进入控制台并执行以下操作：

![data-price-dep](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/7.png)

我们需要某种方式来链接数据的属性（像 `price` 和 `quantity`），所以当它被访问时，我们可以将 `target` 保存到 subscribers 数组中，并且在其发生变化时运行存储在 subscribers 数组中的函数。

## ✅ 解决方案：Object.defineProperty()

我们需要了解 [Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)，它只是简单的 ES5 的功能。它允许为一个属性设置 getter 和 setter 函数。在我演示它如何与我们的 Dep 类一起使用之前，让我为你演示一下基本用法：

```javascript
let data = { price: 5, quantity: 2 };

Object.defineProperty(data, 'price', {
  // Create a get method
  get() {
    console.log(`I was accessed`);
  },

  // Create a set method
  set(newVal) {
    console.log(`I was changed`);
  }
});
data.price; // This calls get()
data.price = 20; // This calls set()
```

![object-defineproperty](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/8.png)

如你所见，它只打印了两行。无论怎样，它实际上不会 `get` 或 `set` 任何值，因为我们重写了该功能。我们来把它加回去。`get()` 期望返回一个值，`set()` 则需要更新一个值，所以添加一个 `internalValue` 变量来存储当前 `price` 值。

```javascript
let data = { price: 5, quantity: 2 };

let internalValue = data.price;

Object.defineProperty(data, 'price', {
  // Create a get method
  get() {
    console.log(`Getting price: ${internalValue}`);
    return internalValue;
  },

  // Create a set method
  set(newVal) {
    console.log(`Setting price to: ${newVal}`);
    internalValue = newVal;
  }
});
total = data.price * quantity; // This calls get()
data.price = 20; // This calls set()
```

现在 get 和 set 可以正常运行了，你认为控制台会打印什么呢？

![price-get-set](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/9.png)

所以现在当我们 get 或者 set 值的时候，我们有种方式可以得到通知。通过一些循环，我们可以为 data 数组中的每一项都运行，对吧？

仅供参考，`Object.keys()` 会返回对象的键的数组。

```javascript
let data = { price: 5, quantity: 2 };

Object.keys(data).forEach(key => {
  let internalValue = data[key];
  Object.defineProperty(data, key, {
    get() {
      console.log(`Getting ${key}: ${internalVal}`);
      return internalValue;
    },
    set(newVal) {
      console.log(`Setting ${key} to: ${newVal}`);
      internalVal = newVal;
    }
  });
});
total = data.price * data.quantity;
data.price = 20;
```

现在所有属性都拥有 getter 和 setter，让我们看一下控制台：

![data-getter-setter](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/10.png)

## 🛠 把两种想法结合起来

```javascript
total = data.price * data.quantity;
```

当这部分代码可以运行而且可以得到 `price` 的值时，我们想让 `price` 记住这个匿名函数（`target`）。这样一来，如果 `price` 发生变化，或者设置了新值时，它将触发这个函数再次运行，因为它知道这行代码依赖于依赖于它。因此，你可以这样思考。

**Get** => 记住这个匿名函数，当值发生变化时再次运行它。

**Set** => 运行保存的匿名函数，值就会发生变化。

或者在我们的 Dep 类中：

**Price accessed** => 调用 `dep.depend()` 保存当前 `target`。

**Price Set** => 在 price 上调用 `dep.notify()`，再次运行全部 `targets`。

让我们组合这两种想法，串联出我们最终的代码：

```javascript
let data = { price: 5, quantity: 2 };
let target = null;

class Dep {
  constructor() {
    this.subscribers = [];
  }

  depend() {
    if (target && !this.subscribers.includes(target)) {
      this.subscribers.push(target);
    }
  }

  notify() {
    this.subscribers.forEach(sub => sub());
  }
}

Object.keys(data).forEach(key => {
  let internalValue = data[key];
  const dep = new Dep();
  Object.defineProperty(data, key, {
    get() {
      dep.depend();
      return internalValue;
    },
    set(newVal) {
      internalValue = newVal;
      dep.notify();
    }
  });
});

function watcher(myFunc) {
  target = myFunc;
  target();
  target = null;
}

watcher(() => {
  data.total = data.price * data.quantity;
});
```

现在让我们来看看控制台会发生什么：

![final-code-console](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/11.png)

正是我们所期望的！`price` 和 `quantity` 确实是响应式的！我们的 total 代码会在 `price` 或者 `quantity` 更新时重新运行。

Vue 文档的插图现在应该有意义了。

![vue-doc-illustration](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/12.png)

你看到那个带着 getter 和 setter 的漂亮的紫色的 Data 圆了么？它看起来应该很熟悉！每个组件实例都有一个 watcher 实例（蓝色的），它从 getter（红色的线条） 收集依赖性。稍后调用 setter 时，它会通知 watcher 来重新渲染组件。这里有一张图片和我自己的一些注释：

![my-own-annotations](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-reactivity/13.png)

是的，现在这些是不是更有意义了呢？

很明显，Vue 在底层做了很复杂的封装，但是现在你已经了解了基本原理。

## ⏪ 那么我们学到了什么？

- 如何创建 **Dep 类**来收集依赖（depend），并且运行所有依赖（notify）。
- 如何创建 **watcher** 来管理我们正在运行的代码，这可能需要作为依赖类添加。
- 如何使用 **Object.defineProperty()** 创建 getter 和 setter。
