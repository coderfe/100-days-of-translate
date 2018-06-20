# Vue.js 侦听属性

> Vue 侦听属性允许你监听组件数据，并在特定属性变化时运行。

侦听属性是 Vue.js 的一个特殊功能，它允许你监听组件数据的一个状态，并且在属性值发生变化时调用一个函数。

这儿有一个例子。我们有一个组件用来显示名称，并且允许你通过点击按钮改变它：

```html
<template>
  <p>My name is {{name}}</p>
  <button @click="changeName()">Change my name!</button>
</template>

<script>
export default {
  data() {
    return {
      name: 'Flavio'
    }
  },
  methods: {
    changeName: function() {
      this.name = 'Flavius'
    }
  }
}
</script>
```

当名称发生变化时，我们可能需要做些事情，例如打印输出。

我们可以通过向 `watch` 对象上添加一个属性，这个属性是我们想要监听的 data 属性：

```html
<script>
export default {
  data() {
    return {
      name: 'Flavio'
    }
  },
  methods: {
    changeName: function() {
      this.name = 'Flavius'
    }
  },
  watch: {
    name: function() {
      console.log(this.name)
    }
  }
}
</script>
```

`watch.name` 属性的函数可接受两个可选参数。第一个是新的属性值，第二个是旧属性值。

```javascript
export default {
  /* ... */
  watch: {
    name: function(newValue, oldValue) {
      console.log(newValue, oldValue)
    }
  }
}
```
