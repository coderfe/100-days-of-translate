# Vue.js 方法

> Vue 方法是与 Vue 实例相关联的函数。方法定义在 `methods` 属性内。让我们看看它如何工作。

<!-- TOC -->

- [Vue.js 方法](#vuejs-方法)
  - [什么是 Vue.js 方法](#什么是-vuejs-方法)
  - [向 Vue.js 传参](#向-vuejs-传参)
  - [在方法中如何访问 data](#在方法中如何访问-data)
  - [访问原始事件对象](#访问原始事件对象)
  - [事件修饰符](#事件修饰符)

<!-- /TOC -->

## 什么是 Vue.js 方法

Vue 方法是与 Vue 实例相关联的函数。

方法定义在 `methods` 属性内：

```javascript
new Vue({
  methods: {
    handleClick: function() {
      alert('test')
    }
  }
})
```
或者是在单文件组件中：

```javascript
export default {
  methods: {
    handleClick: function() {
      alert('test')
    }
  }
}
```

方法非常有用，特别是对于你要处理一些操作，并且在元素上添加 `v-on` 时。就像这个，当点击元素时 `handleClick` 会被调用：

```html
<template>
  <a @click="handleClick">Click me!</a>
</template>
```

把方法指定为事件处理器时，你不需要使用圆括号，但这样做也无妨：

```html
<template>
  <a @click="handleClick()">Click me!</a>
</template>
```

## 向 Vue.js 传参

方法可以接受参数。

在这种情况下，你只需在模板中传参：

```html
<template>
  <a @click="handleClick('something')">Click me!</a>
</template>
```

```javascript
new Vue({
  methods: {
    handleClick: function(text) {
      alert(text)
    }
  }
})
```

或者在单文件组件中：

```html
<template>
  <a @click="handleClick('something')">Click me!</a>
</template>

<script>
export default {
  methods: {
    handleClick: function(text) {
      alert(text)
    }
  }
}
</script>
```

## 在方法中如何访问 data

你可以通过使用 `this.propertyName` 访问任何 Vue 组件的 data 属性：

```html
<template>
  <a @click="handleClick()">Click me!</a>
</template>

<script>
export default {
  data() {
    return {
      name: 'Flavio'
    }
  },
  methods: {
    handleClick: function() {
      console.log(this.name)
    }
  }
}
</script>
```

我们不需要使用 `this.data.name` ，仅使用 `this.name` 。Vue 为我们提供了透明的绑定。使用 `this.data.name` 将引发错误。

## 访问原始事件对象

在许多情况下，你需要在事件对象中执行一些操作。你如何访问它呢？

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

或者你已经传递了一个参数：

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

在那里你可以调用 `event.preventDefault()` ，但是有种更好的解决方案：事件修饰符。

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
