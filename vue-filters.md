# Vue.js 过滤器

> Vue.js 过滤器允许我们对模板插值中的某个值进行格式化或者转化。

过滤器是 Vue 组件提供的实用功能，它允许你对任何动态模板的部分数据进行格式化或者转化。

它们不会改变组件数据，但是它们仅影响输出值。

假设你要输出一个名称：

```html
<template>
  <p>Hi {{ name }}!</p>
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

如果你想检查 `name` 实际包含一个值，如果不打印 'there' ，那么我们的模板会打印 "Hi there?" 吗？

输入过滤器：

```html
<template>
  <p>Hi {{ name | fallback }}!</p>
</template>

<script>
export default {
  data() {
    return {
      name: 'Flavio'
    }
  },
  filters: {
    fallback: function(name) {
      return name ? name : 'there'
    }
  }
}
</script>
```

注意应用过滤器的语法时是 `| filterName` 。如果你熟悉 Unix ，它相当于 Unix 管道操作符，用于将操作后的输出传递给下一个操作。

组件的 `filters` 属性是一个对象。单个过滤器是一个函数，接受一个值，返回另一个值。

返回值实际上是输出在 Vue.js 模板中的值。

过滤器可以访问组件的 data 和方法。

好的过滤器用例是什么？

- 转换字符串，例如大写或小写
- 格式化价格

你在上面看到的简单的过滤器的例子：`{{ name | fallback }}` 。

通过重复管道语法可以连接过滤器：

```html
{{ name | fallback | capitalize }}
```

第一个应用 `fallback` 过滤器，然后是 `capitalize` 过滤器（我们没有定义，试着做一个）。

高级过滤器也可以接受参数，使用常规的函数参数语法：

```html
<template>
  <p>Hello {{ name | prepend('Dr.') }}</p>
</template>

<script>
export default {
  data() {
    return {
      name: 'House'
    }
  },
  filters: {
    prepend: (name, prefix) => {
      return `${prefix} ${name}`
    }
  }
}
</script>
```

如果你向过滤器传参，第一个传递给过滤器函数的参数总是模板插值中的项目（在这个例子中是 `name`），后面是你显式传递的参数。

你可以使用逗号传递多个参数。

注意：我用的是箭头函数。通常在方法和计算属性中避免使用箭头函数，因为它们几乎总是引用 `this` 访问组件数据，但是在这种情况下，过滤器不需要访问 this ，而是通过参数接受它所需要全部数据，我们可以安全地使用更简单的箭头函数。

[这个库](https://www.npmjs.com/package/vue2-filters)有很多可以直接使用在模板中的预置过滤器，包括 `capitalize`, `uppercase`, `lowercase`, `placeholder`, `truncate`, `currency`, `pluralize` 等等。
