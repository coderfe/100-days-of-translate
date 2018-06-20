# Vue.js 组件

组件是接口的单个独立单元。它们拥有自身的状态、标记和样式。

## 如何使用组件

Vue 组件可以用 4 种方式定义。

我们讨论以下代码。

第一种是：

```javascript
new Vue({ /* options */ })
```

第二种是：

```javascript
Vue.component('component-name', { /* options */ })
```

第三种是使用本地组件：组件只能在特定的组件中访问，并不适用于其它地方（非常适合封装）。

第四种是 `.vue` 文件，又称为[单文件组件](https://github.com/coderfe/100-days-of-translate/blob/master/vue-single-file-components.md)。

让我们深入了解一些前三种方式。
