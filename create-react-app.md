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

*（npx 需要 npm 5.2+ 或者更高版本，参见 [npm 的旧版本](https://gist.github.com/gaearon/4064d3c23a77c74a3614c498a8bb1c5f)）*

然后打开 [http://localhost:3000](http://localhost:3000) 查看你的程序。

当你准备部署在生产上时，可以运行 `yarn build` 来打包。

![create-react-app](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/create-react-app/1.svg)

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

*（npx 需要 npm 5.2+ 或者更高版本，参见 [npm 的旧版本](https://gist.github.com/gaearon/4064d3c23a77c74a3614c498a8bb1c5f)）*

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

![errors](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/create-react-app/2.svg)

### `npm test` 或者 `yarn test`

在可交互模式中运行测试。

默认情况下，只运行与上次提交文件相关的测试。

[参阅更多测试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)。

### `npm run build` 或者 `yarn build`


