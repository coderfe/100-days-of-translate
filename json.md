# 为什么 JSON 不是一种好的配置语言

许多项目使用 JSON 作为配置文件。也许最常见的例子就是 [npm](https://wwww.npmjs.com/) 和 [yarn](https://yarnpkg.com/lang/en/) 使用的 package.json 文件，但是还有很多其它的，包括 [CloudFormation](https://aws.amazon.com/cloudformation/)（起初只支持 JSON，现在也支持 YAML） 和 [composer](https://getcomposer.org/)（PHP）。

无论怎样，有几个原因让 JSON 成为非常糟糕的配置语言。别误会我——我喜欢 JSON。它是一种比较灵活的格式，对人和机器都比较易读，而且它也是非常棒的数据交换和存储格式。但是作为配置文件，它还不够好。

## 为什么 JSON 作为配置语言很受欢迎？

JSON 作为配置语言有几个原因。最大的原因可能是它容易实现。许多语言在标准库中都有 JSON 支持，and those that don’t almost certainly have an easy-to-use JSON package readily available. 其次，有个事实是，开发者和用户可能已经对 JSON 比较熟悉，不需要再去学习新的配置格式来用在产品上。而且更不用说现存的 JSON 工具，包括与发高亮、自动格式化、验证个工具等等。

这些实际上是非常好的理由。但是这种常见的格式不适合作为配置，而且非常糟糕。

### JSON 的问题

#### 缺乏注释

对于配置文件必不可少的功能之一就是注释。注释是必要的，用来注释不同选项的用途，以及为什么我们会选择某个特定的值，也许最重要的是——在我们为测试和调试使用不同的配置时，临时注释掉部分配置。如果你认为 JSON 是数据交换格式，那么注释对它也就没有意义了。

当然，也可以变通方法为 JSON 添加注释。一个常见的替代方案是，在在对象中使用特殊的 key 作为注释，例如“//”或者“\_\_comment”。不管怎样，这种语法不太易读，而且为了在单个对象中包含多个注释，你就需要给每个注释一个唯一的 key。David Crockford（JSON 的发明者）[建议使用预处理器删除注释](https://plus.google.com/+DouglasCrockfordEsq/posts/RK8qyGVaGSr)。如果你使用的应用程序需要 JSON 作为配置文件，我建议你这样做，特别是对于在使用配置之前已经有构建步骤的情况。当然，编辑配置文件确实了一些添加额外的工作，所以如果你创建的应用程序需要解析配置文件，那就不要依靠用户能够做到这些。

一些 JSON 库允许输入注释。举个例子，Ruby 的 JSON 模块和 Java 的带有 JsonParser 的 Jackson 库。启用 ALLOW_COMMENTS 功能可以在输入中处理 JavaScript 风格的注释。无论怎样，这不是标准，而且许多编辑器无法正确处理 JSON 文件中的注释，这使得编辑 JSON 更加困难。
