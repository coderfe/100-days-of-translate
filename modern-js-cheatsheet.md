# 现代 JavaScript 备忘单

## 介绍

### 动机

这份文档是一份备忘单，你会频繁地在现代项目和大多数现代示例代码中遇到它。

这份指南不是要从头开始教你 JavaScript，而是帮助那些具有基础知识的开发者，由于使用了 JavaScript 的概念，他们可能会挣扎于现代代码库（或者比如说学习 React）。

此外，我有时会提供一些有待商榷的个人见解，但在我这样做的时候会提及是这是个人推荐。

> 注：在这里介绍的大对数概念来自于 JavaScript 语言的更新（ES2015，通常称为 ES6）。你可以在[这里](http://es6-features.org/)找到在这次更新中添加的新功能；它做的非常好。

### 补充资源

但你很难理解一个概念时，我推荐你在下面的资源中查找答案：

- [MDN(Mozilla Developer Network)](https://developer.mozilla.org/en-US/search?q=)
- [You don't konw JS(book)](https://github.com/getify/You-Dont-Know-JS)
- [ES6 Feathers With examples](http://es6-features.org/)
- [WesBos Blog(ES6)](http://wesbos.com/category/es6/)
- [Reddit(JavaScript)](https://www.reddit.com/r/javascript/)
- [Google](https://www.google.com/) 查找指定的博客和资源
- [StackOverflow](https://stackoverflow.com/questions/tagged/javascript)

## 概念

### 变量声明：var, const, let

在 JavaScript 中，有三个关键词用于声明变量，而且每一个都有其不同之处。分别是 `var`、`let` 及 `const`。

#### 简短解释

`const` 关键词声明的变量不能在重新赋值，但是 `var` 和 `let` 可以。

我推荐总是默认使用 `const` 声明变量，如果你稍后要改变或重新为变量赋值，则使用 `let`。

|       | 作用域   | 能否重新赋值 | 可改变 | 临时死区 |
| ----- | -------- | ------------ | ------ | -------- |
| const | Block    | No           | Yes    | Yes      |
| let   | Block    | Yes          | Yes    | Yes      |
| var   | Function | Yes          | Yes    | No       |

#### 示例代码

```javascript
const person = 'Nick';
person = 'Jhon'; // 抛出错误，person 不能被重新赋值
```

```javascript
let person = 'Nick';
person = 'Jhon';
console.log(person); // "Jhon"，let 允许重新赋值
```

#### 详细解释

变量[作用域](https://mbeaudru.github.io/modern-js-cheatsheet/#scope_def)的粗略意思是“这个变量在代码中的哪些地方可用”。

##### var

`var` 声明的变量是*函数作用域*，意味着在函数中创建一个变量，函数中的任何事物都可以访问到该变量。此外，创建在函数中的*函数作用域*变量无法在函数外访问。
