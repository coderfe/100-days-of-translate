# Javascript 正则表达式指南

> 通过这篇简短的指南，你可以了解所有关于 Javascript 正则表达式的信息，这篇指南总结了最重要的概念，并且用示例展示了这些概念。

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

## 很难但有用
