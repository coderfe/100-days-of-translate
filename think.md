# 如何像程序员一样思考

> 我学不会 JavaScript。我无法从头开始制作组件。当我面对空白的 JavaScript 文件时我的大脑是一片空白。我猜我做不到，因为我不会像一个程序员那样思考。

听着很熟悉吧？我的朋友，你不是一个人这样想。许多把 JavaScript 作为自己的第一门编程语言的人面临着同样的问题。

即使是使用其它编程语言的人也对 JavaScript 面临着同样的问题。他们说“我无法用 JavaScript 思考”，而不是“我不会像程序员一样思考”。

但是不再如此，让今天成为你学习如何像程序员一样思考的日子。

## 你已经可以像程序员一样思考

你是否在 FreeCodeCamp、Code Academy 或者 Code Wars 这些地方尝试过一些关于 JavaScript 的基本练习吗？

如果你做过，你已经知道如何像程序员一样思考了。

当你面对 JavaScript 文件时脑袋空白的可能原因是 coder‘s blank。你害怕编写的 JavaScript 文件无法工作。你害怕面对错误。你不知道该如何开始。

克服 coder's block 很简单，可以遵循以下四个步骤：

1.  把问题分解为更小的问题
2.  为每个小问题找到解决方法
3.  以连贯的方式组合解决方放
4.  重构并改善

## 第一步：把问题分解为更小的问题

如何把一只大象放到冰箱里？

这可能是大多数人的答案：

1.  打开冰箱
2.  把大象放进去
3.  关上冰箱

问题解决了。

![ele-in-frdige](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/think/1.jpg)

> 可怜的大象。在冰箱里看起来很难过 :(

当你面对空白的 JavaScript 文件时遇到困难时，这个答案是最好的例子。它跳过了所有步骤。

如果你思考一下这个问题的逻辑，你会看见一些明显的问题还是没有答案：

1.  我们讨论的是什么冰箱？
2.  我们讨论的是哪种大象？
3.  如果大象对于冰箱来说太大了，你会怎么做？
4.  你第一次发现大象的地方在哪里？
5.  你如何把大象运输到你的冰箱里？

在你写代码时，你要回答你能考虑到的每个小问题。这就是为什么第一步是将问题分解为更小的问题的原因。

## 第二步：为每个小问题找到解决方法

第二步就是为每个小问题找到解决方法。这一步的关键在于尽可能的详细。

1.  什么冰箱？——在厨房的冰箱
2.  哪种大象？——[非洲丛林大象](https://en.wikipedia.org/wiki/African_elephant)
3.  如果大象太大了怎么办？——拿一把收缩枪（🔫）缩小大象（🐘）
4.  在哪里找到大象？——非洲
5.  如何运输大象？——将缩小之后的大象装在包里，然后坐飞机回家

有时候，你需要深挖几层才能得到你需要的答案。在上面的例子中，我们可以深入研究一下答案 3 和 4。

1.  在哪里拿到收缩枪？——从隔壁疯狂的科学家借用
2.  在非洲的哪里你可以找到大象？——南非的 Addo 大象公园

一旦每个更小的问题都有了答案，你就可以将它们拼装起来解决大问题了。

## 第三步：以连贯的方式组合解决方法

所以，在把大象放进冰箱的例子中，你可能会遵循以下这些步骤：

1.  从隔壁疯狂的科学家拿到收缩枪
2.  飞到南非
3.  旅行到 Addo 大象公园
4.  在公园里找一只大象
5.  用收缩枪射击大象
6.  把缩小的大象放进包里
7.  旅行到机场
8.  飞回自己的国家
9.  旅行到自己家
10. 把大象放进冰箱

问题解决了。

听起来很有趣，但你不可能用 JavaScript 捕捉大象并将其放进冰箱。让我们来看一个真实的例子。

## 让我们来看一个真实的例子

假设你想要创建一个按钮，在单击时会展示一个侧边栏。

[Codepen](https://codepen.io/zellwk/pen/zdqmLe)

制作这个 button-and-sidebar 组件的第一步是将组件分解成更小的部分。你可能会有下面几个问题：

1.  这个按钮的标记是怎样的？
2.  这个按钮应该是什么样子？
3.  第一次点击按钮会发生什么？
4.  再次点击按钮会发生什么？
5.  第三次点击按钮又会发生什么？
6.  侧边栏的标记是怎样的？
7.  侧边栏显示的时候是什么样子？
8.  侧边栏隐藏的时候是什么样子？
9.  侧边栏如何显示出来？
10. 侧边栏如何隐藏？
11. 页面加载时是否应该展示侧边栏？

第二步是为这些问题创造解决方案。

要创造解决方案，你需要了解有关的知识。在这个例子中，你需要充分了解 HTML、CSS 和 JavaScript。

如果你不知道这些问题的答案，不要担心。如果你能够把它充分地分解，你应该就能够在五分钟之内通过 Google 找出答案。

让我们来回答每一个问题。

###　这个按钮的标记是怎样的？

按钮标记是一个了类名为　`.button` 的　`<a>` 标签。

```html
<a href="#" class="button">Click me</a>
```

### 这个按钮应该是什么样子？

```css
.button {
  display: inline-block;
  font-size: 2em;
  padding: 0.75em 1em;
  background-color: #1ce;
  color: #fff;
  text-transform: uppercase;
  text-decoration: none;
}
```

### 当按钮点击一次，两次，三次，分别会发生什么？

按钮点击第一次的时候，侧边栏组件应该显示出来。再一次点击按钮时侧边栏应该消失。第三次点击按钮时，侧边栏组件会再次显示。

### 侧边栏的标记是怎样的？

侧边栏应该是一个 `<div>`，它包含了链接的列表：

```html
<div class="sidebar">
  <ul>
    <li><a href="#">Link 1</a></li>
    <li><a href="#">Link 2</a></li>
    <li><a href="#">Link 3</a></li>
    <li><a href="#">Link 4</a></li>
    <li><a href="#">Link 5</a></li>
    <li><a href="#">Link 6</a></li>
  </ul>
</div>
```

### 侧边栏显示的时候是什么样子？

侧边栏应该放在窗口的右侧。它需要固定在那个地方，因为用户要看见它。它的宽度应该是 300px。

当你解决了这些问题后，你可能会有类似下面的 CSS：

```css
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  width: 200px;
  background-color: #333;
}
.sidebar ul {
  margin: 0;
  padding: 0;
}
.sidebar li {
  list-style: none;
}
.sidebar li + li {
  border-bottom: 1px solid white;
}
.sidebar a {
  display: block;
  padding: 1em 1.5em;
  color: #fff;
  text-decoration: none;
}
```

### 侧边栏隐藏的时候是什么样子？

侧边栏应该向右移动 300px，这样它就离开了屏幕。

当你回答这个问题时，另外两个问题又出现在你的脑海里：

1.  你怎么知道侧边栏是隐藏的还是显示的？
2.  怎样设置隐藏的侧边栏的样式？

让我们来解答。

### 你怎么知道侧边栏是隐藏的还是显示的？

如果侧边栏有有　`is-hidden`　类名，这个侧边栏就是隐藏的。否则，它就是可见的。

### 怎样设置隐藏的侧边栏的样式？

我们使用 `translateX` 把侧边栏向右移动 300px，因为　`transform`　是更好的动画属性之一。你的样式会变成：

```css
.sidebar.is-hidden {
  transform: translateX(300px);
}
```

### 侧边栏如何显示出来？

侧边栏无法立即显示。它需要从右侧隐藏的视图中移动到左侧可见的视图中。

如果你了解 CSS，你可以使用　`transition`　属性。如果不了解，你可以通过 Google 找到答案。

```css
.sidebar {
  transition: transform 0.3s ease-out;
}
```

### 侧边栏如何隐藏？

它应该以它显示的方式消失，只是方向相反。这样，你就不必再编写额外的 CSS 代码。

### 页面加载时是否应该展示侧边栏？

不。只给我们所拥有的，在侧边栏的标记中添加 `is-hidden` 类名，并且侧边栏应该保持隐藏。

```html
<div class="sidebar hidden">
  <!-- links -->
</div>
```

至此，我们已经回答了几乎全部问题，除了一个——当按钮点击一次，两次，三次，分别会发生什么？

我们上面的回答比较含糊。我们知道当你点击时，侧边栏应该出现，但是如何出现？当你再次点击时，侧边栏应该消失，但是如何消失？

现在，我们可以更加细节性地回到这个问题。但是在这之前，如何知道你何时点击按钮？

### 如何知道你何时点击按钮？

如果你了解 JavaScript，你就知道我们可以在按钮上添加事件监听器并且监听一个 `click` 事件。如果你不了解 JavaScript，你可以去 Google。

在你添加事件监听器之前，你需要使用 `querySelector` 在标记中找到按钮。

```javascript
const button = document.querySelector('.button');

button.addEventListener('click', function() {
  console.log('button clicked');
});
```

### 第一次点击按钮会发生什么？

当按钮被点击了一次，我们应该移除 `is-hidden` 类名，这样侧边栏就会出现。在 JavaScript 中要移除类名，我们可以使用 `Element.classList.remove`。这意味着首先要选中侧边栏。

```javascript
const button = document.querySelector('.button');
const sidebar = document.querySelector('.sidebar');

button.addEventListener('click', function() {
  sidebar.classList.remove('is-hidden');
});
```

### 再次点击按钮会发生什么？

再次点击按钮时，我们应该把 `is-hidden` 添加回侧边栏上，这样它就会消失。

可惜的是，我们无法在事件监听器中检测到按钮被点击了几次。最好的方法是，检查 `is-hidden` 类名是否在侧边栏上。如果在，我们将它移除。如果不在，我们为其添加。

```javascript
const button = document.querySelector('.button');
const sidebar = document.querySelector('.sidebar');

button.addEventListener('click', function() {
  if (sidebar.classList.contains('is-hidden')) {
    sidebar.classList.remove('is-hidden');
  } else {
    sidebar.classList.add('is-hidden');
  }
});
```

这样，你就有了一个初始的原型组件。

[Codepen](https://codepen.io/zellwk/pen/zdqmLe)

## 重构并提升

最后一步是重构并改善你的代码。这个步骤你现在可能不会自然而然地去做。当你更好的掌握了 JavaScript 时，你回过头来看时可能会注意到一些小细节。

在上面的这个例子中，你可以提出几个小问题：

1.  怎样使你的侧边栏组件对视觉残疾者可访问？
2.  如何使侧边栏组件对使用键盘的人更加易用？
3.  你能使用任何方法改善你的代码么？

对于第三点，如果你再进一步 Google，你或许会注意到 `toggle` 方法，如果存在该类名就删除，如果不存在则添加：

```javascript
const button = document.querySelector('.button');
const sidebar = document.querySelector('.sidebar');

button.addEventListener('click', function() {
  sidebar.classList.toggle('is-hidden');
});
```

## 总结

像程序员一样思考很简单。关键在于知道如何把问题分解成更小的部分。

当你把问题分解后，给你的小问题找到解决方法并且实现代码。在这个过程中，你会发现更多你之前没有考虑到的问题。解决它们。

当你编写完小问题的答案时，你的大问题也就有答案了。有时候，你需要把你写的这些小问题的步骤组装起来。

最后，当你创建了第一个解决方案时，工作并没有完成，总会有改善的余地。无论怎样，你可能无法现在就看到改进。放轻松休息一下，去做其它的事情，然后再回来。那时，你可以问更好的问题。

感谢阅读。这篇文章是否对你有帮助？如果我做到了，我希望[你分享它](http://twitter.com/share?text=How%20to%20think%20like%20a%20programmer%20by%20@zellwk%20👇%20&url=https://zellwk.com/blog/think/&hashtags=JavaScript,%20thinkLikeProgrammers)。你或许能帮助到和你在读这篇文章之前有同样感觉的人。谢谢。
