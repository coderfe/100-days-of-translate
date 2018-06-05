# Vue.js 开发工具

Vue 有一个优秀的面板可以集成到浏览器开发者工具中，可以检查你的应用程序并与之交互，更加方便地调试和理解。

当你第一次尝试 Vue 时，如果你打开了浏览器开发者工具，你可以看到这个信息：“下载 Vue Devtools 扩展以获得更好的开发体验：[https://github.com/vuejs/vue-devtools](https://github.com/vuejs/vue-devtools)”：

![vue-devtools](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/1.png)

这是安装 Vue Devtools 扩展的友好的提示。那是什么？任何流行的框架都有自己的开发工具扩展，通常它们会为浏览器开发者工具添加一个新的面板，比浏览器提供的默认面板更加专业。在这种情况下，这个面板会让我们检查 Vue 应用程序并与之交互。

当构建 Vue 应用时，这个工具将会有令人惊喜的帮助。开发者工具只能在开发模式下检查 Vue 应用。这也确保了没有人可以在生产环境下检查你的程序（这可以使 Vue 性能更加优秀，因为它不必关心开发者工具）。

让我们安装它！

有 3 种方式安装 Vue 开发者工具：

- on Chrome
- on Firefox
- as a standalone application

Safari、Edge 和其他浏览器不支持自定义扩展，但是通过使用独立开发工具，你可以在任何浏览器调试 Vue.js 程序。

## 在 Chrome 上安装

前往 Google Chrome 商店：[https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) ，并且点击**添加到 Chrome** 。

完成安装过程。Vue 开发者工具的 Icon 显示在工具栏上。如果页面上没有 Vue.js 实例运行的话，它将显示为灰色。如果检测到 Vue.js ，图标将是 Vue logo 的颜色。

图标除了向我们展示是否存在 Vue.js 实例之外，它什么也不做。要使用 Vue 开发者工具的话，我们必须打开浏览器开发者工具，通过“View → Developer → Developer Tools”，或者 `Cmd-Alt-i` 。

![Cmd-Alt-i](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/2.png)

## 在 Firefox 上安装

你可以在 Mozilla 插件商店中找到开发者工具：[https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)

## 安装独立应用程序

或者你可以使用开发者工具的独立版本。

只需如下安装：

```shell
npm install -g @vue/devtools

//or

yarn global add @vue/devtools
```

运行它通过：

```shell
vue-devtools
```

这回打开一个基于 Electron 的应用程序。

![Electron](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/3.png)

现在，粘贴 script 标签：

```html
<script src="http://localhost:8098"></script>
```

等待应用程序重新加载，并且它会自动连接。

## 如何使用开发者工具

正如前面提到的，Vue 开发者工具需要通过打开浏览器开发者工具并移动到 Vue 面板才能激活。

另一个方式是在页面的任何元素上右键，选择“Inspect Vue component” 。

Vue 开发者工具面板打开后，我们可以导航到组件树。当我们在左侧列表选择一个组件，右边的面板将会显示 props 和 data :

![props-data](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/4.png)

顶部有 4 个按钮：

- Components（当前面板），这是运行在当前页面的所有组件实例。Vue 可以同时运行多个实例，例如，它可能会使用单独的轻量级应用程序来管理购物车小部件和幻灯片显示。

- Vuex 通过 Vuex 检查状态管理。

- Events 显示所有提交的事件。

- Refresh 刷新开发者工具面板。

注意到组件上的小字 `= $vm0` 了吗？这是使用控制台检查组件的便捷方式。按下 `Esc` 可以在开发工具底部显示控制台，你可以键入 `$vm0` 来访问 Vue 组件。

![$vm0](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/5.png)

这非常酷，同来检查并且与组件交互，而不用将它们分配给代码中的全局变量。

### 管理数据

你可以直接编辑组件的 state 通过修改 data

### 筛选组件

通过键入组件名称，然后组件树会过滤掉不匹配的。

## 在页面中选择组件

点击
![select-component](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue.js-devtools/6.png)
按钮，然后会在开发工具中打开。

### 格式化组件名称

你可以选在用 camelCase 或者 dashes 显示组件名称。

### 过滤检查数据

右边的面板，你可以键入任何单词来过滤不匹配的属性。

### 检查 DOM

单击检查 DOM 的按钮会将你带到开发者工具元素的检查器，使用组件生成的 DOM 元素。

### 在编辑器中打开

任何用户组件（不是框架层组件）都有一个按钮，可以在默认的编辑器中打开。非常便捷。
