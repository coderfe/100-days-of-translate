# 现代 JavaScript 备忘单

## 介绍

### 动机

这份文档是一份备忘单，你会频繁地在现代项目和大多数现代示例代码中遇到它。

这份指南不是要从头开始教你 JavaScript，而是帮助那些具有基础知识的开发者，由于使用了 JavaScript 的概念，他们可能会挣扎于现代代码库（或者比如说学习 React）。

此外，我有时会提供一些有待商榷的个人见解，但在我这样做的时候会提及是这是个人推荐。

> 注：在这里介绍的大多数概念来自于 JavaScript 语言的更新（ES2015，通常称为 ES6）。你可以在[这里](http://es6-features.org/)找到在这次更新中添加的新功能；它做的非常好。

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

`const` 关键词声明的变量不能再重新赋值，但是 `var` 和 `let` 可以。

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

我建议你把它想象为 X 作用域变量，意味着这个变量将是 X 的一个属性。

```javascript
function myFunction() {
  var myVar = 'Nick';
  console.log(myVar); // "Nick" - myVar 可以在函数内访问
}
console.log(myVar); // 抛出 ReferenceError，myVar 无法在函数外访问
```

仍然关注变量作用域，这是一个更加微妙的例子：

```javascript
function myFunction() {
  var myVar = 'Nick';
  if (true) {
    var myVar = 'Jhon';
    console.log(myVar); // "Jhon"，实际上 myVar 是函数作用域，我们只是用 Jhon 覆盖了之前的 myVar 的值
  }
  console.log(myVar); // "Jhon"，查看 if 块中的指令如何影响 myVar 的值
}
console.log(myVar); // 抛出 ReferenceError，myVar 无法在函数外访问
```

此外，var 声明的变量在运行时会被提升到作用域顶部。这就是我们通常所说的[变量提升](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)。

这部分代码：

```javascript
console.log(myVar); // undefined - 没有出现错误
var myVar = 2;
```

在执行时会理解为：

```javascript
var myVar;
console.log(myVar); // undefined - 没有出现错误
myVar = 2;
```

##### let

`var` 和 `let` 大致相同，但是 `let` 声明的变量：

- 是块作用域
- 赋值之前不能访问
- 不能再同样的作用域中重新声明

让我们用之前的例子来看看块作用域的影响：

```javascript
function myFunction() {
  let myVar = 'Nick';
  if (true) {
    let myVar = 'Jhon';
    console.log(myVar); // "Jhon"，实际上 myVar 是块作用域，我们在这儿又创建了一个变量，这个变量在这个块之外无法访问，而且完全独立于第一次创建的 myVar
  }
  console.log(myVar); // "Nick"，if 块中的指令不会影响 myVar 的值
}
cosnole.log(myVar); // 抛出 ReferenceError，myVar 无法在函数外访问
```

现在，let 和 const 变量在声明之前无法访问意味着什么：

```javascript
console.log(myVar); // 出现错误 ReferenceError
let myVar = 2;
```

和 var 变量相比，如果你尝试在 let 和 const 变量分配之前读取或写入时将出现一个错误。这种现象通常称为[临时死区](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let)或者 TDZ。

> 注：严格来讲，let 和 const 变量声明也会被提升，但却没有分配。因为创造它们是为了在分配之前不能被使用，直觉上并没有提升，但实际上提升了。如果你想了解更多，[在这里查找详细的解释](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified)。

此外，你不能重新定义 let 变量：

```javascript
let myVar = 2;
let myVar = 3; // SyntaxError
```

##### const

`const` 定义变量的行为类似于 `let`，但它们也不能重新赋值。

总之，const 定义的变量：

- 是块作用域
- 赋值之前不能访问
- 不能在相同作用域内重新声明
- 不能重新赋值

```javascript
const myVar = 'Nick';
myVar = 'Jhon'; // 抛出错误，不允许重新赋值
```

```javascript
const myVar = 'Nick';
const myVar = 'Jhon'; // 抛出错误，不允许再次声明
```

但是微妙之处在于：`const` 变量不是[不可变的](https://mbeaudru.github.io/modern-js-cheatsheet/#mutation_def)！具体来说，`const` 声明的*对象*和*数组*变量**可以**被改变。

对于对象：

```javascript
const persone = {
  name: 'Nick'
};
persone.name = 'Jhon'; // 可以运行！person 变量没有重新赋值，只是被改变
console.log(person.name); // "Jhon"
person = 'Sandra'; // 抛出错误，因为 const 声明的变量不允许重新赋值
```

对于数组：

```javascript
const person = [];
person.push('Nick'); // 可以运行！person 变量没有重新赋值，只是被改变
console.log(person[0]); // "Nick"
person = ['Nick']; // 抛出错误，因为 const 声明的变量不允许重新赋值
```

#### 外部资源

- [How let and const are scoped in JavaScript - WesBos](http://wesbos.com/javascript-scoping/)
- [Temporal Dead Zone (TDZ) Demystified](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified)

### 箭头函数

ES6 JavaScript 更新引入了箭头函数，这是声明和使用函数的另一个方式。这是它带来的好处：

- 更加简洁
- _this_ is picked up from surroundings
- 隐式返回

#### 示例代码

- 简洁及隐式返回

  ```javascript
  // 传统的方式
  function double(x) {
    return x * 2;
  }
  console.log(double(2)); // 4
  ```

  ```javascript
  // 箭头函数
  const double = x => x * 2;
  console.log(double(2)); // 4
  ```

- _this_ 引用

  在箭头函数中，this 相当于封闭执行上下文的 this 值。通常对于箭头函数，在调用函数内部的函数之前，你不必再使用 `that = this`。

  ```javascript
  function myFun() {
    this.myVar = 0;
    setTimeout(() => {
      this.myVar++;
      console.log(myVar); // 1
    }, 0);
  }
  ```

#### 详细解释
