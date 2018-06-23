# Vue.js 插槽

> Slots 可以帮助你在组件中放置内容，并且允许其父组件整理内容。

组件可以选择定义自己的全部内容，例如这种情况：

```javascript
Vue.component('user-name', {
  props: ['name'],
  template: '<p>Hi {{ name }}</p>'
})
```

或者它也可以允许父组件通过**插槽**向其插入各种类型的内容。

什么是插槽？

你可以通过在组件模板中放置 `<slot></slot>` 来定义它：

```javascript
Vue.component('user-information', {
  template: '<div class="user-information"><slot></slot></div>'
})
```

使用此组件时，任何添加在开始标签和结束标签之间的内容都将会被添加到插槽占位符中。

```html
<user-information>
  <h2>Hi!</h2>
  <user-name name="Flavio"></user-name>
</user-information>
```

如果你将任何内容放在 `<slot></slot>` 标记中，它们将作为默认内容，以防没有内容传入。

复杂的组件布局可能需要更好的方式来组织内容。

**具名插槽**

使用具名插槽，你可以把部分插槽分配到组件模板布局中的指定位置，并且你可以在任何标签上使用 `slot` 属性，把内容分配到对应的插槽。

任何 template 标记之外的任何东西都会被添加到主 `slot` 中。

方便起见，我在例子中使用了 `page` 单文件组件：

```html
<template>
  <div>
    <main>
      <slot></slot>
    </main>
    <sidebar>
      <slot name="sidebar"></slot>
    </sidebar>
  </div>
</template>
```

```html
<page>
  <ul slot="sidebar">
    <li>Home</li>
    <li>Contact</li>
  </ul>

  <h2>Page title</h2>
  <p>Page content</p>
</page>
```
