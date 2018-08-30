# 制作一个 Vue 长按指令

你是否曾想在 Vue 程序中创建一个只需按住几秒就可以执行一个功能的按钮？

你是否曾想创建这样一个按钮，按下它可以清除单次输入值（长按清空整个输入）？

你有过？我也是。那么你就来到了对的地方。

本文将解释如何通过按下（或按住）按钮来执行功能或者清空输入。

首先，我会讲解如何在 VanillaJS 中实现。然后，再创建 Vue 指令。

## 原理

要实现长按，用户需要按住按钮并持续几秒钟。

要在代码中实现这一点，在鼠标“单击”键按下时，我们需要在执行功能之前启动一个定时器来监听按住的时间，一旦这个时间超过我们设置的时间，就触发这个功能。

很简单！但是，首先我们需要知道用户何时按下按钮。

## 怎么做

用户点击按钮时，有两个事件会先于单击事件触发：**mousedown/mouseup**。

mousedown 事件会在按下鼠标按钮时调用，而 mouseup 会在松开按钮时调用。

我们需要做的是：

1. mousedown 事件发生时启动定时器
2. 只要 mouseup 事件在预设时间 2 秒之前执行，就将其当做是正常的单击事件。清除定时器，不执行相关函数。

只要在我们设置的时间之前定时器没有清除，也就是 mouseup 事件没有触发之前——我们可以认为用户没有松开按钮。
因此，可以认为它是长按，然后就可以执行相关功能。

## 实践

让我们深入代码来完成它。

首先，我们需要定义 3 件事，分别是：

1. 用来存储定时器的**变量**
2. 用来启动定时器的**启动函数**
3. 用来取消定时器的**取消函数**

### 变量

这个变量用来存储 setTimeout 的值，以便在 mouseup 事件发生时取消定时器。

```javascript
let pressTimer = null;
```

我们把变量设置为 null 是为了在取消定时器之前检查当前是否存在激活的定时器。

### 启动函数

这个函数包括 [setTimeout][settimeout]，它是 JavaScript 中的一个方法，允许我们在持续一段时间之后执行函数。

记住，在创建单击事件的过程中，有两个事件被触发。但是，我们要启动定时器的是对于 mousedown 事件。因此，如果是单击事件，我们无需启动定时器。

```javascript
let start = e => {
  // 确保不是单击事件
  if (e.type === 'click' && e.button === '0') {
    return;
  }
  // 确保在启动另一个定时器之前没有激活的定时器
  if (pressTimer === null) {
    pressTimer = setTimeout(() => {
      // 执行函数
    }, 1000);
  }
};
```

### 取消函数

这个函数就是其字面意思，在启动函数调用后取消之前创建的 setTimeout。

要取消 setTimeout，我们需要使用 JavaScript 中的 [clearTimeout][cleartimeout] 方法，它可以清除由 setTimeout 生成的定时器。

使用 clearTimeout 之前，我们首先需要检查变量 **pressTimer** 是否为 null。如果它不是 null，则意味着存在激活的定时器。因此，我们需要清除定时器，如你所想，把 **pressTimer** 设置为 null。

```javascript
let cancel = e => {
  if (pressTimer !== null) {
    clearTimeout(pressTimer);
    pressTimer = null;
  }
};
```

一旦 mouseup 事件触发，该函数会被调用。

## 设置触发器

剩下的就是在你想要应用长按效果的按钮上添加监听器了。

```javascript
addEventListener('mousedown', start);
addEventListener('click', cancel);
```

将代码组装在一起，我们会得到如下代码：

```javascript
let pressTimer = null;

let start = e => {
  if (e.type === 'click' && e.button === '0') {
    return;
  }

  if (pressTimer === null) {
    pressTimer = setTimeout(() => {
      console.log('Long Press');
    }, 1000);
  }
};

let cancel = e => {
  if (pressTimer !== null) {
    clearTimeout(pressTimer);
    pressTimer = null;
  }
};

let el = document.getElementById('longPressButton');

el.addEventListener('mousedown', start);

el.addEventListener('click', cancel);
el.addEventListener('mouseout', cancel);
```

## 封装为 Vue 指令

Vue 允许我们在全局或者组件内定义指令，本文将在全局定义指令。

让我们开始实现这个指令。

首先，我们需要声明自定义指令的名称。

```javascript
Vue.directive('longpress', {
  // ...
});
```

这段代码创建了一个名为 **v-longpresss** 的全局自定义指令。

接下来，我们添加带有参数的 [hook 函数][hook-function]，它允许我们引用该指令绑定的元素，获取传递给指令的值，并标识使用指令的组件。

```javascript
Vue.directive('longpress', {
  bind: funciton (el, binding, vNode) {

  }
});
```

接下来，把长按的实现代码放进 bind 函数。

```javascript
Vue.directive('longpress', {
  bind: function(el, binding, vNode) {
    let pressTimer = null;

    let start = e => {
      if (e.type === 'click' && e.button === '0') {
        return;
      }

      if (pressTimer === null) {
        pressTimer = setTimeout(() => {
          //...
        }, 1000);
      }
    };

    let cancel = e => {
      if (pressTimer !== null) {
        clearTimeout(pressTimer);
        pressTimer = null;
      }
    };

    el.addEventListener('mousedown', start);

    el.addEventListener('click', cancel);
    el.addEventListener('mouseout', cancel);
  }
});
```

然后，我们要添加一个方法来传递给 longpress 指令运行。

```javascript
Vue.directive('longpress', {
  bind: function(el, binding, vNode) {
    let pressTimer = null;

    let start = e => {
      if (e.type === 'click' && e.button === '0') {
        return;
      }

      if (pressTimer === null) {
        pressTimer = setTimeout(() => {
          handler();
        }, 1000);
      }
    };

    let cancel = e => {
      if (pressTimer !== null) {
        clearTimeout(pressTimer);
        pressTimer = null;
      }
    };

    const handler = e => {
      binding.value(e);
    };

    el.addEventListener('mousedown', start);

    el.addEventListener('click', cancel);
    el.addEventListener('mouseout', cancel);
  }
});
```

现在，我们可以在 Vue 程序中使用这个指令，直到用户在指令中传递了一个非函数类型的值。所以，当这种情况发生时，我们需要通过警告来阻止。

为了警告用户，我们在 bind 函数添加以下代码：

```javascript
if (typeof bind.value !== 'function') {
  const compName = vNode.context.name;
  let warn = `[longpress:] provided expression '${
    binding.expression
  }' is not a function, but has to be`;
  if (compName) {
    warn += `Found in component '${compName}'`;
  }
  console.warn(warn);
}
```

最后，让这个指令适用于触屏设备。因此，我们需要为 `touchstart/touchend/touchcancel` 添加事件监听器。

把所有代码放在一起：

```javascript
Vue.directive('longpress', {
  bind: function(el, binding, vNode) {
    if (typeof binding.value !== 'function') {
      const compName = vNode.context.name;
      let warn = `[longpress:] provided expression '${
        binding.value
      }' is not a function, but has to be`;
      if (compName) {
        warn += `. Found in component ${compName}`;
      }
    }

    let pressTimer = null;

    let start = e => {
      if (e.type === 'click' && e.button === '0') {
        return;
      }
      if (pressTimer === null) {
        pressTimer = setTimeout(() => {
          handler();
        }, 1000);
      }
    };

    let cancel = e => {
      if (pressTimer !== null) {
        clearTimeout(pressTimer);
        pressTimer = null;
      }
    };

    const handler = e => {
      binding.value(e);
    };

    el.addEventListener('mousedown', start);
    el.addEventListener('touchstart', start);

    el.addEventListener('click', cancel);
    el.addEventListener('mouseout', cancel);
    el.addEventListener('touchend', cancel);
    el.addEventListener('touchcancel', cancel);
  }
});
```

现在，在 Vue 组件中引用：

```html
<template>
  <div>
    <button v-longpress="incrementPlusTen" @click="incrementPlusOne">{{ value }}</button>
  </div>
</template>
<script>
  export default {
    name: 'App',
    data() {
      return {
        value: 10
      }
    },
    methods: {
      incrementPlusTen() {
        this.value += 10;
      },
      incrementPlusOne() {
        this.value++;
      }
    }
  }
</script>
```

[settimeout]: https://www.w3schools.com/jsref/met_win_settimeout.asp
[cleartimeout]: https://www.w3schools.com/jsref/met_win_cleartimeout.asp
[hook-function]: https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions
