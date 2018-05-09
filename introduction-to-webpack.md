# Webpack 简介

> Webpack 使一个在过去几年中备受关注的工具，几乎在每个项目中都可以看到它。我们一起来了解一下。

## 什么是 Webpack

Webpack 是一个用于编译 JavaScript 的工具，也可以称之为模块打包器。

给它大量文件，它能生成一个文件（或几个文件）来运行的的程序。

它可以执行许多操作：

  - 帮助你打包资源
  - 监听改动并重新运行任务
  - 可以运行 Babel 转译到 ES5 ，允许你使用 [JavaScript](https://flaviocopes.com/javascript/) 最新的功能而不用担心浏览器的支持情况
  - 可以把 CoffeeScript 转译为 JavaScript
  - 可以把内联图片转换为 data URIs
  - 允许你在 CSS 文件中使用 require()
  - 可以运行开发服务器
  - 可以处理热模块替换
  - 可以将输出文件分拆为多个文件，避免在渲染首屏时加载大量的 js 文件
  - 可以处理 [tree shaking](https://flaviocopes.com/javascript-glossary/#tree-shaking)

Webpack 不限于在前端使用，在开发后端 Node.js 时也很有用。

Webpack 的前身，并且仍在被广泛使用的工具包括：

  - Grunt
  - Broccoli
  - Gulp

这些工具都和 Webpack 有许多相似之处，不同之处在于它们以**任务运行**出名，而 Webpack 则是一个模块打包器。

Webpack 是一个更具针对性的工具：你只需指定一个程序的入口文件（它甚至可以是一个带有 script 标签的 HTML 文件），Webpack 会分析文件并将其打包为一个用来运行你的程序的 JavaScript 文件。

## 安装 Webpack
