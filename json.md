# 为什么 JSON 不是一种好的配置语言
<!-- TOC -->

- [为什么 JSON 不是一种好的配置语言](#为什么-json-不是一种好的配置语言)
  - [为什么 JSON 作为配置语言很受欢迎？](#为什么-json-作为配置语言很受欢迎)
  - [JSON 的问题](#json-的问题)
    - [缺乏注释](#缺乏注释)
    - [过于严格](#过于严格)
    - [低信噪比](#低信噪比)
    - [长字符串](#长字符串)
    - [数字](#数字)
  - [你应该用什么代替](#你应该用什么代替)
    - [TOML](#toml)
    - [HJSON](#hjson)
    - [HOCON](#hocon)
    - [YAML](#yaml)
    - [脚本语言](#脚本语言)
    - [编写你自己的](#编写你自己的)
  - [结论](#结论)
  - [相关材料](#相关材料)

<!-- /TOC -->


许多项目使用 JSON 作为配置文件。也许最常见的例子就是 [npm](https://wwww.npmjs.com/) 和 [yarn](https://yarnpkg.com/lang/en/) 使用的 package.json 文件，但是还有很多其它的，包括 [CloudFormation](https://aws.amazon.com/cloudformation/)（起初只支持 JSON，现在也支持 YAML） 和 [composer](https://getcomposer.org/)（PHP）。

无论怎样，有几个原因让 JSON 成为非常糟糕的配置语言。别误会我——我喜欢 JSON。它是一种比较灵活的格式，对人和机器都比较易读，而且它也是非常棒的数据交换和存储格式。但是作为配置文件，它还不够好。

## 为什么 JSON 作为配置语言很受欢迎？

JSON 作为配置语言有几个原因。最大的原因可能是它容易实现。许多语言在标准库中都有 JSON 支持，and those that don’t almost certainly have an easy-to-use JSON package readily available. 其次，有个事实是，开发者和用户可能已经对 JSON 比较熟悉，不需要再去学习新的配置格式来用在产品上。而且更不用说现存的 JSON 工具，包括与发高亮、自动格式化、验证个工具等等。

这些实际上是非常好的理由。但是这种常见的格式不适合作为配置，而且非常糟糕。

## JSON 的问题

### 缺乏注释

对于配置文件必不可少的功能之一就是注释。注释是必要的，用来注释不同选项的用途，以及为什么我们会选择某个特定的值，也许最重要的是——在我们为测试和调试使用不同的配置时，临时注释掉部分配置。如果你认为 JSON 是数据交换格式，那么注释对它也就没有意义了。

当然，也可以变通方法为 JSON 添加注释。一个常见的替代方案是，在在对象中使用特殊的 key 作为注释，例如“//”或者“\_\_comment”。不管怎样，这种语法不太易读，而且为了在单个对象中包含多个注释，你就需要给每个注释一个唯一的 key。David Crockford（JSON 的发明者）[建议使用预处理器删除注释](https://plus.google.com/+DouglasCrockfordEsq/posts/RK8qyGVaGSr)。如果你使用的应用程序需要 JSON 作为配置文件，我建议你这样做，特别是对于在使用配置之前已经有构建步骤的情况。当然，编辑配置文件确实了一些添加额外的工作，所以如果你创建的应用程序需要解析配置文件，那就不要依靠用户能够做到这些。

一些 JSON 库允许输入注释。举个例子，Ruby 的 JSON 模块和 Java 的带有 JsonParser 的 Jackson 库。启用 ALLOW_COMMENTS 功能可以在输入中处理 JavaScript 风格的注释。无论怎样，这不是标准，而且许多编辑器无法正确处理 JSON 文件中的注释，这使得编辑 JSON 更加困难。

### 过于严格

JSON 规范是非常严格的。它的严格性是比较容易实现一个 JSON 解析器的部分原因，但在我看来，它也破坏了易读性，在一定程度上也破坏了人们的可写性。

### 低信噪比
向其他许多配置语言，JSON 非常嘈杂。有许多标点无法帮助人类阅读，尽管它确实使得为机器编写实现更加容易。特别是对于配置文件，对象中的 keys 几乎都是标识符，因此 keys 上的引号就是多余的。

此外，JSON 在整个文档外面要使用花括号，这使得它几乎成为 Javascript 的子集，有助于使用流发送多个对象是用以区分不同的对象。但是作为配置文件，最外面的括号是没有用的。键值对之间的逗号在配置文件中也是没有必要的。一般来说，每一行都有一个键值对，因此接受换行符作为分隔符是有意义的。

说到逗号，JSON 不接受尾随逗号。如果需要每个键值对后面的逗号，它应该至少接受尾随逗号。因为尾随逗号可以使添加新条目更加容易，并且可以获得更加清晰的提交差异。

### 长字符串

JSON 作为配置文件的另一个问题是，它不支持任何多行文本。如果想要在字符串中换行，你必须将其转义为“\n”，更糟的是，如果你想要把文件中的一个字符串延续到另一行，那你就倒霉了。如果你的配置文件中没有包含长字符串，这就不是问题。无论怎样，如果你的配置文件包含长字符串，例如项目描述或者是 GPG key，你可能不想使用“\n”转义来将其放到单独的一行，而是使用真正的换行。

### 数字

此外，JSON 对数字的定义在有些情况下可能是有问题的。正如 JSON 规范中定义的，数字是十进制表示法中的任意精度有限浮点数。在许多应用程序中，这是很好的。但是如果你要使用十六进制，或者要描述无穷和 NaN 这样的值，那么 TOML 或者 YAML 可能更适合处理这种输入。

```json
{
  "name": "example",
  "description": "A really long description that needs multiple lines.\nThis is a sample project to illustrate why JSON is not a good configuration format. This description is pretty long, but it doesn't have any way to go onto multiple lines.",
  "version": "0.0.1",
  "main": "index.js",
  "//": "This is as close to a comment as you are going to get",
  "keywords": ["example", "config"],
  "scripts": {
    "test": "./test.sh",
    "do_stuff": "./do_stuff.sh"
  },
  "bugs": {
    "url": "https://example.com/bugs"
  },
  "contributors": [{
    "name": "John Doe",
    "email": "johndoe@example.com"
  }, {
    "name": "Ivy Lane",
    "url": "https://example.com/ivylane"
  }],
  "dependencies": {
    "dep1": "^1.0.0",
    "dep2": "3.40",
    "dep3": "6.7"
  }
}
```

## 你应该用什么代替

选择配置语言依赖于你的应用程序。每种语言都有不同的优点和缺点，但是有一些选择需要考虑。它们都是优先为配置设计的语言，而且对于 JSON 这种数据语言是更好的选择。

### TOML

[TOML](https://github.com/toml-lang/toml) 是越来越受欢迎的配置语言，Cargo（Rust 构建工具）、pip（Python 包管理）及 dep（golang 依赖管理）都使用了它。TOML 有些类似于 INI 格式，但是不同于 INI，它有一套标准规范和为嵌套结构定义地良好的语法。它比 YAML 简单很多，如果你的配置简单，TOML 就很有吸引力。但是如果你的配置如果有大量的嵌套结构，TOML 可能会有点冗长，而且其他格式，如 YAML 或者 HOCON 或许是更好的选择。

```toml
name = "example"
description = """
A really long description that needs multiple lines.
This is a sample project to illustrate why JSON is not a \
good configuration format. This description is pretty long, \
but it doesn't have any way to go onto multiple lines."""

version = "0.0.1"
main = "index.js"
# This is a comment
keywords = ["example", "config"]

[bugs]
url = "https://example.com/bugs"

[scripts]

test = "./test.sh"
do_stuff = "./do_stuff.sh"

[[contributors]]
name = "John Doe"
email = "johndow@example.com"

[[contributors]]
name = "Ivy Lane"
url = "https://example.com/ivylane"

[dependencies]

dep1 = "^1.0.0"
# Why we depend on dep2
dep2 = "3.40"
dep3 = "6.7"
```

### HJSON

[HJSON(https://hjson.org/) 基于 JSON，但是它更具灵活性且更加易读。它添加了对注释、多行字符串、没有引号的键和字符串及可选逗号的支持。如果你想要 JSON 的简单结构，但是对配置文件更加友好，HJSON 可能是一条路。有命令行工具可以把 HJSON 转换为 JSON，所以如果你使用的工具需要普通的 JSON，你可以用 HJSON 编写配置，然后使用构建步骤将其转换为 JSON。[JSON5](https://json5.org/) 是另一个比 HJSON 更加简洁的方案。

```hjson
{
  name: example
  description: '''
  A really long description that needs multiple lines.
  
  This is a sample project to illustrate why JSON is 
  not a good configuration format.  This description 
  is pretty long, but it doesn't have any way to go 
  onto multiple lines.
  '''
  version: 0.0.1
  main: index.js
  # This is a a comment
  keywords: ["example", "config"]
  scripts: {
    test: ./test.sh
    do_stuff: ./do_stuff.sh
  }
  bugs: {
    url: https://example.com/bugs
  }
  contributors: [{
    name: John Doe
    email: johndoe@example.com
  } {
    name: Ivy Lane
    url: https://example.com/ivylane
  }]
  dependencies: {
    dep1: ^1.0.0
    # Why we have this dependency
    dep2: "3.40"
    dep3: "6.7"
  }
}
```

### HOCON

[HOCON](https://github.com/lightbend/config/blob/master/HOCON.md) 是为 [Play](https://www.playframework.com/) 框架设计的配置语言，但是在 Scala 项目中相当流行。它是 JSON 的超集，所以现有的 JSON 文件也可以利用。除了注释、多行字符串、没有引号的键和字符串及可选逗号这些标准功能，HOCON 还支持从其他文件导入，引用其它值的其它键以避免代码重复，并且使用点分隔键来指定值的路径，这样用户就不必把所有值都放在大括号对象中。

```hocon
name = example
description = """
A really long description that needs multiple lines.

This is a sample project to illustrate why JSON is 
not a good configuration format.  This description 
is pretty long, but it doesn't have any way to go 
onto multiple lines.
"""
version = 0.0.1
main = index.js
# This is a a comment
keywords = ["example", "config"]
scripts {
  test = ./test.sh
  do_stuff = ./do_stuff.sh
}
bugs.url = "https://example.com/bugs"
contributors = [
  {
    name = John Doe
    email = johndoe@example.com
  }
  {
    name = Ivy Lane
    url = "https://example.com/ivylane"
  }
]
dependencies {
  dep1 = ^1.0.0
  # Why we have this dependency
  dep2 = "3.40"
  dep3 = "6.7"
}
```

### YAML

[YAML](http://yaml.org/)（YAML 不是标记预览）是一个非常灵活的格式，它几乎是 JSON 的超集，它被用在几个引人注目的项目中，如 Travis CI、Circle CI 以及 AWS CloudFormation。YAML 的库几乎和 JSON 一样无处不在。除了支持注释、新行定界符、多行字符串、空字符串和更灵活的类型系统之外，YAML 允许你引入文件中的早期结构以避免代码重复。

YAML 的主要缺点是规范非常复杂，这导致不同实现之间不一致。它认为缩进级别中语法比较重要（类似于 Python），一些人喜欢，另一些人则不然。它使得复制和粘贴比较困难。查看 [YAML: 或许根本没有那么好](https://arp242.net/weblog/yaml_probably_not_so_great_after_all.html)来获取更多使用 YAML 时的缺点。

```yaml
name: example
description: >
  A really long description that needs multiple lines.
  
  This is a sample project to illustrate why JSON is not a good 
  configuration format. This description is pretty long, but it 
  doesn't have any way to go onto multiple lines.
version: 0.0.1
main: index.js
# this is a comment
keywords:
  - example
  - config
scripts: 
  test: ./test.sh
  do_stuff: ./do_stuff.sh
bugs: 
  url: "https://example.com/bugs"
contributors:
  - name: John Doe
    email: johndoe@example.com
  - name: Ivy Lane
    url: "https://example.com/ivylange"
dependencies:
  dep1: ^1.0.0
  # Why we depend on dep2
  dep2: "3.40"
  dep3: "6.7"
```

### 脚本语言

如果你的应用程序是用 Python 或 Ruby 这样的脚本语言编写的，并且你知道配置来自于一个可信任的源，最简单的方式可能就是为你的配置文件使用你编写应用程序的语言。如果你需要更灵活的配置项，你也可以在汇编语言中嵌入 Lua 这样的脚本语言。这样的做法给予你脚本语言的全部灵活性，而且比使用不同的配置语言更易实现。使用脚本语言的缺点可能在于：过于强大，当然，如果配置的源是不受信任的，那将会导致严重的安全问题。

### 编写你自己的

如果出于某些原因，键值对的配置格式不符合你的需求，并且由于性能和大小限制，那么编写你自己的配置格式就有必要了。但是如果你发现自己处于这种情况，在做决定之前要思考不仅是你需要编写和维护一个解析器，而且要求你的用户要熟悉另一种格式的配置语言。

## 结论

有这些更好的选择作为配置语言，那也就没有充分的原因要使用 JSON。如果你正在创建新的应用程序、框架或者库，请选择 JSON 之外的配置语言。

## 相关材料

- [JSON as configuration files: please don’t](https://arp242.net/weblog/json_as_configuration_files-_please_dont)
- [Don’t Use JSON as a Configuration File Format. (Unless You Have To…)](https://revelry.co/json-configuration-file-format/)
