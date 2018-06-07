# Vue 单文件组件

> 学习 Vue 如何帮你创建单个文件，这个文件负责处理与单个组件有关的所有内容，主要负责外观和行为。

Vue 组件可以像这样声明在 Javascript 文件（`.js`）里：

```javascript
Vue.component('component-name', {
  // options
})
```

也可以这样：

```javascript
new Vue({
  // options
})
```

或者也可以在指定的 `.vue` 文件中

`.vue` 文件非常酷，因为它允许你把这些都定义在单个文件：

- Javascript 逻辑
- HTML 代码模板
- CSS 样式

正因为如此，才称之为**单文件组件**。

举个例子：

```vue
<template>
  <p>{{ hello }}</p>
</template>

<script>
export default {
  data() {
    return {
      hello: 'Hello World'
    }
  }
}
</script>

<style scoped>
p {
  color: blue;
}
</style>
```

[webpack](https://flaviocopes.com/webpack/) 使这一切成为可能。 [Vue CLI](https://flaviocopes.com/vue-cli/) 使得这些非常容易，并且支持开箱即用。没有 webpack 设置，就不能使用 `.vue` 文件，因此，它们并不适用于仅在页面上使用 Vue 的程序，而不是完整的单页应用程序（SPA）。

由于单文件组件依赖 webpack ，我们可以放心使用现代的 Web 功能。

你的 CSS 可以用 SCSS 或者 Stylus 定义，模板可以用 Pug 构建，要实现这些，你要做的就是在 Vue 中声明你将要使用哪种预处理器语言。

这里是支持的预处理器：

- TypeScript
- SCSS
- Sass
- Less
- PostCSS
- Pug

我们可以使用现代 Javascript（ES6-7-8）而不用考虑目标浏览器，使用 Babel 集成及 ES 模块，因此我们也可以使用 `import/export` 语句。

我们可以使用 CSS Modules 来限定 CSS 的作用域。

谈到 CSS 作用域，单文件组件通过使用 `<style scoped>` 标签使其变得非常容易，而且也不会污染其它组件。

如果你省略 `scoped` ，你定义的 CSS 将会是全局的。但是加上 `scoped` ，Vue 会自动为组件添加一个特定的 class ，在程序中是唯一的，因此可以确保 CSS 不会污染其它组件。

也许你的 Javascript 文件很大，因为有很多逻辑需要处理。如果你想针对你的 Javascript 使用单独的文件，怎么办？

你可以使用 `src` 属性使其外部化：

```vue
<template>
  <p>{{ hello }}</p>
</template>
<script src="./hello.js"></script>
```

这也适用于 CSS ：

```vue
<template>
  <p>{{ hello }}</p>
</template>
<script src="./hello.js"></script>
<style src="./hello.css"></style>
```

注意我是怎么在组件的 Javascript 中设置数据的：

```javascript
export default {
  data() {
    return {
      hello: 'Hello World!'
    }
  }
}
```

其他常见的方式是：

```javascript
export default {
  data: function() {
    return {
      name: 'Flavio'
    }
  }
}
```

或者：

```javascript
export default {
  data: () => {
    return {
      name: 'Flavio'
    }
  }
}
```

不同之处在于它使用了箭头函数。在我们访问组件的方法时，箭头函数时可用的，但是因为我们需要用到 `this` ，并且这个属性没有绑定到使用箭头函数的组件。所以必须使用常规函数而不是箭头函数。

你可能也会看到：

```javascript
module.exports = {
  data: () => {
    return {
      name: 'Flavio'
    }
  }
}
```

这个使用了 CommonJS 语法，并且可以适用，尽管推荐使用 ES 模块语法，因为它是 Javascript 标准。
