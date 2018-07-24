# Create React App

创建没有构建配置的 React 应用程序。

- [创建应用程序](https://github.com/facebook/create-react-app/blob/next/README.md#creating-an-app)——如何创建新的应用程序。
- [用户指南](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md)——如何使用 Create React App 创建应用程序。

Create React App 可以运行在 macOS、Windows 及 Linux 上。

如果有问题，请[提 issue](https://github.com/facebook/create-react-app/issues/new)。

## 概览

```shell
npx create-react-app my-app
cd my-app
yarn start
```

_（npx 需要 npm 5.2+ 或者更高版本，参见 [npm 的旧版本](https://gist.github.com/gaearon/4064d3c23a77c74a3614c498a8bb1c5f)）_

然后打开 [http://localhost:3000](http://localhost:3000) 查看你的程序。

当你准备部署在生产上时，可以运行 `yarn build` 来打包。

<p style="text-align: center">
<img src="https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/create-react-app/1.svg" alt="yarn-build" />
</p>

### 立即开始

你不需要安装或配置想 Webpack 和 Babel 这样的工具。

它们已经预先配置好并隐藏了，所以你只需关注于写代码。

只需要创建一个项目，你就可以开始了。

## 创建应用程序

在你本地开发机器上需要 Node >= 6 的版本（但是在服务器上不是必须的）。你可以使用 [nvm](https://github.com/creationix/nvm#installation)（macOS/Linux）或者 [nvm-windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 轻松地在不同的项目之间切换 Node 版本。

要创建应用程序，你可以选择下面的任意一种方法：

### npx

```shell
npx create-react-app my-app
```

_（npx 需要 npm 5.2+ 或者更高版本，参见 [npm 的旧版本](https://gist.github.com/gaearon/4064d3c23a77c74a3614c498a8bb1c5f)）_

### npm

```shell
npm init react-app my-app
```

`npm init <initializer>` 在 npm 6+ 上可用。

### Yarn

```shell
yarn init react-app my-app
```

`yarn create` 在 Yarn 0.25+ 上可用。

它将会在当前文件夹下创建 `my-app` 目录。

在这这目录下，它会生成初始项目结构并且安装依赖项：

```plain
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── registerServiceWorker.js
```

没有配置或者复杂的问价结构，只有你构建应用程序所需的文件。

一旦安装完成，你就可以打开项目文件夹：

```shell
cd my-app
```

在新创建的项目中，你可以运行一些内建的命令。

### `npm start` 或者 `yarn start`

在开发模式中运行程序。

打开 [http://localhost:3000](http://localhost:3000) 在浏览器中查看。

如果你改动了代码，页面就会自动重载。你可以在控制台看到构建错误和校验警告。

<p style="text-align: center">
<img src="https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/create-react-app/2.svg" alt="yarn-start" />
</p>

### `npm test` 或者 `yarn test`

在可交互模式中运行测试。

默认情况下，只运行与上次提交文件相关的测试。

[参阅更多测试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)。

### `npm run build` 或者 `yarn build`

将生产环境的应用程序构建在 `build` 文件夹中。

它以最佳方法将 React 打包到生产环境，并且优化构建以获得最佳性能。

构建被最小化，并且文件名包含了哈希值。

默认情况下，它也包含了 [service worker](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#making-a-progressive-web-app)，所以以后访问你的应用程序时可以从本地缓存加载。

你的应用程序已经准备好部署了。

## 用户指南

这份[用户指南](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md)包含了不同方面的话题，如下：

- [更新至新版本](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#updating-to-new-releases)
- [文件夹结构](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#folder-structure)
- [可用脚本](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#available-scripts)
- [支持的浏览器](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#supported-browsers)
- [支持的语言功能和 Polyfills](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#supported-language-features-and-polyfills)
- [编辑器的语法高亮](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#syntax-highlighting-in-the-editor)
- [编辑器中显示 Lint 输出](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#displaying-lint-output-in-the-editor)
- [自动格式化代码](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#formatting-code-automatically)
- [在编辑器中调试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#debugging-in-the-editor)
- [改变页面 `<title>`](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#changing-the-page-title)
- [安装依赖](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#installing-a-dependency)
- [导入组件](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#importing-a-component)
- [代码分割](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#code-splitting)
- [添加样式表](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-stylesheet)
- [后处理 CSS](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#post-processing-css)
- [添加 CSS 预处理（Sass 或者 Less 等）](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-css-preprocessor-sass-less-etc)
- [添加图像、字体和文件](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-images-fonts-and-files)
- [使用 `public` 文件夹](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#using-the-public-folder)
- [使用全局变量](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#using-global-variables)
- [添加 Bootstrap](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-bootstrap)
- [添加 Flow](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-flow)
- [添加路由](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-router)
- [添加自定义环境变量](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-custom-environment-variables)
- [我可以使用装饰器吗？](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#can-i-use-decorators)
- [使用 Ajax 获取数据](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#fetching-data-with-ajax-requests)
- [继承后端 API](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#integrating-with-an-api-backend)
- [开发环境使用代理 API 请求](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#proxying-api-requests-in-development)
- [开发环境中使用 HTTPS](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#using-https-in-development)
- [在服务端生成动态 `<meta>` 标签](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#generating-dynamic-meta-tags-on-the-server)
- [预渲染静态 HTML 文件](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#pre-rendering-into-static-html-files)
- [运行测试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)
- [调试测试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#debugging-tests)
- [隔离开发组件](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#developing-components-in-isolation)
- [把组件发布到 npm](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#publishing-components-to-npm)
- [制作渐进式网页应用](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#making-a-progressive-web-app)
- [分析打包大小](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#analyzing-the-bundle-size)
- [部署](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#deployment)
- [高级配置](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#advanced-configuration)
- [排除故障](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#troubleshooting)

用户指南会被作为 `README.md` 创建在项目文件夹下。

## 如何升级到新版本？

请参阅[用户指南](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#updating-to-new-releases)。

## Philosophy

- **一个依赖**：只有一个构建依赖。它使用了 Webpack、Babel、Eslint 和其他优秀的项目，但是在它们之上提供了更加一致的体验。
- **零配置**：你需要配置任何东西。开发和生产的构建已经为你合理地配置了，因此，你只需专注于编写代码。
- **没有锁定**：你随时可以“eject”为自定义设置。只要运行一个命令，所有的配置和构建依赖都会直接移动到你的项目文件夹下。

## 包含什么？

你的环境将包含你需要构建一个现代单页 React 应用程序的所有东西：

- React、JSX、ES6 和 Flow 语法支持
- ES6 语言意外的附加功能，如对象扩展运算符
- Autoprefixed CSS，你不需要 `-webkit` 或者其他前缀
- 一个快速可交互式的单元测试运行器，并且内置支持覆盖率报告
- 能够提醒常见开发错误的实时开发服务器
- 使用哈希值和 sourcemaps 来为生产打包 JS、CSS 及 images 的构建脚本
- 一个离线优先的 [service worker](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers) 和 一个 [web app manifest]，满足所有[渐进式网页应用](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#making-a-progressive-web-app)的标准
- 使用单独的依赖可以轻松更新上述工具

查看[这个指南](https://github.com/nitishdayal/cra_closer_look)来了解这些工具如何组合在一起工作。

权衡是**这些工具被预先配置以一种特定的方式工作**。如果你的项目需要高度自定义化，你可以“eject”并且自定义，但是之后你需要维护这些配置。

## 流行的替代方案

Create React App 非常适合用来：

- **学习 React**：舒适而且功能丰富的开发环境
- **开始一些新的单页 React 应用程序**
- 为你的 React 库和组件**创建一些示例**

这里是一些常见的情况，你可能想尝试一些别的东西：

- 如果你想在没有数百个构建依赖项的情况下**试用 React**，考虑[使用单个 HTML文件或者在线 sandbox 来代替](https://reactjs.org/docs/try-react.html)。
- 如果你**需要把 React 代码集成到像 Rails 或者 Django 这样的服务端模板框架中**，或者你不是在**构建单页应用**，考虑使用 [nwb](https://github.com/insin/nwb)，或 [Neutrino](https://neutrinojs.org/) 更加适用。特别对于 Rails，你可以使用 [Rails Webpacker](https://github.com/rails/webpacker)。
- 如果你需要**发布 React 组件**，[nwb](https://github.com/insin/nwb) 也[可以做到](https://github.com/insin/nwb#react-components-and-libraries)，[Neutrino's react-conponents preset](https://neutrino.js.org/packages/react-components/) 同样也可以。
- 如果你想使用 React 和 Node.js 做**服务端渲染**，考虑 [Next.js](https://github.com/zeit/next.js) 或者 [Razzle](https://github.com/jaredpalmer/razzle)。Create React App 不考虑后端，只处理静态的 HTML/CSS/JS。
- 如果你想让网站**静态化**（例如博客），可以考虑使用 [Gatsby](https://www.gatsbyjs.org/) 代替。不像 Create React App，它在构建时会把网站预渲染为 HTML。
- 如果你想使用 **TypeScript**，考虑使用 [create-react-app-typescript](https://github.com/wmonk/create-react-app-typescript)。
- 如果你想用 **Parcel** 代替 **Webpack** 作为你的打包器，考虑使用 [create-react-app-parcel](https://github.com/sw-yx/create-react-app-parcel)。
- 最后如果你想**更加定制化**，查看 [Neutrino](https://neutrinojs.org/) 和它的 [React preset](https://neutrino.js.org/packages/react/)。

上面的工具都可以在零配置或者很少配置的情况下工作。

如果你喜欢构建自己的配置，[参考这个指南](https://reactjs.org/docs/add-react-to-a-website.html)。

## 贡献

我们希望你能和我们一起开发 `create-react-app`！关于我们在寻找什么以及如何开始等更多信息，可以参阅 [CONTRIBUTING.md](https://github.com/facebook/create-react-app/blob/next/CONTRIBUTING.md)。

## React Native

想要为 React Native 寻找类似的工具？

查看 [Create React Native App](https://github.com/react-community/create-react-native-app/)。

## 致谢

我们非常感激现有关联项目作者的想法和合作：

- [@eanplatter](https://github.com/eanplatter)
- [@insin](https://github.com/insin)
- [@mxstbr](https://github.com/mxstbr)

## License

Create React App 是以 MIT 授权的开源软件。
