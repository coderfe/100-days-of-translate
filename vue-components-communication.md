# Vue.js 组件通讯

> 如何使组件在 Vue.js 应用程序中通信？

Vue 组件可以使用多种方式通信。

## Props

第一种方式是使用 [props](https://github.com/coderfe/100-days-of-translate/blob/master/vue-props.md)：

通过组件声明时添加参数，父组件可以向下传递数据。

```html
<template>
  <div>
    <Car color="green" />
  </div>
</template>

<script>
import Car from './components/Car'

export default {
  name: 'App',
  components: {
    Car
  }
}
</script>
```

Props 是单向的：从父组件到子组件。父组件改变 props 时，更新后的值将会被传递到子组件并且重新渲染。

反之则不然，你不应该在子组件内部改变 prop。

## 父子组件通过事件通讯

事件允许你从子组件向父组件通讯：

```javascript
export default {
  name: 'Car',
  methods: {
    handleClick: function() {
      this.$emit('clickedSomething')
    }
  }
}
```

父组件在组件模板中可以通过使用 `v-on` 捕获该事件：

```html
<template>
  <div>
    <Car v-on:clickedSomething="handleClickInParent" />
    <!-- or -->
    <Car @clickedSomething="handleClickInParent" />
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    handleClickInParent: function() {
      //...
    }
  }
}
</script>
```

当然你也可以传递参数：

```javascript
export default {
  name: 'Car',
  methods: {
    handleClick: function() {
      this.$emit('clickedSomething', param1, param2)
    }
  }
}
```

自夫组件中接收：

```html
<template>
  <div>
    <Car v-on:clickedSomething="handleClickInParent" />
    <!-- or -->
    <Car @clickedSomething="handleClickInParent" />
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    handleClickInParent: function(param1, param2) {
      //...
    }
  }
}
</script>
```

## 使用 Event Bus 在任何组件间通讯

使用事件让你不再局限于父子关系。

你可以使用所谓的 Event Bus。

上面我们使用 `this.$emit` 在组件实例上触发一个事件。

我们要做的是在更普遍可访问的组件上触发事件。

`this.$root` 是根组件，通常用于此。

你也可以创建一个专门用来做次工作的 Vue 组件，并在需要时导入它。

```javascript
export default {
  name: 'Car',
  methods: {
    handleClick: function() {
      this.$root.$emit('clickedSomething')
    }
  }
}
```

任何其它组件都可以监听这个事件。你可以在 `mounted` 回调里做这些事情：

```javascript
export default {
  name: 'Car',
  mounted() {
    this.$root.$on('clickSomething', () => {
      //...
    })
  }
}
```

## 可选

这些是 Vue 提供的能够开箱即用的。

当你的程序太大无法适用这些方法时，你可以试试 Vuex 或者其它第三方库。



