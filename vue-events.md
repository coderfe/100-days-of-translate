# Vue.js 事件

> Vue.js 允许我们在 DOM 元素上使用 v-on 指令来捕获任何 DOM 事件。这个话题的关键在于让组件变得可交互。

<!-- TOC -->

- [Vue.js 事件](#vuejs-事件)
  - [什么是 Vue.js 事件](#什么是-vuejs-事件)
  - [访问原生事件对象](#访问原生事件对象)
  - [事件修饰符](#事件修饰符)

<!-- /TOC -->

## 什么是 Vue.js 事件

Vue.js 允许我们在 DOM 元素上使用 v-on 指令来捕获任何 DOM 事件。

如果在元素上发生单击事件时，你想做些事情：

```html
<template>
  <a>Clicke me!</a>
</template>
```

我们添加 `v-on` 指令：

```html
<template>
  <a v-on:click="handleClick">Clicke me!</a>
</template>
```

Vue 也提供了一种非常便捷的替代方法：

```html
<template>
  <a @click="handleClick">Clicke me!</a>
</template>
```

你可以选择是否使用圆括号。`@click="handleClick"` 相当于 `@click="handleClick()"`。

`handleClick` 是附加在组件上的方法。

```javascript
export default {
  methods: {
    handleClick: function(event) {
      console.log(event)
    }
  }
}
```

方法在 [Vue 方法教程](https://github.com/coderfe/100-days-of-translate/blob/master/vue-methods.md)里有详细解释。

在这里你需要知道的是你可以为事件传递参数：`@click="handleClick(param)"`，它们将在方法内部接收参数。

## 访问原生事件对象

许多情况下，你要对事件对象执行一些操作，或者查找一些它上面的属性。你如何访问它呢？

使用特殊的 `$event` 指令：

```html
<template>
  <a @click="handleClick($event)">Click me!</a>
</template>

<script>
export default {
  methods: {
    handleClick: function(event) {
      console.log(event)
    }
  }
}
</script>
```

如果你已经传递了一个参数：

```html
<template>
  <a @click="handleClick('something', $event)">Click me!</a>
</template>

<script>
export default {
  methods: {
    handleClick: function(text, event) {
      console.log(text)
      console.log(event)
    }
  }
}
</script>
```

在方法中你可以调用 `event.preventDefault()`，但是有种更好的方法：事件修饰符。

## 事件修饰符

不要在方法中弄乱 DOM 的事情，而是告诉 Vue 为你处理事情：

- `@click.prevent` 调用 `event.preventDefault()`

- `@click.stop` 调用 `event.stopPropagation()`

- `@click.passive` 使用 [addEventListener 的被动选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)

- `@click.capture` 使用事件捕获而非事件冒泡

- `@click.self` 确保单击事件不是从子事件冒泡的，而是直接发生在元素上的

- `@click.once` 事件将只触发一次

所有的这些修饰符都可以结合使用。

有关更多的广播、冒泡和捕获，阅读我的文章：[Javascript 事件规范](https://flaviocopes.com/javascript-events/)
