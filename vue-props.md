# Vue.js 组件 Props

> Props 用于将状态传递给子组件。了解它的一切。

## 在组件中定义 prop

组件期望有一个或多个 prop，它必须定义在组件的 props 属性中：

```javascript
Vue.component('user-name', {
  props: ['name'],
  template: '<p>Hi {{ name }}</p>'
})
```

或者在单文件组件中：

```html
<template>
  <p>{{ name }}</p>
</template>

<script>
export default {
  props: ['name]
}
</script>
```

## 接收多个 props

你可以接收多个 props，通过将它们追加到数组中：

```javascript
Vue.component('user-name', {
  props: ['firstName', 'lastName'],
  template: '<p>Hi {{ firstName }} {{ lastName }}</p>'
})
```

## 设置 prop 类型

通过使用对象代替数组，你可以非常简单地指定 prop 的类型，使用每个属性的名称作为 key，类型作为 value：

```javascript
Vue.component('user-name', {
  props: {
    firstName: String,
    lastName: String
  },
  template: '<p>Hi {{ firstName }} {{ lastName }}</p>'
})
```

你可以使用的有效的类型：

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

当类型不匹配时，Vue 会控制台发出警告（开发模式下）。

Prop 类型可以更加明确。

你可以允许多种不同的类型：

```javascript
props: {
  firstName: [String, Number]
}
```

## 将 prop 设置为必选项

```javascript
props: {
  firstName: {
    type: String,
    required: true
  }
}
```

## 设置 prop 默认值

你可以明确指定默认值：

```javascript
props: {
  firstName: {
    type: String,
    default: 'Unknown person'
  }
}
```

对于 Object：

```javascript
props: {
  firstName: {
    type: Object,
    default: {
      firstName: 'Unkonw',
      lastName: ''
    }
  }
}
```

`default` 也可以是返回一个适当值的函数，而不是实际值。

你甚至可以构建一个自定义验证器，对复杂数据来说非常酷：

```javascript
props: {
  name: {
    validator: (name) => {
      return name === 'Flavio'  // 只允许 Flavio
    }
  }
}
```

## 向组件传递 props

向组件传递 prop，可以使用以下语法：

```html
<ComponentName color="white" />
```

如果你传递的值是静态值。

如果它是 data 的属性，你可以使用：

```html
<template>
  <ComponentName :color="color" />
</template>

<script>
...
export default {
  //...
  data: function() {
    return {
      color: 'white'
    }
  },
  //...
}
</script>
```

你可以在 prop 值内部使用三元表达式检查真值条件，并且传递一个依赖于它的值：

```html
<template>
  <ComponentName :colored="color === 'white' ? true : false" />
</template>

<script>
...
export default {
  //...
  data: function() {
    return {
      color: 'white'
    }
  },
  //...
}
</script>
```
