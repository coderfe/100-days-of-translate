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

第三种是使用局部组件：组件只能在特定的组件中访问，并不适用于其它地方（非常适合封装）。

第四种是 `.vue` 文件，又称为[单文件组件](https://github.com/coderfe/100-days-of-translate/blob/master/vue-single-file-components.md)。

让我们深入了解一下前三种方式。

当你构建的应用程序不是单页应用（SPA），而只是在一些例如联系人表单或者购物车的页面使用 Vue.js 时，`new Vue()` 和 `Vue.component()` 是使用 Vue 的标准方式。或者在所有页面中使用 Vue ，但是由服务端渲染布局，并且由你将 HTML 呈现给客户端，然后客户端加载你构建的 Vue 应用程序。

在单页应用中，最常见的时单文件组件，因为它们足够方便。

通过将 Vue 挂载到 DOM 元素上来实例化它。如果你有 `<div id="app"></div>` 这样的标记，你可以这样做：

```javascript
new Vue({ el: '#app' })
```

使用 `new Vue()` 初始化的组件没有相应的标签名，因此它通常是主容器组件。

应用程序中使用的其它组件是由 `Vue.component()` 初始化的。这种组件允许你定义一个标签，你可以在程序中多次使用组件，并且可以在 `template` 属性中指定组件的输出。

```html
<div id="app">
  <user-name name="Flavio"></user-name>
</div>
```

```javascript
Vue.component({
  props: ['name'],
  template: '<p>Hi {{ name }}</p>'
})

new Vue({
  el: '#app'
})
```

[Edit in JSFiddle](https://jsfiddle.net/flaviocopes/nvgedhq4/?utm_source=website&utm_medium=embed&utm_campaign=nvgedhq4)

我们做了什么？我们在 `#app` 元素中初始化了一个 Vue 根组件，并且在它内部我们使用了 `user-name` 组件，用于抽象我们向用户致意的逻辑。

组件接收一个用于向子组件传递参数的属性。

在 `Vue.component()` 调用中，我们传递 `user-name` 作为第一个参数。这是给组件的一个名称。你可以用两种方式书写名称。第一种是我们使用过的，称为**短横线命名**。第二种是**大驼峰命名**，类似小驼峰命名，但是第一个字母是大写。

```javascript
Vue.component('UserName', { /* ... */ })
```

Vue 内部会自动创建从 `user-name` 到 `UserName` 的别名，反之亦然，所以你可以随意使用。在 Javascript 中通常使用 `UserName` ，模板中使用 `user-name` 。

## 局部组件

任何使用 `Vue.component()` 创建的组件都是**全局组件**。你不需要将其赋值给某个变量，或者传递到模板以复用组件。

通过将定义组件的对象分配给一个对象，可以封装本地组件。

```javascript
const Sidebar = {
  template: '<aside>Sidebar</aside>'
}
```

通过使用 `components` 属性使其在其它组件中可重用：

```javascript
new Vue({
  el: '#app',
  components: {
    Sidebar
  }
})
```

你可以将组件写在同一个文件中，但更好的方式是使用 Javascript 模块。

```javascript
import Sidebar from './Sidebar'

export default {
  el: '#app',
  components: {
    Sidebar
  }
}
```

## 重用组件

子组件可以添加多次。每个单独的实例都独立于其它实例。

```html
<div id="app">
  <user-name name="Flavio"></user-name>
  <user-name name="Roger"></user-name>
  <user-name name="Syd"></user-name>
</div>
```

```javascript
Vue.component('user-name', {
  props: ['name'],
  template: '<p>Hi {{ name }}</p>'
})

new Vue({
  el: "#app"
})
```

[Edit in JSFiddle](https://jsfiddle.net/flaviocopes/3kebv908/?utm_source=website&utm_medium=embed&utm_campaign=3kebv908)

## 组件的构建块

目前为止，我们看到组件如何接受 `el` 、`props` 及 `template` 等属性。

- `el` 仅用于使用 `new Vue({})` 初始化的跟组件中，并且定义了组件将要挂载的 DOM 元素。
- `props` 我们可以向子组件传递的所有属性
- `template` 我们可以设置组件模板的地方，它负责定义组件的输出

组件还可以接受的其它属性：

- `data` 组件局部状态
- `methods` ：组件的[方法](https://github.com/coderfe/100-days-of-translate/blob/master/vue-methods.md)
- `computed` ：与组件的[计算属性](https://github.com/coderfe/100-days-of-translate/blob/master/vue-computed-properties.md)相关 
- `watch` ：组件[侦听器](https://github.com/coderfe/100-days-of-translate/blob/master/vue-watchers.md)
