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
      - [详细解释](#详细解释-2)
      - [补充资源](#补充资源-4)
    - [展开运算符“...”](#展开运算符)
      - [示例代码](#示例代码-4)
      - [详细解释](#详细解释-3)
      - [补充资源](#补充资源-5)
    - [对象属性简写](#对象属性简写)
      - [详细解释](#详细解释-4)
      - [补充资源](#补充资源-6)
    - [Promises](#promises)
      - [示例代码](#示例代码-5)
      - [详细解释](#详细解释-5)
      - [补充资源](#补充资源-7)
    - [模板字符串](#模板字符串)
      - [示例代码](#示例代码-6)
      - [补充资源](#补充资源-8)
    - [标签模板字符串](#标签模板字符串)
      - [补充资源](#补充资源-9)
    - [导入 / 导出](#导入--导出)
      - [示例代码](#示例代码-7)
      - [补充资源](#补充资源-10)
    - [JavaScript *this*](#javascript-this)
      - [补充资源](#补充资源-11)
    - [Class](#class)
      - [示例代码](#示例代码-8)
      - [补充资源](#补充资源-12)
    - [Async Await](#async-await)
      - [示例代码](#示例代码-9)
      - [详细解释](#详细解释-6)
      - [补充资源](#补充资源-13)
    - [Truthy/Falsy](#truthyfalsy)

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

map / filter / reduce 都是数组的方法，它们源于[函数式编程](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)这种编程模式。

总之：

- **Array.prototype.map()** 接受一个数组，对其每个元素执行某些操作，并且返回转换后元素的数组
- **Array.prototype.filter()** 接受一个数组，决定是否保留一个元素，并且只返回保留下的元素的数组
- **Array.prototype.reduce()** 接受一个数组并将所有元素归并为一个单独的值（即返回值）

我推荐在遵循函数式编程的原则下尽可能地使用它们，因为它们可组合、简洁且优雅。

用这三种方法，你在大多数情况下就可以避免使用 for 或 forEach 循环。当你想使用 for 循环时，试着使用 map / filter / reduce 来做。第一次使用时你可能会纠结，因为它需要你学习一种新的思考方式，但是当你理解之后，一切就会变得简单。

#### 示例代码

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6];
const doubleNumbers = numbers.map(n => n * 2); // [0, 2, 4, 6, 8, 10, 12]
const evenNumbers = numbers.filter(n => n % 2 === 0); // [0, 2, 4, 6]
const sum = numbers.reduce((prev, next) => prev + next, 0); // 21
```

通过组合 map / filter / reduce 来计算 10 年级及其以上年级的学生的成绩总和：

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

#### 详细解释

为接下来的例子思考下面这个数字的数组：

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6];
```

##### Array.prototype.map()

```javascript
const doubleNumbers = numbers.map(function(n) {
  return n * 2;
});
console.log(doubleNumbers); // [0, 2, 4, 6, 8, 10, 12]
```

这里发生了什么？我们在 numbers 数组上使用了 .map 方法，map 会遍历数组的每一项并将其传递到函数中。函数的目的是从传递进来的参数产生并返回新值，便于 map 用来替换。

我们把函数提取出来使其更加清晰：

```javascript
const doubleN = function(n) { return n * 2; };
const doubleNumbers = numbers.map(doubleN);
console.log(doubleNumbers); // [0, 2, 4, 6, 8, 10, 12]
```

**注**：你会经常遇到这个方法和箭头函数结合使用

```javascript
const doubleNumbers = numbers.map(n => n * 2);
console.log(doubleNumbers); // [0, 2, 4, 6, 8, 10, 12]
```

`numbers.map(doubleN)` 产生 `[doubleN(0), doubleN(1), doubleN(2), doubleN(3), doubleN(4), doubleN(5), doubleN(6)]`，等于 `[0, 2, 4, 6, 8, 10, 12]`。

> 注：如果你不需要返回新数组，并且只想做一个有副作用的循环，你可能只想使用 for 或 forEach 循环而不是 map。

##### Array.prototype.filter()

```javascript
const evenNumbers = numbers.filter(function(n) {
  return n % 2 === 0;
});
console.log(evenNumbers); // [0, 2, 4, 6]
```

**注**：你会经常遇到这个方法和箭头函数结合使用

```javascript
const evenNumbers = numbers.filter(n => n % 2 === 0);
console.log(evenNumbers); // [0, 2, 4, 6]
```

我们在 numbers 数组上使用 .filter 方法时，filter 会遍历数组的每个元素并将其传递到函数中。函数的目标是返回一个 boolean 值并决定当前值是否保留。然后 filter 会返回保留下的元素的数组。

##### Array.prototype.reduce()

reduce 方法的目标在于将数组的所有元素归并为一个值。它如何归并这些元素取决于你。

```javascript
const sum = numbers.reduce(
  function(acc, n) {
    return acc + n;
  },
  0
);
console.log(sum); // 21
```

**注**：你会经常遇到这个方法和箭头函数结合使用

```javascript
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 21
```

就像 .map 和 .filter 方法，.reduce 可应用于数组并接受一个函数作为第一个参数。

不过这一次有些变化：

- .reduce 接受两个参数

  第一个参数是在每个迭代步骤都会调用的函数。

  第二个参数是在第一次迭代步骤（参阅下面的知识点）中累加器（在这里是 acc）的值。

- 函数参数

  作为 .reduce 第一个参数传递的函数接受两个参数。第一个（acc）是累加器变量，第二个参数（n）是当前元素。

  累加器变量等于前一次迭代的函数的返回值。在第一次迭代时，acc 等于你传递给 .reduce 的第二个参数的值。

###### 第一次迭代

`acc = 0` 因为我们为 reduce 传递的第二个参数是 0

`n = 0` numbers 数组的一个元素是 0

函数返回：acc + n -> 0 + 0 -> 0

###### 第二次迭代

`acc = 0` 因为它是前一次迭代返回的值

`n = 1` numbers 数组的第二个元素是 1

函数返回：acc + n -> 0 + 1 -> 1

###### 第三次迭代

`acc = 1` 因为它是前一次迭代返回的值

`n = 2` numbers 数组的第三个元素是 2

函数返回：acc + n -> 1 + 2 -> 3

###### 第四次迭代

`acc = 3` 因为它是前一次迭代返回的值

`n = 3` numbers 数组的第四个元素是 3

函数返回：acc + n -> 3 + 3 -> 6

###### [...]最后一次迭代

`acc = 15` 因为它是前一次迭代返回的值

`n = 6` numbers 数组的最后一个元素是 6

函数返回：acc + n -> 15 + 6 -> 21

最后一次迭代，.reduce 返回 21。

#### 补充资源

- [Understanding map / filter / reduce in JS](https://hackernoon.com/understanding-map-filter-and-reduce-in-javascript-5df1c7eee464)

### 展开运算符“...”

展开运算符在 ES2015 中引入，它用来将可迭代元素展开到可以容纳多个元素的地方。

#### 示例代码

```javascript
const arr1 = ['a', 'b', 'c'];
const arr2 = [...arr1, 'd', 'e', 'f']; // ['a', 'b', 'c', 'd', 'e', 'f']
```

```javascript
function myFunc(x, y, ...params) {
  console.log(x);
  console.log(y);
  console.log(params);
}
myFunc('a', 'b', 'c', 'd', 'e', 'f');
// 'a'
// 'b'
// ['c', 'd', 'e', 'f']
```

```javascript
const {x, y, ...z} = { x: 1, y: 2, a: 3, b: 4 };
cosnole.log(x); // 1
cosnole.log(y); // 2
cosnole.log(z); // { a: 3, b: 4 }

const n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }
```

#### 详细解释

##### 可迭代（如数组）

假设我们有下面两个数组：

```javascript
const arr1 = ['a', 'b', 'c'];
const arr2 = [arr1, 'd', 'e', 'f']; // [['a', 'b', 'c'], 'd', 'e', 'f']
```

arr2 的第一个元素是数组，因为 arr1 注入到了 arr2 中。但我们想要 arr2 是一组字母。为此，我么可以把 arr1 的元素展开到 arr2。

使用展开运算符：

```javascript
const arr1 = ['a', 'b', 'c'];
const arr2 = [...arr1, 'd', 'e', 'f']; // ['a', 'b', 'c', 'd', 'e', 'f']
```

##### 函数剩余参数

在函数参数中，我们可以使用 rest 操作符把参数注入到可以循环的数组中。每个函数已经绑定了一个 **arguments** 对象，它等于传递到函数中的所有参数的数组。

```javascript
funciton myFunc() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}

myFunc('Nick', 'Anderson', 10, 12, 6);
// "Nick"
// "Anderson"
// 10
// 12
// 6
```

但是假设我们需要这个函数能够创建一个新的学生和他的分数及平均分数。把前两个参数提取到单独的变量中，然后把所有的分数放在可以遍历的数组中，这样是否更方便？

这正是 rest 操作符允许我们做的！

```javascript
function createStudent(firstName, lastName, ...grades) {
  const avgGrade = grades.reduce((acc, curr) => acc + curr, 0) / grades.length;

  return {
    firstName: firstName,
    lastName: lastName,
    grades: grades,
    avgGrade: avgGrade
  };
}
const student = createStudent('Nick', 'Anderson', 10, 12, 6);
console.log(student);
// { firstName: 'Nick',
//   lastName: 'Anderson',
//   grades: [ 10, 12, 6 ],
//   avgGrade: 9.333333333333334
// }
```

> 注：createStudent 函数是糟糕的，因为我们没有检查 grades.length 是否存在或者是否为 0。但是为了易读，我没有处理这种情况。

##### 对象属性展开

对于这个，我建议你阅读上面的可迭代的 rest 操作符和函数参数。

```javascript
const myObj = { x: 1, y: 2, a: 3, b: 4 };
const {x, y, ...z} = myObj;
cosnole.log(x); // 1
cosnole.log(y); // 2
cosnole.log(z); // { a: 3, b: 4 }

const n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }
```

#### 补充资源

- [TC39 - Object rest/spread](https://github.com/tc39/proposal-object-rest-spread)
- [Spread operator introduction - WesBos](https://github.com/wesbos/es6-articles/blob/master/28%20-%20Spread%20Operator%20Introduction.md)
- [JavaScript & the spread operator](https://codeburst.io/javascript-the-spread-operator-a867a71668ca)
- [6 Great uses of the spread operator](https://davidwalsh.name/spread-operator)

### 对象属性简写

在为对象属性分配属性时，如果变量名和属性名一样，你可以这样写：

```javascript
const x = 10;
const myObj = { x };
console.log(myObj.x); // 10
```

#### 详细解释

通常（在 ES2015 之前）在声明新对象时，如果想用变量作为对象的属性值，你可能会写出以下代码：

```javascript
const x = 10;
const y = 20;

const myObj = {
  x: x,
  y: y
};

console.log(myObj.x);  // 10
console.log(myObj.y); // 20
```

如你所见，这是相当重复的，因为 myObj 的属性名称和变量名称是相同的。

在 ES2015 中，如果变量名和属性名称相同，那么你就这样简写：

```javascript
const x = 10;
const y = 20;

const myObj = {
  x: x,
  y: y
};

console.log(myObj.x);  // 10
console.log(myObj.y); // 20
```

#### 补充资源

- [Property shorthand - ES6 Features](http://es6-features.org/#PropertyShorthand)

### Promises

Promise 是可以从异步函数（[ref](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261#3cd0)）中同步返回的对象。

使用 Promises 可以避免[回调地狱](http://callbackhell.com/)，而且它也越来越频繁地使用在现代 JavaScript 项目中。

#### 示例代码

```javascript
const fetchingPosts = new Promise((res, rej) => {
  $.get('/posts')
    .done(posts => res(posts))
    .fail(err => rej(err));
});

fetchingPosts
  .then(posts => console.log(posts))
  .catch(err => console.log(err));
```

#### 详细解释

当你发起一次 Ajax 请求时，响应并不是同步的，因为请求的资源会花费一些时间。如果你请求的资源由于一些原因（404）不可用，它甚至不会返回。

为了处理这种情况，ES2015 引入了 promises。Promises 有三种不同的状态：

- Pending
- Fulfilled
- Rejected

假设我们使用 promise 处理一个获取资源 X 的 Ajax 请求。

##### 创建 promise

首先我们需要创建一个 promise。我们将使用 jQuery 的 get 方法来处理获取 X 的 Ajax 请求。

```javascript
const xFetchPromise = new Promise(function(resolve, reject) {
  $.get('X')
    .done(function(x) {
      resolve(x);
    })
    .fail(function(err) {
      reject(err);
    });
});
```

正如在上面的例子中看到的，Promise 对象接受一个执行器函数，这个函数接受两个参数 **resolve** 和 **reject**。这些参数都是函数，在被调用时，它们会把 promise 的 pending 状态分别转换为 fulfilled 和 rejected 状态。

Promise 会在实例创建完成和执行器函数被执行之后立即进入 pending 状态。一旦 resolve 或 reject 函数中任意一个函数被调用时，promise 就会调用与之相关的处理函数。

##### Promise 处理器用法

为了得到 promise 的结果（或错误），我们必须通过以下操作为其添加处理函数：

```javascript
xFetchPromise
  .then(function(x) {
    cosnole.log(x);
  })
  .catch(function(err) {
    console.log(err);
  });
```

如果 promise 成功了，则执行 resolve 并把函数作为 `.then` 参数执行。

如果 promise 失败了，则执行 reject 并把函数作为 `.catch` 参数执行。

#### 补充资源

- [JavaScript Promises for dummies - Jecelyn Yeen](https://scotch.io/tutorials/javascript-promises-for-dummies)
- [JavaScript Promise API - David Walsh](https://davidwalsh.name/promises)
- [Using promises - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [What is a promise - Eric Elliott](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)
- [JavaScript Promises: an Introduction - Jake Archibald](https://developers.google.com/web/fundamentals/getting-started/primers/promises)
- [Promise documentation - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

### 模板字符串

模板字符串是针对单行和多行字符串的[插值表达式](https://en.wikipedia.org/wiki/String_interpolation)。

换句话说，它是一种新语法，你可以在其中使用任何 JavaScript 表达式（例如变量）。

#### 示例代码

```javascript
const name = 'Nick';
`Hello ${name}, the following expression is equal to four: ${2+2}`
// Hello Nick, the following expression is equal to four: 4
```

#### 补充资源

- [String interpolation - ES6 Features](http://es6-features.org/#StringInterpolation)
- [ES6 Template Strings - Addy Osmani](https://developers.google.com/web/updates/2015/01/ES6-Template-Strings)

### 标签模板字符串

模板标签是可以放在[模板字符串](#模板字符串)前面的函数。当一个函数以这种方式被调用时，第一个变量是出现在模板插值之间的字符串数组，随后的参数是插值。使用展开操作符 `...` 来捕获它们。（[Ref: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals)）。

> 注：著名的 [styled-components](https://www.styled-components.com/) 非常依赖于这个功能。

下面是一个关于它如何运行的例子：

```javascript
function highlight(strings, ...values) {
  const interpolation = strings.reduce((prev, next) => {
    return prev + next + (values.length ? '<mark>' + values.shift() + '</mark>' : '');
  }, '');
  return interpolation;
}
const condiment = 'jam';
const meal = 'toast';

highlight`I like ${condiment} on ${meal}.`;
// I like <mark>jam</mark> on <mark>toast</mark>.
```

一个更有趣的例子：

```javascript
function comma(strings, ...values) {
  return strings.reduce((prev, next) => {
    let value = values.shift() || [];
    value = value.join(', ');
    return prev + next + value;
  }, '');
}

const snacks = ['apples', 'bananas', 'cherries'];
comma`I like ${snacks} to snack on.`;
// I like apples, bananas, cherries to snack on.
```

#### 补充资源

- [Wes Bos on Tagged Template Literals](http://wesbos.com/tagged-template-literals/)
- [Library of common template tags](https://github.com/declandewet/common-tags)

### 导入 / 导出

ES6 模块用于访问由其导入模块中显式导出的变量和函数。

我强烈建议你看看关于 import/export 的 MDN 资源（参见下面的补充资源），它既完整又通俗易懂。

#### 示例代码

##### 命名导出

命名导出用于从模块中导出几个值。

> 注：你只能导出有名称的[一等公民](https://en.wikipedia.org/wiki/First-class_citizen)。

```javascript
// mathConstants.js
export const pi = 3.14;
export const exp = 2.7;
export const alpha = 0.35;

// -----------------

// myFile.js
import {pi, exp} from 'mathConstants.js'; // 命名导入 -- 类似解构语法
console.log(pi); // 3.14
console.log(exp); // 2.7

// -----------------

// mySecondFile.js
import * as constants from 'mathConstants.js';  // 把所有导出的值注入到 constants 变量
console.log(constants.pi); // 3.14
console.log(constants.exp); // 2.7
```

虽然命名导入看起来像解构，但它们的语法不一样。它既不支持默认值，也不支持深解构。

此外，你可以使用别名，但是不同于解构中使用的语法：

```javascript
import {foo as bar} from 'myFile.js'; // foo 被导入并注入给新变量 bar
```

##### 默认 import/export

关于默认导出，每个模块中只有一个默认导出。一个默认导出可以是一个函数，一个类，一个对象或者其他任何东西。这个值被认为是“main”导出值，因为是最容易导入的值。[Ref: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export#Description)

```javascript
// coolNumber.js
const ultimateNumber = 42;
export default ultimateNumber;

// -----------------

// myFile.js
import number from 'coolNumber.js';
console.log(number); // 42
```

函数导出：

```javascript
// sum.js
export default function sum(x, y) {
  return x + y;
}

// -----------------

// myFile.js
import sum from 'sum.js';
const result = sum(1, 2);
console.log(result); // 3
```

#### 补充资源

- [ES6 Modules in bulletpoints](https://ponyfoo.com/articles/es6#modules)
- [Export - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [Import - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [Understanding ES6 Modules](https://www.sitepoint.com/understanding-es6-modules/)
- [Destructuring special case - import statements](https://ponyfoo.com/articles/es6-destructuring-in-depth#special-case-import-statements)
- [Misunderstanding ES6 Modules - Kent C. Dodds](https://medium.com/@kentcdodds/misunderstanding-es6-modules-upgrading-babel-tears-and-a-solution-ad2d5ab93ce0)
- [Modules in JavaScript](http://exploringjs.com/es6/ch_modules.html#sec_modules-in-javascript)

### JavaScript *this*

JavaScript 中 *this* 操作符的行为不同于其它语言，而且在大多数情况下决定它的是一个函数会如何被调用。（[Ref: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)）。

这个概念有许多微妙之处，而且非常难理解，我强烈建议你深入思考下面的补充资源。因此，我会提供我的个人理解来确定 this 等于什么。我从 [Yehuda Katz 写的这篇文章](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)学到了许多 this 的技巧。

```javascript
function myFunc() {
  //...
}

// 每个语句之后，你在 myFunc 中查找 this 的值

myFunc.call('myString', 'hello'); // "myString" -- .call 的第一个参数被注入到 this

// 在非严格模式下
myFunc('hello'); // window -- myFunc() 是 myFunc.call(window, 'hello') 的语法糖

// 在严格模式下
myFunc('hello'); // undefined -- myFunc() 是 myFunc.call(undefined, 'hello') 的语法糖
```

```javascript
var person = {
  myFunc: function() {/*...*/}
};

person.myFunc.call(person, 'test'); // person Object -- .call 的第一个参数被注入到 this
person.myFunc('test'); // person Object -- person.myFunc() 是 person.myFunc(person, 'test') 的语法糖

var myBoundFunc = person.myFunc.bind('hello');
person.myFunc('test'); // person Object -- bind 方法对原方法没有副作用
myBoundFunc('test'); // "hello" -- myBoundFunc 是 person.myFunc 把 this 绑定为 hello
```

#### 补充资源

- [Understanding JavaScript Function Invocation and “this” - Yehuda Katz](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
- [JavaScript this - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

### Class

JavaScript 是一种[基于原型的](https://en.wikipedia.org/wiki/Prototype-based_programming)语言（而 Java 是[基于类的](https://en.wikipedia.org/wiki/Class-based_programming)语言）。ES6 引入了 JavaScript 类，这意味着类是基于原型的继承的语法糖，而不是基于类的一种新的继承模型（[ref](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)）。

如果你熟悉其他语言中的类，JavaScript class 确实容易出错。如果是这样，尽量避免假设 JavaScript class 是如何工作的，并且以一种完全不同的概念去思考它。

由于这份文档不是试图从头开始教你这门语言，我相信你应该知道什么是原型以及它的行为表现。如果你不知道，参考示例代码下面的补充资源。

#### 示例代码

ES6 之前，原型语法：

```javascript
var Person = function(name, age) {
  this.name = name;
  this.age = age;
};

Person.prototype.stringSentence = function() {
  return 'Hello, my name is ' + this.name + ' and I\'m ' + this.age;
}
```

使用 ES6 class 语法：

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  stringSentence() {
    return `Hello, my name is ${this.name} and I'm ${this.age}`;
  }
}

const person = new Person('Manu', 23);
console.log(person.age); // 23
console.log(person.stringSentence()); // "Hello, my name is Manu and I'm 23"
```

#### 补充资源

对于理解原型：

- [Understanding Prototypes in JS - Yehuda Katz](http://yehudakatz.com/2011/08/12/understanding-prototypes-in-javascript/)
- [A plain English guide to JS prototypes - Sebastian Porto](http://sporto.github.io/blog/2013/02/22/a-plain-english-guide-to-javascript-prototypes/)
- [Inheritance and the prototype chain - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

对于理解 class：

- [ES6 Classes in Depth - Nicolas Bevacqua](https://ponyfoo.com/articles/es6-classes-in-depth)
- [ES6 Features - Classes](http://es6-features.org/#ClassDefinition)
- [JavaScript Classes - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

### Async Await

除了 [Promises](https://github.com/coderfe/100-days-of-translate/blob/master/modern-js-cheatsheet.md#promises) 之外，还有一种称为 `async / await` 的新语法可以处理异步代码。

async/await 函数试图简化使用 promises 的异步行为，并且在一组 Promises 中执行一些操作。正如 Promises 类似于结构化回调一样，async/await 类似于生成器和 Promises 的组合。async 函数总是返回一个 Promise。（[Ref: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)）。

> 注：在尝试 async/await 之前，你必须理解 Promises 的原理以及它如何工作，因为 async/await 依赖于 Promises。

> 注2：[await 必须使用在 async 函数中](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9#f3f0)，这意味着你不能再代码顶部使用 await，因为它不在 async 函数中。

####　示例代码

```javascript
async function getGithubUser(username) {
  const response = fetch(`https://api.github.com/users/${username}`);
  return response.json();
}

getGithubUser('mbeaudru')
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

#### 详细解释

async/await 是基于 Promises 的，但是它又允许一种更加紧凑的代码风格。

async 表示一个函数是异步的，而且始终会返回一个 Promise，你可以在 async 函数中使用 await 操作符来暂停该行的执行，直到表达式返回的 Promise 被 resolve 或者 reject。

```javascript
async function myFunc() {
  return 'Hello World';
}
myFunc().then(msg => console.log(msg)); // "Hello World"
```

当到达 async 函数的 return 语句时，Promise 会以 fulfilled 状态返回值。如果在 async 函数中抛出错误，Promise 的状态会转变为 rejected。如果 async 函数中没有任何返回值，在 async 函数实行完成时，它仍旧会返回没有值的 Promise。

await 操作符会等待 Promise 实现，并且只能用在 async 函数中。遇到 await 时，代码会暂停执行，直到 promises 完成。

> 注：*fetch* 是一个函数，它允许发起 Ajax 请求并返回 Promise。

让我们看看如何使用 promise 查找 Github 用户：

```javascript
function getGithubUser(username) {
  return fetch(`https://api.github.com/users/${username}`).then(res => res.json());
}

getGithubUser('mbeaudru')
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

这是 async/await：

```javascript
async function getGithubUser(username) {
  const response = await fetch(`https://api.github.com/users/${username}`);
  return response.json();
}

getGithubUser('mbeaudru')
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

当你需要链接相互依赖的 promises 时，async/await 语法非常方便。

举个例子，如果你需要获取 token 以便能够从数据库获取博客文章，并且获取作者的信息：

```javascript
async function fetchPostById(postId) {
  const token = (await fetch('token_url')).json().token;
  const post = (await fetch(`posts/${postId}?token=${token}`)).json();
  const author = (await fetch(`/users/${post.authorId}`)).json();

  post.author = author;
  return post;
}

fetchPostById('gzIrzeo64')
  .then(post => console.log(post))
  .catch(err => console.log(err));
```

##### 错误处理

除非为 await 添加 try/catch 语句，否则未捕获的异常——无论是在 async 函数主体抛出的，或者在 await 期间挂起——都将 reject async 函数返回的 promise，在 async 函数中使用 `throw` 语句和返回一个 reject 的 Promise 是一样的。（[Ref: MDN](https://ponyfoo.com/articles/understanding-javascript-async-await#error-handling)）。

> 注： Promises 的行为都是一致的。

在 Promise 中处理错误：

```javascript
function getUser() {
  return new Promise((res, rej) => rej('User no found'));
}

function getAvatarByUsername(userId) {
  return getUser(userId).then(user => user.avatar);
}

function getUserAvatar(username) {
  return getAvatarByUsername(username).then(avatar => { username, avatar });
}

getUserAvatar('mbeaudru')
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

等价的 async/await：

```javascript
async function getUser() {
  throw 'User not found';
}
async function getAvatarByUsername(userId) {
  const user = getUser(userId);
  return user.avatar;
}
async function getUserAvatar(username) {
  const avatar = getAvatarByUsername(username);
  return { username, avatar };
}

getUserAvatar('mbeaudru')
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

#### 补充资源

- [Async/Await - JavaScript.Info](https://javascript.info/async-await)
- [ES7 Async/Await](http://rossboucher.com/await/#/)
- [6 Reasons Why JavaScript’s Async/Await Blows Promises Away](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9)
- [JavaScript awaits](https://dev.to/kayis/javascript-awaits)
- [Using Async Await in Express with Node 8](https://medium.com/@Abazhenov/using-async-await-in-express-with-node-8-b8af872c0016)
- [Async Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [Using async / await in express with node 8](https://medium.com/@Abazhenov/using-async-await-in-express-with-node-8-b8af872c0016)

### Truthy/Falsy

JavaScript 中，truthy/falsy 是在 boolean 计算上下文中被转换为 boolean 型的值。一个 boolean 上下文的例子是 `if` 条件的评估。

每个值都将被转换为 `true`，除非它等于：

- `false`
- `0`
- ''（空字符串）
- `null`
- `undefined`
- `NaN`

以下是 boolean 上下文的示例：

- `if` 条件评估

  ```javascript
  if (myVar) {}
  ```

  `myVar` 可以是任何[一等公民](https://en.wikipedia.org/wiki/First-class_citizen)（变量、函数、boolean），但是它会被转换为布尔值，因为它在 boolean 上下文中。

- 在逻辑运算符**非** `!` 后面

  如果该操作符的单个操作数能够转换为 true，它会返回 false，否则返回 true。

  ```javascript
  !0 // true -- 0 是假值，所以它返回 true
  !!0 // false -- 0 是假值，!0 返回 true，!(!0) 返回 false
  !!'' // false -- '' 是假值，!'' 返回 true，!(!'') 返回 false
  ```

- Boolean 对象构造函数

  ```javascript
  new Boolean(0); // false
  new Boolean(1); // true
  ```

- 三元表达式

  ```javascript
  myVar ? 'truthy' : 'falsy'
  ```

在比较两个值时要注意。
