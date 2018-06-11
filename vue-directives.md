# Vue.js 指令

> Vue.js 使用的模板语言是 HTML 超集。我么可以使用插值和指令。这篇文章介绍指令。

<!-- TOC -->

- [Vue.js 指令](#vuejs-指令)
  - [`v-text`](#v-text)
  - [`v-once`](#v-once)
  - [`v-html`](#v-html)
  - [`v-bind`](#v-bind)
  - [双向绑定：`v-model`](#双向绑定v-model)
  - [使用表达式](#使用表达式)
  - [条件语句](#条件语句)
  - [循环](#循环)
  - [事件](#事件)
  - [显示或隐藏](#显示或隐藏)
  - [事件指令修饰符](#事件指令修饰符)
  - [自定义指令](#自定义指令)

<!-- /TOC -->

## `v-text`

你可以使用 `v-text`指令，而不是插值，它执行相同的工作：

```html
<span v-text="name"></span>
```

## `v-once`

你知道 `{{ name }}` 如何绑定到组件 data 的 `name` 属性。

每当组件 data 中的 `name` 发生变化时，Vue 将会更新浏览器中表示的值。

除非你使用 `v-once` 指令，它看起来像 HTML 属性：

```html
<span v-once="name"></span>
```

## `v-html`

当你使用插值输出一个 data 属性时，HTML 会被转义。这是 Vue 用来防范 XSS 攻击的一个非常好的方法。

但是在某些情况下，你需要输出 HTML 并让浏览器解析。你可以使用 `v-html` 指令：

```html
<span v-html="someHtml"></span>
```

## `v-bind`

插值仅在标签内容中起作用。无法在属性上使用。

属性必须使用 `v-bind` ：

```html
<a v-bind:href="url">{{ linkText }}</a>
```

`v-bind` 非常常见，有一种速记语法：

```html
<a v-bind:href="url">{{ linkText }}</a>
<a :href="url">{{ linkText }}</a>
```

## 双向绑定：`v-model`

`v-model` 允许我们绑定一个表单元素，当用户改变元素内容时 Vue data 属性也随之改变。

```html
<input v-mode="messgae" placeholder="Enter a message">
<p>Message is: {{ message }}</p>
```

```html
<select v-model="selected">
  <option disabled value="">Choose a fruit</option>
  <option>Apple</option>
  <option>Banana</option>
  <option>Strawberry</option>
</select>
<span>Fruit chosen: {{ selected }}</span>
```

## 使用表达式

你可以在指令中使用 Javascript 表达式：

```html
<span v-text="'Hi, ' + name + '!'"></span>

<a v-bind:href="'https://' + domain + path">{{ linkText }}</a>
```

指令中使用的任何变量都会指向相应的 data 属性。

## 条件语句

在指令中你可以使用三元表达式执行条件检查，因为它是一个表达式：

```html
<span v-text="name == Flavio ? 'Hi Flavio!' : 'Hi' + name + '!'"></span>
```

有专门的指令允许你处理更多有条理的条件表达式：`v-if`、`v-else-if` 及 `v-else` 。

```html
<p v-if="shouldShowThis">Hey!</p>
```

`shouldShowThis` 是一个包含在组件 data 中的一个布尔值。

## 循环

`v-for` 允许你渲染一个列表。与 `v-bind` 结合可用来设置每个列表项目的属性。

你可以迭代一个简单数组的值：

```html
<template>
  <ul>
    <li v-for="item in items">{{ item }}</li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      items: ['car', 'bike', 'dog']
    }
  }
}
</script>
```

或者一个对象数组：

```html
<template>
  <div>
    <!-- using interpolation -->
    <ul>
      <li v-for="todo in todos">{{ todo.title }}</li>
    </ul>
    <!-- using v-text -->
    <ul>
      <li v-for="todo in todos" v-text="todo.title"></li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [
        { id: 1, title: 'Do something' },
        { id: 2, title: 'Do something else' }
      ]
    }
  }
}
</script>
```

`v-for` 可以为你提供索引：

```html
<li v-for="(todo, index) in todos"></li>
```

## 事件

`v-on` 允许你监听 DOM 事件，并且当事件发生时触发一个方法。这里我们监听一个单击事件：

```html
<template>
  <a v-on:click="handleClick">Click me!</a>
</template>

<script>
export default {
  methods: {
    handleClick: function() {
      alert('test')
    }
  }
}
</script>
```

你可以在任何事件中传参：

```html
<template>
  <a v-on:click="handleClick('test')">Click me!</a>
</template>

<script>
export default {
  methods: {
    handleClick: function(value) {
      alert(value)
    }
  }
}
</script>
```

而且也可以直接将部分 Javascript （单个表达式）直接放在模板中。

```html
<template>
  <a v-on:click="counter = counter + 1">{{counter}}</a>
</template>

<script>
export default {
  data: function() {
    return {
      counter: 0
    }
  }
}
</script>
```

`click` 只是一种事件。一种更常见的事件时 `submit` ，你可以使用 `v-on:submit` 绑定。

`v-on` 非常常见，它有一种速记语法，`@` ：

```html
<a v-on:click="handleClick">Click me!</a>
<a @:click="handleClick">Click me!</a>
```

## 显示或隐藏

你可以通过使用 `v-show` 选择在 DOM 显示一个元素，如果一个特定的属性在 Vue 实例中为 true 时：

```html
<p v-show="isTrue">Something</p>
```

这个元素依旧会被插入到 DOM 中，但是如果条件不满足会被设置为 `display: none` 。

## 事件指令修饰符

Vue 提供了一些可选的事件修饰符，你可以和 `v-on` 结合使用，它可以使事件自动执行某些事情，而不用在你的事件处理程序中编写代码。

一个比较好的例子是 `.prevent` ，它会在事件中自动调用 `preventDefault()` 。

这种情况下，表单不会导致页面重新加载，这是默认行为：

```html
<form v-on:submit.prevent="formSubmitted"></form>
```

其它修饰符包括：`.stop`, `.capture`, `.self`, `.once`, `passive` ，在[官方文章中有其详细描述](https://vuejs.org/v2/guide/events.html#Event-Modifiers)。

## 自定义指令

Vue 默认的指令已经可以让你做很多事情了，但是如果你需要，你总是可以添加新的指令。

如果你想学习更多，阅读 [https://vuejs.org/v2/guide/custom-directive.html](https://vuejs.org/v2/guide/custom-directive.html) 。
