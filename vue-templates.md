# Vue.js 模板和插值

> Vue.js 使用的模板语言是 HTML 的超集。任何 HTML 都是有效的 Vue.js 模板，此外，Vue.js 提供了两个强大的功能：插值和指令。

Vue.js 使用的模板语言是 HTML 的超集。

任何 HTML 都是有效的 Vue.js 模板，此外，Vue.js 提供了两个强大的功能：**插值**和**指令**。

我将在这篇文章中详细介绍插值，明天再为指令写篇文章。

这是一个有效的 Vue.js 模板。

```html
<span>Hello!</span>
```

这个模板可以放在已经明确声明的 Vue.js 组件中。

```javascript
new Vue({
  template: '<span>Hello!</span>'
})
```

或者放在[单文件组件](https://github.com/coderfe/100-days-of-translate/blob/master/vue-single-file-components.md)中：

```html
<template>
  <span>Hello!</span>
</template>
```

这是一个非常基础的例子。下一步是使其输出一个组件的状态，例如名称。

可以使用插值来完成。首先我们为组件添加一些数据。

```javascript
new Vue({
  data: {
    name: 'Flavio'
  },
  template: '<span>Hello!</span>'
})
```

然后我们可以使用双括号语法将其添加到模板中：

```javascript
new Vue({
  data: {
    name: 'Flavio'
  },
  template: '<span>Hello {{ name }}!</span>'
})
```

这里有一件有趣的事情。看看我们如何仅仅使用 `name` 代替 `this.data.name` ？

这是因为 Vue.js 做了一些内部绑定，并让模板使用这些属性。非常便捷。

在单文件组件中是这样的：

```html
<template>
  <span>Hello {{ name }}!</span>
</template>

<script>
export default {
  data() {
    return {
      name: 'Flavio'
    }
  }
}
</script>
```

我在导出中使用了常规函数。为什么不是箭头函数呢？

这是因为在 `data` 中我们可能需要访问组件实例中的方法，然而如果 `this` 没有绑定到组件，这无法实现，所以使用箭头函数是不可能的。

我们可以使用箭头函数，但是如果要使用 `this` 时记得要切换到常规函数。我认为最好安全一些。

现在，回到插值。

`{{ name }}` 是 Mustache / Handlebars 模板插值的提示，但它仅仅是视觉提示。

在这些模板引擎中，它们是”dumb”。在 Vue 中你可做到更多，而且更加灵活。

你可以在插值中使用 Javascript 表达式，但是仅限一个表达式：

```html
{{ name.reverse() }}
```

```html
{{ name === 'Flavio' ? 'Flavio' : 'stranger' }}
```

Vue 在插值中提供了一些全局对象，包含 Math 和 Date ，因此你可以像这样使用它们：

```html
{{ Math.sqrt(16) * Math.random() }}
```

最好避免向模板中添加复杂的逻辑，Vue 允许我们提供更多的灵活性，特别是在尝试解决问题时。

可以首先尝试在模板中添加表达式，然后稍后将其移动到计算属性或者方法中。

包含在插值中任何值都将在它所依赖的数据属性发生变化时得到更新。

你可以使用 `v-once` 指令避免这种反应。

结果总是会被转义，所以输出中不能有 HTML 。

如果你需要 HTML 片段，你需要使用 `v-html` 指令。
