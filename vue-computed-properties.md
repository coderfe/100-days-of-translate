# Vue.js 计算属性

> 学习如何使用 Vue 计算属性缓存计算值。

<!-- TOC -->

- [Vue.js 计算属性](#vuejs-计算属性)
  - [什么是 Vue.js 计算属性](#什么是-vuejs-计算属性)
  - [计算属性的一个例子](#计算属性的一个例子)
  - [计算属性 vs 方法](#计算属性-vs-方法)

<!-- /TOC -->

## 什么是 Vue.js 计算属性

在 Vue.js 中你可以使用大括号输出任何 data 中的值：

```html
<template>
  <p>{{ count }}</p>
</template>

<script>
export default {
  data() {
    return {
      count: 1
    }
  }
}
</script>
```

这个属性可以进行一些小运算，例如：

```html
<template>
  {{ count * 10 }}
</template>
```

但是仅限于单个 [Javascript 表达式](https://flaviocopes.com/javascript-expressions/)。

除了这个技术限制之外，你还需考虑模板应该只关心向用户展示数据，而不是处理逻辑运算。

想用单个表达式做更多事情，你就需要声明更多的模板，所以你需要使用**计算属性**。

计算属性可以定义在 Vue 组件中的 `computed` 属性中：

```javascript
export default {
  computed: {
    
  }
}
```

## 计算属性的一个例子

这里有示例代码，它使用一个计算属性 `count` 来计算输出。注意：

1. 我不必调用 `{{ count() }}` 。Vue.js 会自动调用这个函数。
2. 我使用常规函数（而非箭头函数）定义 `count` 计算属性，因为我需要通过 `this` 访问当前组件实例。

```html
<template>
  <p>{{ count }}</p>
</template>

<script>
export default {
  data() {
    return {
      items: [1, 2, 3]
    }
  },
  computed: {
    count: function() {
      return 'The count is ' + this.items.length * 10
    }
  }
}
</script>
```

## 计算属性 vs 方法

如果你已经了解 [Vue Methods](https://github.com/coderfe/100-days-of-translate/blob/master/vue-methods.md) ，你可能想知道有何不同。

首先，方法必须被调用，不只是引用，因此你需要：

```html
<template>
  <p>{{ count() }}</p>
</template>

<script>
export default {
  data() {
    return {
      items: [1, 2, 3]
    }
  },
  methods: {
    count: function() {
      return 'The count is ' + this.items.length * 10
    }
  }
}
</script>
```

但是最主要的不同在于：**计算属性会被缓存**。

`count` 计算属性的结果会进行缓存，直到 `items` 属性发生变化。

重要：计算属性只有在其依赖源更新时才会得到更新。常规的 Javascript 不是响应式的，一个常见的例子是 `Date.now()` ：

```html
<template>
  <p>{{ now }}</p>
</template>

<script>
export default {
  computed: {
    now: function() {
      return Date.now()
    }
  }
}
</script>
```

它只渲染一次，然后即使组件重新渲染时也不会更新。仅在页面刷新，Vue 组件退出并重新初始化时才会更新。

在这种情况下，方法更适合你的需求。
