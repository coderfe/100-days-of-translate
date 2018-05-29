# Javascript 正则表达式指南

> 通过这篇简短的指南，你可以了解所有关于 Javascript 正则表达式的信息，这篇指南总结了最重要的概念，并且用示例展示了这些概念。

<!-- TOC -->

- [Javascript 正则表达式指南](#javascript-正则表达式指南)
  - [正则表达式简介](#正则表达式简介)
  - [很难但很有用](#很难但很有用)
  - [正则表达式看起来是什么样](#正则表达式看起来是什么样)
  - [正则如何工作](#正则如何工作)
  - [（标志位）Anchoring](#标志位anchoring)
  - [范围匹配](#范围匹配)
  - [范围内多次匹配](#范围内多次匹配)
  - [否定模式](#否定模式)
    - [元字符](#元字符)
  - [正则表达式中的或](#正则表达式中的或)
  - [量词](#量词)
    - [`+`](#)
    - [`*`](#)
    - [`{n}`](#n)
    - [`{n,m}`](#nm)
  - [可选项](#可选项)
  - [分组](#分组)
  - [捕获组](#捕获组)
    - [可选分组](#可选分组)
    - [引用分组](#引用分组)
    - [命名捕获组](#命名捕获组)
  - [使用无组 match 和 exec](#使用无组-match-和-exec)
  - [非捕获组](#非捕获组)
  - [修饰符](#修饰符)
  - [检查正则表达式](#检查正则表达式)
  - [转义](#转义)
  - [字符串边界](#字符串边界)
  - [使用正则表达式替换](#使用正则表达式替换)
  - [贪婪匹配](#贪婪匹配)
  - [前瞻：根据字符串后面的内容匹配字符串](#前瞻根据字符串后面的内容匹配字符串)
  - [后瞻: 根据字符串前面的内容匹配字符串](#后瞻-根据字符串前面的内容匹配字符串)
  - [正则表达式和 Unicode](#正则表达式和-unicode)

<!-- /TOC -->

## 正则表达式简介

正则表达式（也称之为 regx）是一种高效处理字符串的方法。

通过使用特殊语法构造正则表达式，你可以用来：

- 在字符串中查找文本
- 替换字符串中的子字符串
- 从字符串中提取信息

几乎所有的编程语言都实现了正则表达式。每个实现之间的差异很小，但是一般性概念几乎适用于任何语言。

Regular Expressions date back to the 1950s, when it was formalized as a conceptual search pattern for string processing algorithms.

由于在 UNIX 中（如 grep 和 sed）和受欢迎的编辑器中实现，正则表达式越来越受欢迎，并且被引入到 Perl 编程语言中，随后被引入到更多的编程语言。

Javascript 和 Perl 一样，是支持正则表达式直接内置在该语言中的编程语言之一。

## 很难但很有用

如果没有投入必要的时间去理解它们，正则表达式对于初学者来说简直是一派胡言，而且对于专业的开发人员来讲也经常如此。

神秘的正则表达式很难写、难以理解并且难以维护和修改。

但是有些时候正则是处理一些字符串问题的最明智的方法，因此它是一个非常有价值的工具。

这篇教程的目的在于用一种简单的方式向你介绍 Javascript 的正则表达式，并且提供给你阅读和创造你自己的正则表达式的所有信息。

经验法则是，简单的正则表达式是易写并易读的，而复杂的正则表达式如果不深入掌握基本知识，很快就会变得一团糟。

## 正则表达式看起来是什么样

在 Javascript 中的正则表达式是一个对象，可以用两种方式定义。

第一种是使用构造函数初始化一个新的 RegExp 对象：

```javascript
const re1 = new RegExp('hey');
```

第二种是使用正则表达式的字面量形式：

```javascript
const re2 = /hey/;
```

你知道 Javascript 有对象字面量和数组字面量吗？它也有正则表达式字面量。

在上面的例子中，`hey` 称之为 **模式**。在字面量形式中它由正斜杠分隔，但在构造函数中则不是。

这是两种形式之间第一个重要的区别，但是在稍后我们会看到其他的不同之处。

## 正则如何工作

我们在上面定义的 `re1` 正则表达式非常简单。它会搜索字符串 `hey` ，没有任何限制：字符串可以包含许多文本，`hey` 也在其中，regex 是满足要求的。字符串也可以仅包含 `hey` ，同样也是满足正则匹配的的。

这很简单。

你可以使用 `RexExp.test(String)` 来测试正则表达式，它会返回一个布尔值：

```javascript
re1.test('hey');  // ✅
re1.test('blablabla hey blablabla');  // ✅

re1.test('he');  // ❌
re1.test('blablabla');  //❌
```

在上面的示例中，我们检查了 `hey` 是否满足存储在 `re1` 中的正则表达式模式。

这是它能做的最简单的事儿，但是你已经了解了很多正则表达式的概念。

## （标志位）Anchoring

`/hey/` 匹配字符串任意位置的 `hey` 字符串。

如果你想匹配以 `hey` **开头**的字符串，使用 `^` 操作符：

```javascript
/^hey/.test('hey');  // ✅
/^hey/.test('bla hey');  // ❌
```

如果你想匹配以 `hey` **结尾**的字符串，使用 `$` 操作符：

```javascript
/hey$/.test('hey');  // ✅
/hey$/.test('bla hey');  // ✅
/hey$/.test('hey you');  // ❌
```

结合上面的方法，如果想要完全匹配字符串 `hey` ，仅需要以下代码：

```javascript
/^hey$/.test('hey'); // ✅
```

匹配某个字符串以一个子字符串开头且以另一个子字符串结尾，你可以使用 `.*` ，它用来匹配任意字符不出现或者出现多次。

```javascript
/^hey.*joe$/.test('hey joe');  // ✅
/^hey.*joe$/.test('heyjoe');  // ✅
/^hey.*joe$/.test('hey how are you joe');  // ✅
/^hey.*joe$/.test('hey joe!');  // ❌
```

## 范围匹配

你可以选择在某个范围内匹配任意字符，而不是匹配某个特定的字符串，例如：

```javascript
/[a-z]/  // a, b, c, ..., x, y, z
/[A-Z]/  // A, B, C, ..., X, Y, Z
/[a-c]/  // a, b, c
/[0-8]/  // 0, 1, 2, ..., 7, 8, 9
```

这些正则表达式能过够匹配包含在这些范围内的任意一个字符：

```javascript
/[a-z]/.test('a');  // ✅
/[a-z]/.test('1');  // ❌
/[a-z]/.test('A');  // ❌

/[a-c]/.test('d');  // ❌
/[a-c]/.test('dc');  // ✅
```

范围匹配可以组合起来：

```javascript
/[a-z0-9A-Z]/
```

```javascript
/[A-Za-z0-9]/.test('a');  // ✅
/[A-Za-z0-9]/.test('1');  // ✅
/[A-Za-z0-9]/.test('A');  // ✅
```

## 范围内多次匹配

你可以通过使用 `-` 字符检查一个字符串是否包含一个且仅一个在该范围内的字符：

```javascript
/[^a-zA-z0-9]$/

/[^a-zA-z0-9]$/.test('A');  // ✅
/[^a-zA-z0-9]$/.test('Ab');  // ❌
```

## 否定模式

The ^ character at the beginning of a pattern anchors it to the beginning of a string.

在范围匹配中使用 `^` 会否定正则表达式，像这样：

```javascript
/[^A-Za-z0-9]/.test('a'); // ❌
/[^A-Za-z0-9]/.test('A'); // ❌
/[^A-Za-z0-9]/.test('1'); // ❌
/[^A-Za-z0-9]/.test('@'); // ✅
```

### 元字符

- `\d` 匹配任何数字，等价于 `[0-9]`
- `\D` 匹配任何非数字，等价于 `[^0-9]`
- `\w` 匹配任何字母或数字，等价于 `[A-Za-z0-9]`
- `\W` 匹配任何非字母或数字，等价于 `[^A-Za-z0-9]`
- `\s` 匹配任何空白字符：空格、制表符、换行及 Unicode 空白
- `\S` 匹配非空白字符
- `\0` 匹配 null
- `\n` 匹配换行符
- `\t` 匹配制表符
- `\uXXXX` 匹配 [unicode](https://flaviocopes.com/unicode/) 字符（需要 `u` 标识）
- `.` 匹配非换行符（例如 `\n`）的任意字符（除非你使用 `n` 标志，稍后将谈到它）。
- `[^]` 匹配任意字符，包括换行符。在多行文本中非常有用。

## 正则表达式中的或

如果你要搜索一个字符串或另一个字符串，可以使用 `|` 操作符：

```javascript
/hey|ho/.test('hey');  // ✅
/hey|ho/.test('ho');  // ✅
```

## 量词

假如你有以下正则表达式，检查一个字符串中是否包含一个数字并且没有其它内容：

```javascript
/^\d$/
```

你可以使用 `?` 使其成为可选项，也就是说需包含 0 个或 1 个：

```javascript
/^\d?$/
```

但是如果你想匹配多个数字呢？

你可以使用 `+` 、`*`、`{n}` 及 `{n,m}` 4 种方式来实现。

### `+`

匹配 1 次或多次。

```javascript
/^\d+$/

/^\d+$/.test('12');  // ✅
/^\d+$/.test('14');  // ✅
/^\d+$/.test('144343');  // ✅
/^\d+$/.test('');  // ❌
/^\d+$/.test('1a');  // ❌
```

### `*`

匹配 0 次或多次。

```javascript
/^\d*$/

/^\d*$/.test('12');  // ✅
/^\d*$/.test('14');  // ✅
/^\d*$/.test('144343');  // ✅
/^\d*$/.test('');  // ✅
/^\d*$/.test('1a');  // ❌
```

### `{n}`

完全匹配 n 次。

```javascript
/^\d{3}$/

/^\d{3}$/.test('3');  // ✅
/^\d{3}$/.test('12');  // ❌
/^\d{3}$/.test('1234');  // ❌

/^[A-Za-z0-9]{3}$/.test('Abc');  // ✅
```

### `{n,m}`

匹配 n-m 次。

```javascript
/^\d{3,5}$/

/^\d{3,5}$/.test('123');  // ✅
/^\d{3,5}$/.test('1234');  // ✅
/^\d{3,5}$/.test('12345');  // ✅
/^\d{3,5}$/.test('123456');  // ❌
```

`m` 可以被省略用以至少匹配 `n` 次：

```javascript
/^\d{3,}$/

/^\d{3,}$/.test('12');  // ❌
/^\d{3,}$/.test('123');  // ✅
/^\d{3,}$/.test('12345');  // ✅
/^\d{3,}$/.test('123456789');  // ✅
```

## 可选项

使用 `?` 跟在某个字符后面，使其成为可选项：

```javascript
/^\d{3}\w$/

/^\d{3}\w$/.test('123');  // ✅
/^\d{3}\w$/.test('123a');  // ✅
/^\d{3}\w$/.test('123ab');  // ❌
```


## 分组

通过使用圆括号，你可以创建字符组：`(...)`

这个例子匹配 3 位数字后面跟 1 个或多个字母或数字字符：

```javascript
/^(\d{3})(\w+)$/

/^(\d{3})(\w+)$/.test('123');  // ❌
/^(\d{3})(\w+)$/.test('123s');  // ✅
/^(\d{3})(\w+)$/.test('123something');  // ✅
/^(\d{3})(\w+)$/.test('1234');  // ✅
```

## 捕获组

目前为止，我们已经看到了如何检测一个字符串是否包含一个特定的模式。

正则表达式非常好用的一个功能是捕获一部分字符串，并将其凡在一个数组中。

你可以使用组来执行这一操作，尤其是捕获组。

默认情况下，组是捕获组。现在，我们不使用 `RegExp.tets(String)` （如果模式满足就返回一个布尔值）这种方法，我们使用下面的方法之一：

- `String.match(RegExp)`
- `RegExp.exec(String)`

它们完全相同，并会返回一个数组，第一项是完全匹配的字符串，然后是每个分组匹配的结果。

如果没有匹配到，则返回 `null` ：

```javascript
'123s'.match(/^(\d{3})(\w+)$/);
// Array ['123s', '123', 's']

/^(\d{3})(\w+)$/.exec('123s');
// Array ['123s', '123', 's']

'hey'.match(/(hey|ho)/);
// Array ['hey', 'hey']

/(hey|ho)/.exec('hey');
// Array ['hey', 'hey']

/(hey|ho)/.exec('hi');
// null
```

当一个分组被匹配到多次的情况时，最后一次匹配的结果将被置于数组中。

```javascript
'123456789'.exec(/(\d)+/);
// Array ['123456789', '9']
```

### 可选分组

捕获组可以通过使用 `(...)?` 成为可选分组。如果匹配不到，则生成的数组中将包含 `undefined` ：

```javascript
/^(\d{3})(\s)?(\w+)$/.exec('123 s');
// Array [ "123 s", "123", " ", "s" ]

/^(\d{3})(\s)?(\w+)$/.exec('123s');
// Array [ "123s", "123", undefined, "s" ]
```

### 引用分组

每个匹配到的分组会被分配一个数字。`$1` 指向第一个，`$2` 指向第二个等等。这将有助于我们接下来要讨论的替换字符串。

### 命名捕获组

这是 [ES2018](https://flaviocopes.com/ecmascript/) 的新功能。

可以为组命名，而不仅仅是在结果数组中为其分配一个插槽：

```javascript
const re = /(?<year>\d{4}-(?<month>\d{2})-(?<day>\d{2}))/;
const result = re.exec('2018-05-21');

// result.groups.year === '2018';
// result.groups.month === '05';
// result.groups.day === '21';
```

## 使用无组 match 和 exec

使用无组 `match` 和 `exec` 有所不同：数组中的第一项不是完全匹配的字符串，而直接是匹配到的内容：

```javascript
/hey|ho/.exec('hey');
// Array [ "hey" ]

/(hey).(ho)/.exec('hey ho');
// Array [ "hey ho", "hey", "ho" ]
```

## 非捕获组

默认情况下，分组是捕获组，你需要一种方式在结果数组中忽略一些分组。这可以使用非捕获组，它们以 `(?:...)` 开头：

```javascript
'123s'.match(/^(\d{3})(?:\s)(\w+)$/);
// null

'123 s'.match(/^(\d{3})(?:\s)(\w+)$/)；
// Array ['123 s', '123', 's']
```

## 修饰符

你可以在任何正则表达式中使用下面的修饰符：

- `g` 多次匹配模式。
- `i` 忽略大小写。
- `m` 多行匹配。这这种模式下，`^` 和 `$` 匹配整个字符串的开头和结尾。如果没有此修饰符，它们将匹配每行的开头和结尾。
- `u` 支持 unicode （在 ES6/2015 中引入）。
- `s` (new in ES2018) short for single line, it causes the . to match new line characters as well.

修饰符可以结合使用并添加在正则表达式的末尾：

```javascript
/hey/ig.test('HEy'); // ✅
```

或者作为 RegExp 对象构造函数的第二个参数：

```javascript
new RegExp('hey', 'ig').test('HEy'); // ✅
```

## 检查正则表达式

给定一个正则表达式，你可以检查它的属性：

- `source` 模式字符串
- `mutiline` `m` 修饰符为 true
- `global` `g` 修饰符为 true
- `ignoreCase` `i` 修饰符为 true
- `lastIndex` 

```javascript
/^(\w{3})$/i.source  // '^(\\d{3})(\\w+)$'
/^(\w{3})$/i.nutiline  // false
/^(\w{3})$/i.global  // false
/^(\w{3})$/i.ignoreCase  // true
/^(\w{3})$/i.lastIndex  // 0
```

## 转义

这些字符是特殊的：

- `\`
- `/`
- `[]`
- `{}`
- `()`
- `?`
- `+`
- `*`
- `|`
- `.`
- `^`
- `$`

它们的特殊之处在于，它们是正则表达式中具有控制含义的字符，如果你将它们用在模式中匹配字符，你需要通过在它们前面加上反斜杠来转义。

```javascript
/^\\$/

/^\^$/.test('^');  // ✅
/^\$$/.test('$');  // ✅
```

## 字符串边界

`\b` 和 `\B` 用来检查字符串是在一个单词的开头还是结尾。

- `\b` 匹配在单词开头或结尾的一组字符
- `B` 匹配不再单词开头或结尾的一组字符

例如：

```javascript
'I saw a bear'.match(/\bbear/);  // Array ['bear']
'I saw a beard'.match(/\bbear/);  // Array ['bear']
'I saw a beard'.match(/\bbear\b/);  //null
'cool_bear'.match(/\bbear\b/);  //null
```

## 使用正则表达式替换

我们已经了解了如何检查字符串包含某一模式。

我们也了解了如何将匹配到的字符提取到一个数组中。

让我们了解一下如何根据模式来替换字符串。

Javascript 的 `String` 对象拥有一个 replace() 方法，它可以在没有正则表达式的情况下对字符串进行单次替换。

```javascript
'Hello world'.replace('world', 'dog');  // 'Hello dog'
'My dog is a good dog!'.replace('dog', 'cat');  // My cat is a good dog!
```

这个方法也接受一个正则表达式为参数：

```javascript
'Hello world!'.replace(/world/, 'dog');  // Hello dog!
```

使用 `g` 修饰符是在 Javascript 中替换多次出现的字符串的唯一方法：

```javascript
'My dog is a good dog!'.replace(/dog/g, 'cat');  // 'My cat is a good cat!'
```

分组可以让我们做更多有趣的事，比如移动字符串的某个部分：

```javascript
'Hello, world'.replace(/(\w+), (\w+)/, '$2: $1!!!');
// 'world: Hello!!!'
```

通过使用函数替换字符串，你可以做更有趣的事情。它将会接受一些参数，如 `String.match(RegExp)` 或 `RegExp.exec(String)` 的返回值，一些参数取决于组的数量：

```javascript
'Hello, world'.replace(/(\w+), (\w+)/, (matchedString, first, second) => {
  console.log(first);
  console.log(second);

  return `${second.toUpperCase()}: ${first}!!!`;
});

// 'WORLD: hello!!!'
```

## 贪婪匹配

正则表达式默认情况下是贪婪的。

这是什么意思呢？

例如这个正则表达式：

```javascript
/\$(.+)\s?/
```

它能够从字符串中提取美元数量。

```javascript
/\$(.+)\s?/.exec('This cost $100')[1];
// 100
```

但是如果在这个数字后有更多的单词，它会变得很怪异：

```javascript
/\$(.+)\s?/.exec('This cost $100 and it is less than $200')[1];
// 100 and it is less than $200
```

为什么呢？因为在 $ 后面的 `.+` 正则表达式会匹配任何字符，它不会停止匹配直到字符串的末尾。然后，它完成匹配，因为 `\s?` 使结束空白变成可选项。

要修复这个问题，我么需要告诉正则表达式保持惰性，并且尽可能少地执行匹配。我们可以通过在量词后面使用 `?` 符号来完成。

```javascript
/\$(.+)\s/.exec('This cost $100 and it is less than $200')[1];
// 100
```

> 我移除了 `\s` 后面的 `?` ，否则它只会匹配第一个数字，因为空格是可选的。

因此，`?` 依其位置不同表示的含义也不同，因为它既可以是一个量词，也可以是惰性模式的标识。

## 前瞻：根据字符串后面的内容匹配字符串

用 `?=` 来匹配后面跟了特定字符串的字符串。

```javascript
/Roger(?=Waters)/

/Roger(?= Waters)/.test('Roger is my dog');  // false
/Roger(?= Waters)/.test('Roger is my dog and Roger Waters is a famous musician');  // false
```

`?!` 执行反向操作，用来匹配后面没有特定子字符串的字符串。

```javascript
/Roger(?!Waters)/

/Roger(?! Waters)/.test('Roger is my dog');  // true
/Roger(?! Waters)/.test('Roger Waters is a famous musician');  // 
```

## 后瞻: 根据字符串前面的内容匹配字符串

这是 [ES2018](https://flaviocopes.com/ecmascript/) 的功能。

前瞻匹配使用 `?=` 符号，后瞻匹配使用 `?<=` 符号。

```javascript
/(?<=Roger) Waters/

/(?<=Roger) Waters/.test('Pink Waters is my dog');  // false
/(?<=Roger) Waters/.test('Roger is my dog and Roger Waters is a famous musician');  // true
```

负向后瞻 `?<!`

```javascript
/(?<!Roger) Waters/

/(?<!Roger) Waters/.test('Pink Waters is my dog');  // true
/(?<!Roger) Waters/.test('Roger is my dog and Roger Waters is a famous musician');  // false
```

## 正则表达式和 Unicode
