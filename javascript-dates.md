# JavaScript Dates 终极指南

- [JavaScript Dates 终极指南](#javascript-dates-%E7%BB%88%E6%9E%81%E6%8C%87%E5%8D%97)
  - [简介](#%E7%AE%80%E4%BB%8B)
  - [Date 对象](#date-%E5%AF%B9%E8%B1%A1)
  - [初始化 Date 对象](#%E5%88%9D%E5%A7%8B%E5%8C%96-date-%E5%AF%B9%E8%B1%A1)
  - [时区](#%E6%97%B6%E5%8C%BA)
  - [日期转换和格式化](#%E6%97%A5%E6%9C%9F%E8%BD%AC%E6%8D%A2%E5%92%8C%E6%A0%BC%E5%BC%8F%E5%8C%96)
  - [Date 对象 getter 方法](#date-%E5%AF%B9%E8%B1%A1-getter-%E6%96%B9%E6%B3%95)
  - [修改日期](#%E4%BF%AE%E6%94%B9%E6%97%A5%E6%9C%9F)
  - [获取当前时间戳](#%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E6%97%B6%E9%97%B4%E6%88%B3)
  - [JavaScript 努力做到更好](#javascript-%E5%8A%AA%E5%8A%9B%E5%81%9A%E5%88%B0%E6%9B%B4%E5%A5%BD)
  - [根据区域格式化时间](#%E6%A0%B9%E6%8D%AE%E5%8C%BA%E5%9F%9F%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%97%B6%E9%97%B4)
  - [比较日期](#%E6%AF%94%E8%BE%83%E6%97%A5%E6%9C%9F)

> JavaScript 中处理日期可能很复杂。让我们一起学习这些怪癖并了解如何使用它们。

## 简介

处理日期可能很*复杂*。不论什么技术，开发者都会感到痛苦。

![pain][1]

JavaScript 通过强大的 `Date`对象为我们提供了日期处理功能。

> 这篇文章不会讨论 [Moment.js](http://momentjs.com/)，它是我认为处理日期最好的库，在处理日期时，你应该经常使用它。

## Date 对象

一个 Date 对象实例代表了一个时间点。

尽管称之为 __Date__，但是它也会处理 __time__。

## 初始化 Date 对象

我们可以如下来初始化一个 Date 对象：

```javascript
new Date();
```

它会创建一个指向当前时刻的 Date 对象。

在内部，日期表示自 1970 年 1 月 1 日（UTC）以来的毫秒数。这个日期非常重要，因为就计算机而言，那是一切的开始。

你也许很熟悉 UNIX 时间戳：它代表自那个著名日期以来经过的秒数。

> 重点：UNIX 时间戳以秒为单位，JavaScript 日期以毫秒为单位。

假设我们有一个 UNIX 时间戳，我们可以这样初始化 JavaScript Date 对象：

```javascript
const timestamp = 1530826365;
new Date(timestamp * 1000);
```

如果我们传递 0，将会得到一个代表 1970 年 1 月 1 日（UTC）日期的 JavaScript Date 对象。

```javascript
new Date(0);
```

如果传递字符串而非数字，那么 Date 对象就会使用 `parse` 方法来决定你要传递的日期。例如：

```javascript
new Date('2018-07-22')
new Date('2018-07') //July 1st 2018, 00:00:00
new Date('2018') //Jan 1st 2018, 00:00:00
new Date('07/22/2018')
new Date('2018/07/22')
new Date('2018/7/22')
new Date('July 22, 2018')
new Date('July 22, 2018 07:22:13')
new Date('2018-07-22 07:22:13')
new Date('2018-07-22T07:22:13')
new Date('25 March 2018')
new Date('25 Mar 2018')
new Date('25 March, 2018')
new Date('March 25, 2018')
new Date('March 25 2018')
new Date('March 2018') //Mar 1st 2018, 00:00:00
new Date('2018 March') //Mar 1st 2018, 00:00:00
new Date('2018 MARCH') //Mar 1st 2018, 00:00:00
new Date('2018 march') //Mar 1st 2018, 00:00:00
```

灵活性很大。你可以添加或省略月份和天的前导零。

要注意的是月份/天的位置，否则有可能把月份误解为天。

你也可以使用 `Date.parse`：

```javascript
Date.parse('2018-07-22')
Date.parse('2018-07') //July 1st 2018, 00:00:00
Date.parse('2018') //Jan 1st 2018, 00:00:00
Date.parse('07/22/2018')
Date.parse('2018/07/22')
Date.parse('2018/7/22')
Date.parse('July 22, 2018')
Date.parse('July 22, 2018 07:22:13')
Date.parse('2018-07-22 07:22:13')
Date.parse('2018-07-22T07:22:13')
```

`Date.parse` 会返回时间戳（毫秒），而不是返回 Date 对象。

你也可以传递一组有序的值，分别代表日期的每个部分：年份、月份（从 0 开始）、天、小时、分钟、秒及毫秒。

```javascript
new Date(2018, 6, 22, 7, 12, 13, 0);
new Date(2018, 6, 22);
```

最少应该有 3 个参数，但大多数 JavaScript 引擎接受小于 3 个参数：

```javascript
new Date(2018, 6);
new Date(2018);
```

在这些情况下，生成的日期是相对于你电脑时区的日期。这意味着**两台不同的电脑会从相同的 Date 对象得到不同的值**。

JavaScript 在没有任何时区信息的情况下，会把日期作为 UTC 对待。而且会自动执行转换为电脑当前时区的操作。

综上，你可以通过 4 种方式创建 Date 对象：

- **不传递参数**，代表“现在”
- 传递**数字**，代表自 1970 年 1 月 1 日到现在的毫秒数
- 传递**字符串**，代表一个日期
- 传递**一组参数**，代表日期的不同部分

## 时区

在初始化日期时可以传递时区，因此，日期不会假定为 UTC，而是转换为你当地时区。

你可以通过 +HOURS 这种格式指定时区，或者在括号中添加时区名称：

```javascript
new Date('July 22, 2018 07:22:13 +0700');
new Date('July 22, 2018 07:22:13 (CET)');
```

如果你在括号中指定了错误的时区名称，JavaScript 会毫不抱怨地默认为 UTC。

如果你指定了错误的数字格式，JavaScript 会出现“Invalid Date”错误。

## 日期转换和格式化

给出一个 Date 对象，有很多方法可以从该日期生成字符串：

```javascript
const date = new Date('July 22, 2018 07:22:13');

date.toString(); // "Sun Jul 22 2018 07:22:13 GMT+0800 (China Standard Time)"
date.toTimeString(); // "07:22:13 GMT+0800 (China Standard Time)"
date.toUTCString(); // "Sat, 21 Jul 2018 23:22:13 GMT"
date.toDateString(); // "Sun Jul 22 2018"
date.toISOString(); // "2018-07-21T23:22:13.000Z"
date.toLocaleString(); // "7/22/2018, 7:22:13 AM"
date.toLocaleTimeDtring(); // "7:22:13 AM"
date.toLocaleDateString(); // "7/22/2018"
date.getTime(); // 1532215333000
```

## Date 对象 getter 方法

Date 对象提供一些方法来检查它的值。这都依赖于电脑当前时区：

```javascript
const date = new Date('July 22, 2018 07:22:13');

date.getDate(); // 22
date.getDay(); // 0（0 代表星期日）
date.getFullYear(); // 2018
date.getMonth(); // 6（月份从 0 开始）
date.getHours(); // 7
date.getMinutes(); // 22
date.getSeconds(); // 13
date.getMilliseconds(); // 0（不准确）
date.getTime(); // 1532236933000
date.getTimezoneOffset(); // -480 -- 它返回以分钟表示的时差
```

这些方法由等价的 UTC 版本，它们会返回 UTC 值，而不是适合当前时区的值：

```javascript
date.getUTCDate(); // 22
date.getUTCDay(); // 0（0 代表星期日）
date.getUTCFullYear(); // 2018
date.getUTCMonth(); // 6（月份从 0 开始）
date.getUTCHours(); // 5
date.getUTCMinutes(); // 22
date.getUTCSeconds(); // 13
date.getUTCMilliseconds(); // 0（不准确）
```

## 修改日期

Date 对象也提供修改日期的方法：

```javascript
const date = new Date('July 22, 2018 07:22:13');

date.setDate(newValue);
date.setDay(newValue);
date.setFullYear(newValue); //注意：避免使用 setYear()，它已经废弃了
date.setMonth(newValue);
date.setHours(newValue);
date.setMinutes(newValue);
date.setSeconds(newValue);
date.setMilliseconds(newValue);
date.setTime(newValue);
date.setTimezoneOffset(newValue);
```

> `setDay` 和 `setMonth` 是从数字 0 开始的，例如三月是第 2 个月。

有趣的事实：这些方法“overlap”，例如，你设置 `date.setHours(48)`，它会自动增加一天。

你可以向 `setHours` 添加多个参数来设置分钟、秒和毫秒：`setHours(0, 0, 0, 0)`——同样适用于 `setMinutes` 和 `setSeconds`。

至于 get，set 方法也有等价的 UTC 版本：

```javascript
const date = new Date('July 22, 2018 07:22:13');

date.setUTCDate(newValue);
date.setUTCDay(newValue);
date.setUTCFullYear(newValue);
date.setUTCMonth(newValue);
date.setUTCHours(newValue);
date.setUTCMinutes(newValue);
date.setUTCSeconds(newValue);
date.setUTCMilliseconds(newValue);
```

## 获取当前时间戳

如果你想获取当前时间的毫秒时间戳，你可以使用简写：

```javascript
Date.now();
```

来代替：

```javascript
new Date().getTime();
```

## JavaScript 努力做到更好

注意。如果天数超过一个月，不会出现错误，而且日期会延伸到下一个月。

```javascript
new Date(2018, 6, 40); // Thu Aug 09 2018 00:00:00 GMT+0800 (China Standard Time)
```

同样适用于月份、小时、分钟、秒及毫秒。

## 根据区域格式化时间

国际化 API 已经被现代[浏览器支持][2]（例外：UC 浏览器），允许你翻译日期。

它通过 `Intl` 暴露出来，这也有助于数字、字符串和货币本地化。

我们现在要讨论 `Intl.DateTimeFormat()`。

以下是如何使用它。

根据电脑默认区域格式化日期：

```javascript
const date = new Date('July 22, 2018 07:22:13');
new Intl.DateTimeFormat().format(date); // 在我本地是："2018/7/22"
```

根据不同的区域格式化日期：

```javascript
new Intl.DateTimeFormat('en-US').format(date); // "7/22/2018"
```

`Intl,DateTimeFormat` 方法接受一个可选参数，允许你自定义输出内容。同时显示时分秒：

```javascript
const options = {
  year: 'numeric',
  month: 'numeric',
  day: 'numeric',
  hour: 'numeric',
  minute: 'numeric',
  second: 'numeric'
};

new Intl.DateTimeFormat('en-US', option).format(date);
// "7/22/2018, 7:22:13 AM"

new Intl.DateTimeFormat('zh-CN', option).format(date);
// "2018/7/22 上午7:22:13"
```

[这里有所有你可以使用的属性的参考][3]

## 比较日期

你可以使用 `Date.getTime()` 来比较两个不同的日期：

```javascript
const date1 = new Date('July 10, 2018 07:22:13');
const date2 = new Date('July 22, 2018 07:22:13');
const diff = date2.getTime() - date1.getTime(); // 1036800000
```

用同样的方式可以检查两个日期是否相等：

```javascript
const date1 = new Date('July 10, 2018 07:22:13');
const date2 = new Date('July 10, 2018 07:22:13');
if (date2.getTime() === date1.getTime()) {
  // 日期相等
}
```

要记住 `getTime()` 会返回毫秒数，所以在比较时你要考虑时间因素。`2018 年 7 月 10 日 07:22:13` 不等于 `2018 年 7 月 10 日`。在这种情况下，你可以使用 `setHours(0, 0, 0, 0)` 来重置时间。

[1]: https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/javascript-dates/1.png
[2]: https://caniuse.com/internationalization
[3]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat
