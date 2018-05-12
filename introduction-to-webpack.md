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

### 全局安装

如何使用 [Yarn](https://flaviocopes.com/yarn/) 安装：

```shell
yarn global add webpack webpack-cli
```

使用 [npm](https://flaviocopes.com/npm/) ：

```shell
npm i -g webpack webpack-cli
```

安装完成后，你应该能够运行：

```shell
webpack-cli
```

![webpack-cli](./introduction-to-webpack/1.png)

### 本地安装

Webpack 也可以安装在本地。这也是推荐的操作，因为 Webpack 可以根据项目进行更新

使用 Yarn ：

```shell
yarn add webpack webpack-cli -D
```

使用 npm ：

```shell
npm i webpack webpack-cli --save-dev
```

完成操作后，将其添加到 `package.json` 文件中：

```json
{
  //...
  "scripts": {
    "build": "webpack"
  }
}
```

添加完成后，你可以在项目根目录下通过键入下面的命令来运行 webpack ：

```shell
yarn build
```

## Webpack 配置

如果你遵循以下约定，webpack（从 4.0 版本开始）默认是不需要任何配置的：

- 程序的入口文件是 `./src/index.js`
- 输出文件放在 `./dist/main.js`
- Webpack 以生产模式工作

如果有需要，你可以自定义 webpack 的每一个部分。webpack 的配置存放在项目根目录的 `webpack.config.js` 文件中。

## 入口点

默认情况下的入口点是 `./src/index.js` 。这个简单的示例使用 `./index.js` 文件作为入口：

```javascript
module.exports = {
  //...
  entry: './index.js'
  //...
}
```

## 输出

默认情况下输出文件会被生成在 `./dist/main.js`。这个简单的示例将输出文件放在 `./dist/app.js` ：

```javascript
module.exports = {
  //...
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'app.js'
  }
  //...
}
```

## 加载器

webpack 允许你在 JavaScript 代码中使用 `import` 或者 `require` 语句来导入不仅包括 JavaScript ，而且还有其它各种文件，例如 CSS 。

Webpack 的目标在于帮助我们处理所有依赖，而不仅仅是 JavaScript ，加载器正是实现这一目标的方式之一。

举个例子，你可以在代码中使用：

```javascript
import 'style.css'
```

通过使用这个加载器配置：

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: 'css-loader'
      }
    ]
  }
  //...
}
```

这个[正则表达式](https://flaviocopes.com/javascript-regular-expressions/)匹配任意 CSS 文件。

一个加载器可以有以下配置：

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
  //...
}
```

每条规则可以有多个加载器：

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.css$/,
        use:
          [
            'style-loader',
            'css-loader',
          ]
      }
    ]
  }
  //...
}
```

在这个示例中，`css-loader` 负责解释 CSS 中的 `import 'style.css'` 指令。然后 `style-loader` 负责使用 `<style>` 标签将该 CSS 注入到 DOM 中。

顺序很重要，并且它是颠倒的（最后的最先执行）。

有什么样的加载器？很多！

[你可以在这里找到完整的列表](https://webpack.js.org/loaders/)。

一个很常用的加载器是 [Babel](https://flaviocopes.com/babel/)，它用于将现代 JavaScript 语法转译为 ES5 代码：

```javascript
module.exports {
  //...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  }
  //...
}
```

这个示例使用 Babel 预处理我们所有的 React/JSX 文件：

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  }
  //...
}
```

[babel-loader 配置参见这里](https://webpack.js.org/loaders/babel-loader/)。

## 插件
