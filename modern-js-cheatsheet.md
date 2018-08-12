# 现代 JavaScript 备忘单

<!-- TOC -->

- [现代 JavaScript 备忘单](#现代-javascript-备忘单)
  - [介绍](#介绍)
    - [动机](#动机)
    - [补充资源](#补充资源)
  - [概念](#概念)
    - [变量声明：var, const, let](#变量声明var-const-let)
      - [简短解释](#简短解释)
      - [示例代码](#示例代码)
      - [详细解释](#详细解释)
      - [外部资源](#外部资源)
    - [箭头函数](#箭头函数)
      - [示例代码](#示例代码-1)
      - [详细解释](#详细解释-1)
      - [补充资源](#补充资源-1)
    - [函数参数默认值](#函数参数默认值)
      - [补充资源](#补充资源-2)
    - [结构对象和数组](#结构对象和数组)
      - [示例代码](#示例代码-2)
      - [补充资源](#补充资源-3)
    - [数组方法 - map / filter / reduce](#数组方法---map--filter--reduce)
      - [示例代码](#示例代码-3)

<!-- /TOC -->

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

##### 简洁

箭头函数在许多方面都比传统函数更简洁。让我们看一下各种可能的情况：

- 隐式返回 VS 显式返回

  显式返回是在函数体中必须使用 return 关键词。

  ```javascript
  function double(x) {
    return x * 2; // 这个函数显式返回 x * 2，使用了 return 关键词
  }
  ```

  用传统的方式编写函数时返回总是显式的，但是对于箭头函数，你可以使用隐式返回，这意味着你不需要使用 return 关键词来返回一个值。

  ```javascript
  const double = x => {
    return x * 2; // 显式返回
  };
  ```

  由于这个函数只返回了一些东西（在 return 关键词之前没有指令），我们可以使用隐式返回。

  ```javascript
  const double = x => x * 2; // 正确，返回 x * 2
  ```

  要这样做，我们只需要移除**大括号**和 **return** 关键词。这就是为什么我们称之为隐式返回，因为这里没有 return 关键词，但是这个函数确实会返回 `x * 2`。

  > 注：如果你的函数没有返回值（有副作用），那它既不能显示返回，也不能隐式返回。

  此外，如果你想隐式的地返回一个对象，你必须对其使用圆括号，因为它会和大括号冲突：

  ```javascript
  const getPerson = () => ({ name: 'Nick', age: 24 });
  console.log(getPerson()); // { name: 'Nick', age: 24 }
  ```

- 只有一个参数

  如果你的函数只有一个参数，你可以省略圆括号。如果我们回到上面的 double 代码：

  ```javascript
  const double = (x) => x * 2; // 这个箭头函数只接受一个参数
  ```

  参数的圆括号是可以省略的：

  ```javascript
  const double = x => x * 2; // 这个箭头函数只接受一个参数
  ```

- 没有参数

  如果箭头函数中没有参数，你就需要为其提供圆括号，否则它就是无效的语法：

  ```javascript
  // 有圆括号
  () => {
    const x = 2;
    return x;
  }
  ```

  ```javascript
  // 没有圆括号，无法运行
  => {
    const = 2;
    return x;
  }
  ```

##### _this_ 引用

要理解箭头函数引入 this 的精妙之处，你首先需要知道 [this](https://mbeaudru.github.io/modern-js-cheatsheet/#this_def) 在 JavaScript 的行为。

在箭头函数中，_this_ 相当于封闭的执行上下文的 _this_ 的值。这意味箭头函数不会创造新的 _this_，箭头函数会在其上下文中寻找 _this_。

不在箭头函数中，如果你想在一个函数的内部函数中访问 this 的变量，你就必须使用 `that = this` 或者 `self = this` 这种技巧。

举个例子，在 myFunc 内部使用 setTimeout 函数：

```javascript
function myFunc() {
  this.myVar = 0;
  var that = this; // that = this 技巧
  setTimeout(function() {
    // 在这个函数作用域中又创建了新的 this
    that.myVar++;
    console.log(that.myVar); // 1

    console.log(this.myVar); // undefined -- 查看上面函数的定义
  }, 0);
}
```

但是在箭头函数中，this 时从上下文获取的：

```javascript
function myFunc() {
  this.myVar = 0;
  setTimeout(() => {
    // this 是从上下文环境获取的，意味着是 myFunc
    this.myVar++;
    console.log(myVar); // 1
  }, 0);
}
```

#### 补充资源

- [Arrow functions introduction - WesBos](http://wesbos.com/arrow-functions/)
- [JavaScript arrow function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [Arrow function and lexical _this_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

### 函数参数默认值

从 ES2015 JavaScript 更新开始，你可以用以下的语法为函数参数设置默认值：

```javascript
function myFunc(x = 10) {
  return x;
}
console.log(myFunc()); // 10
console.log(myFunc(5)); // 5

console.log(myFunc(undefined)); // 10
console.log(myFunc(null)); // null
```

默认参数限于且仅限于下面两种情况：

- 没有提供参数
- 参数为 _undefined_

换句话说，如果你传入 _null_，默认参数则不会应用。

> 注：默认值也可以与解构参数一起使用（参见下一个概念的示例）。

#### 补充资源

- [Default parameter value - ES6 Features](http://es6-features.org/#DefaultParameterValues)
- [Default parameters - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

### 结构对象和数组

*解构*是通过把存储在对象或数组中的数据提取出来创建变量的一种非常方便的方法。

举几个例子，解构可以用在结构函数参数，或者 React 项目中的 _this.props_ 中。

#### 示例代码

- 对象

  让我们为所有的示例考虑下面这个对象：

  ```javascript
  const person = {
    firstName: 'Nick',
    lastName: 'Anderson',
    age: 35,
    sex: 'M'
  };
  ```

  不使用解构：

  ```javascript
  const firstName = person.firstName;
  const age = person.age;
  const city = person.city || 'Pairs';
  ```

  使用解构，只需一行：

  ```javascript
  const { firstName: first, age, city = 'Pairs' } = person; // 就是这样

  console.log(age); // 35 -- 一个新变量被创建并且等于 person.age
  console.log(first); // "Nick" -- 一个新变量被创建并且等于 person.firstName
  console.log(firstName); // ReferenceError -- person.firstName 已经存在，但是新创建的变量名为 first
  console.log(city); // "Pairs" -- 一个新变量 city 被创建，然而 person.city 是 undefined，city 等于提供的默认值 "Pairs"
  ```

  > 注：在 `const { age } = person;` 中，*const* 关键词后面的大括号既不是声明对象，也不是语句块，而是解构语法。

- 函数参数

  解构也经常在函数中用于解构参数。

  没有解构：

  ```javascript
  function joinFirstLastName(person) {
    const firstName = person.firstName;
    const lastName = person.lastName;
    return firstName + '-' + lastName;
  }

  joinFirstLastName(person); // "Nick-Anderson"
  ```

  通过解构参数 person，我们可以得到一个更简洁的函数：

  ```javascript
  function joinFirstLastName({ firstName, lastName }) {
    return firstName + '-' + lastName;
  }
  joinFirstLastName(person); // "Nick-Anderson"
  ```

  解构和[箭头函数](https://mbeaudru.github.io/modern-js-cheatsheet/#arrow_func_concept)一起使用可以更加简洁：

  ```javascript
  const joinFirstLastName = ({ firstName, lastName }) => firstName + '-' + lastName;

  joinFirstLastName(person); // "Nick-Anderson"
  ```

- 数组

  思考以下数组：

  ```javascript
  const myArray = ['a', 'b','c'];
  ```

  没有解构：

  ```javascript
  const x = myArray[0];
  const x = myArray[1];
  ```

  使用解构：

  ```javascript
  const [x, y] = myArray; // 就这样

  console.log(x); // "a"
  console.log(y); // "b"
  ```

#### 补充资源

- [ES6 Features - Destructuring Assignment](http://es6-features.org/#ArrayMatching)
- [Destructuring Objects - WesBos](http://wesbos.com/destructuring-objects/)
- [ExploringJS - Destructuring](http://exploringjs.com/es6/ch_destructuring.html)

### 数组方法 - map / filter / reduce

map / filter / reduce 都是数组的方法，它们源于一种称为[函数式编程](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)的编程模式。

总之：

- **Array.prototype.map()** 接受一个数组，对其每个元素执行某些操作，并且返回转换后元素的数组
- **Array.prototype.filter()** 接受一个数组，决定是否保留一个元素，并且只返回保留下的元素的数组
- **Array.prototype.reduce()** 接受一个数组并将所有元素归并为一个单独的值（即返回值）

我推荐在遵循函数式编程的原则下尽可能地使用它们，因为它们可组合、简洁，而且优雅。

用这三种放，你在大多数情况下就可以避免使用 for 或 forEach 循环。当你想使用 for 循环时，试着使用 map / filter / reduce 来做。第一次使用时你可能会纠结，因为它需要你学习一种新的思考方式，但是当你理解了，一切就会变得更简单。

#### 示例代码

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6];
const doubleNumbers = numbers.map(n => n * 2); // [0, 2, 4, 6, 8, 10, 12]
const evenNumbers = numbers.filter(n => n % 2 === 0); // [0, 2, 4, 6]
const sum = numbers.reduce((prev, next) => prev + next, 0); // 21
```

通过组合 map，filter 和 reduce 来计算 10 年级及其以上年级的成绩总和：

```javascript
const students = [
  { name: 'Nick', grade: 10 },
  { name: 'John', grade: 15 },
  { name: 'Julia', grade: 19 },
  { name: 'Nathalie', grade: 9 }
];

const aboveTenSum = students
  .map(student => student.grade)
  .filter(grade => grade >= 10)
  .reduce((prev, next) => prev + next, 0);
```
