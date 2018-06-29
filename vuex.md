# Vue.js 状态管理：Vuex

> Vuex 是 Vue.js 的官方状态管理库。在这篇教程中我将介绍它的基础用法。

## Vuex 简介

Vuex 是 Vue.js 的官方状态管理库。

它的工作是在你的应用程序中跨组件共享数据。

Vue.js 组件中开箱即用的通讯方式有：

- **props**，从父组件向子组件传递状态
- **event**，从子组件更改父组件的状态
- 在没有父子关系的组件之间也仍然可以使用自定义事件（`$emit` 和 `$on`）

有时应用程序相比这些简单的选项变得更复杂。

这种情况下，一个好的选择是将状态集中到一个仓库中。这就是 Vuex 所要做的。

## 为什么要使用 Vuex

Vuex 不是唯一个使用在 Vue 中的状态管理的选项（你也可以使用 Redux），但是它主要的优势在于它是官方提供的，并且与 Vue.js 集成使其取胜。

在 React 中，你就必须在众多可用的类库中选择一个，因为 React 的生态庞大且没有事实上的标准。最近，Recux 是最受欢迎的选择，Mobx 在人气方面紧随其后。在 Vue 中，我甚至会说，除了 Vuex，你不需要寻找其它任何类库，尤其是在刚开始的时候。

Vuex 借鉴了 React 生态系统中的许多想法，因为这是由 Redux 推广的 Flux 模式。

如果你已经知道 Flux 或者 Redux，Vuex 与其非常相似。如果你不知道也没关系——我将会从头解释每个概念。

Vue 中的组件可以拥有它们自己的状态。例如，一个输入框会存储输入的数据。这很好，即使在使用 Vuex 时，组件也有自己的状态。

你知道当你开始做大量工作来传递一些状态，你就需要像 Vuex 这样的工具。

这种情况下，Vuex 为状态提供一个集中仓库，并且你可以通过仓库执行操作来更改状态。

依赖于某些特定状态的每个组件可以通过仓库中的 getter 来访问数据，这确保了一旦发生变化，它可以得到及时的更新。

使用 Vuex 会在程序中引入一些复杂性，由于需要以某种方式设置才能正常工作，但是如果这能帮助解决混乱的 props 传递和事件系统（如果程序过于复杂），其不失为一个好的选择。

## 开始

在本例中，我从一个 [Vue CLI](https://github.com/coderfe/100-days-of-translate/blob/master/vue-cli.md) 程序开始。Vuex 也可以直接通过加载 script 标签使用，但是 Vuex 更适用于较大的应用程序，你更有可能在更有结构化的程序中使用它，像这个例子，你可以使用 Vue CLI 快速开始。

我会把例子放在 CodeSandbox，这是一个优秀的服务，它提供了一个 Vue CLI 示例：[https://codesandbox.io/s/vue](https://codesandbox.io/s/vue)。我建议使用它。

![codesandbox](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vuex/1.png)

一旦你到了那里，单击 **Add dependency** 按钮，选择 Vuex 并单击。

现在，Vuex 会在项目依赖中。

在本地安装 Vuex，你可以项目文件夹下运行 `npm install vuex` 或者 `yarn add vuex`。

## 创建 Vuex 仓库

现在我们准备好创建 Vuex 仓库了。

这个文件可以放在任何地方。通常建议放在 `src/store/store.js` 文件中，所以我们也这样做。

在这个文件中我们初始化 Vuex，并告诉 Vue 来使用它：

```javascript
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export const store = new Vuex.Store({});
```

![storejs](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vuex/2.png)

我们导出了一个 Vuex 仓库对象，它是我们通过使用 `Vuex.Store()` API 创建的。

## 仓库的用例

现在我们有一个骨架，想一个比较好的 Vuex 用例，这样我就可以介绍它的概念。

例如，我有 2 个兄弟组件，一个组件有一个输入框，另一个用来输出输入框的内容。

当输入框发生变化时，我也希望改变第二个组件里的内容。非常简单，但这将为我们工作。

## 介绍所需新组件

我删除了 HelloWorld 组件，并且添加 Form 组件和 Display 组件。

```html
<template>
  <div>
    <label for="flavor">Favorite ice cream flavor?</label>
    <input name="flavor" >
  </div>
</template>
```

```html
<template>
  <p>You choose ???</p>
</template>
```

## 将组件添加到 App

```html
<template>
  <div id="app">
    <Form/>
    <Display/>
  </div>
</template>

<script>
import Form from './components/Form'
import Display from './components/Display'

export default {
  name: 'App',
  components: {
    Form,
    Display
  }
}
</script>
```

## 在仓库中添加状态

在这个地方，我们回到 store.js 文件并为 store 添加一个 state 属性，它是包含 `flavor` 属性的一个对象。它的初始状态是空字符串。

```javascript
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {
    flavor: ""
  }
});
```

当用户向输入框键入时我将更新它的内容。

## 添加 mutation

state 只能使用 mutations 进行操作。我们设置了一个 mutation 会用在 Form 组件内，它用来通知仓库，state 应该得到更新。

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export const store = new Vuex.Store({
  state: {
    flavor: ''
  },
  mutations: {
    change:(state, flavor) {
      state.flavor = falvor
    }
  }
})
```

## 添加 state 属性的 getter 引用

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export const store = new Vuex.Store({
  state: {
    flavor: ''
  },
  mutations: {
    change:(state, flavor) {
      state.flavor = falvor
    }
  },
  getters: {
    flavor: state => state.flavor
  }
})
```

注意 getters 是一个对象。`flavor` 是这个对象的属性，接收 state 作为参数，并且返回 state 的 `flavor` 属性。

## 把 Vuex 仓库添加到程序中

现在仓库已经可以使用了。我们回到应用程序代码，并且在 main.js 中导入我们需要的 state，使其在 Vue 程序中能够使用。

添加：

```javascript
import { store } from "./store/store";
```

然后添加到 Vue 程序中：

```javascript
new Vue({
  el: "#app",
  store,
  components: { App },
  template: "<App />"
});
```

因为这是 Vue 组件的主要部分，一旦我们添加了这些代码，每个 Vue 组件中的 `store` 变量都将指向 Vuex 仓库。

## 使用 commit 更新用户操作的状态

当用户键入内容时让我们更改那个 state。

我们通过 `store.commit()` API 来完成。

但是首先我们得创建一个当输入框内容更改时能够被调用的方法。我们使用 `@input` 代替 `@change`，因为后者只有当焦点移出输入框后才会触发，而 `@input` 则在每次按键后被调用。

```html
<template>
  <div>
    <label for="flavor">Favorite ice cream flavor?</label>
    <input name="flavor" @input="changed" />
  </div>
</template>

<script>
  export default {
    methods: {
      changed: function(event) {
        alert(event.target.value)
      }
    }
  }
</script>
```

现在我们有了 favor 的值了，我么可以使用 Vuex API：

```javascript
export default {
  methods: {
    changed: function(event) {
      this.$store.commit("change", event.target.value);
    }
  }
};
```

看看我们如何使用 `this.$store` 来引用 store？这主要是由于在 Vue 组件初始化中包含了 store 对象。

`commit()` 方法接收一个 mutation 名称（我们在 Vuex 中使用了 `change`）和一个载荷，它将作为其回调函数的第二个参数传递给 mutation。

## 使用 getter 输出 state 的值

现在我们需要在 Display 组件中使用 `$store.getter.flavor` 引用这个 getter 的值。由于实在模板中，`this` 可以省略，因为模板中 `this` 是隐性的。

```html
<template>
  <div>
    <p>You choose {{ $store.getter.flavor }}</p>
  </div>
</template>
```

## 总结

以上，是对 Vuex 的简介！

完整的源代码：[https://codesandbox.io/s/zq7k7nkzkm](https://codesandbox.io/s/zq7k7nkzkm)。

这里仍然有许多概念缺失：

- actions
- modules
- helpers
- plugins

但是你有了基础知识，就可以去阅读官方文档了。

Happy coding!
