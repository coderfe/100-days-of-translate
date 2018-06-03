# 学习如何使用 Vue.js CLI

> Vue 是一个非常令人印象深刻的项目，除了框架的核心之外，它还维护着许多能够使 Vue 开发者生活更加轻松的实用工具。其中之一就是 Vue CLI 。

<!-- TOC -->

- [学习如何使用 Vue.js CLI](#学习如何使用-vuejs-cli)
  - [安装](#安装)
  - [Vue CLI 提供了什么？](#vue-cli-提供了什么)
  - [如何使用 CLI 创建 Vue 项目？](#如何使用-cli-创建-vue-项目)
  - [如何启动新创建的 Vue CLI 应用程序？](#如何启动新创建的-vue-cli-应用程序)
  - [Git 仓库](#git-仓库)

<!-- /TOC -->

Vue 是一个非常令人印象深刻的项目，除了框架的核心之外，它还维护着许多能够使 Vue 开发者生活更加轻松的实用工具。

其中之一就是 Vue CLI 。

CLI 代表着命令行界面。

> 注意：目前 CLI 正在进行大量的重构工作，版本将从 2.0 升级到 3.0 。现在还不太稳定，我将介绍 3.0 版本，因为这是对 2.0 版本的巨大改进，而且大有不同。

## 安装

Vue CLI 是一个命令行实用程序，你可以使用 npm 全局安装：

```shell
npm install -g @vue/cli
```

或者使用 Yarn ：

```shell
yarn global add @vue/cli
```

安装完成后，你就可以运行 `vue` 命令了：

![vue](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/1.png)

## Vue CLI 提供了什么？

Vue CLI 对于快速开发是必不可少的。

它的主要目标是确保你需要的所有工具都在一起工作，来处理你的需求，并且抽象出独立使用每个工具时所需要的配置细节。

它可以执行项目初始化设置和脚手架。

它是一个灵活的工具：一旦你使用 CLI 创建项目，你可以去调整配置，而不必 eject 应用程序（就像你是用 `create-react-app` 那样）。你可以配置任何东西，也可以轻松升级。

当你创建并配置完程序，它将作为一个构建在 webpack 之上的运行时依赖工具。

与 Vue CLI 的首次相遇时创建 Vue 项目。

## 如何使用 CLI 创建 Vue 项目？

你用 Vue CLI 做的第一件事就是创建 Vue 程序：

```shell
vue create example
```

比较酷的在于这是一个可交互的过程。你需要选择 preset ，默认情况下，它会提供一个集成了 Babel 和 ESLint 的 preset 。

![vue-create-example](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/2.png)

我继续按下了 ⬇️ 箭头并手动选择了我需要的功能：

![arrow-down](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/3.png)

按下 `space` 可以启用你需要的一项功能，然后按下 `enter` 继续。由于我选择了 linter/formatter ，Vue CLI 会提示我进行配置。我选择了 ESLint + Prettier ，因为那是我最喜欢的设置。

![linting](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/4.png)

下一项是选择如何使用 linting ，我选择了保存时 lint 。

![lint-on-save](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/5.png)

下一步：测试。我选择了测试，而且 Vue CLI 给我提供了两个最流行的解决方案：Mocha + Chai vs Jest 。

![testing](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/6.png)

Vue CLI 询问我将所有配置放在哪里：是放在 `package.json` 文件，或者放在每个工具一个的专用的配置文件中。我选择后者。

![place-config](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/7.png)

接着，Vue CLI 会询问我是否保存这些预置选项，并且允许我下次创建 Vue 项目时可以选择。这是一个非常方便的功能，因为快速设置我的偏好可以缓解复杂性。

![save-favourite](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/8.png)

然后，Vue CLI 会询问我喜欢使用 npm 还是 Yarn ：

![npm-vs-yarn](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/9.png)

这是最后一次询问我。然后它会接着下载依赖并创建 Vue 程序：

![install-dependencies](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/10.png)

## 如何启动新创建的 Vue CLI 应用程序？

Vue CLI 已经为我们创建了程序，现在我们可以进入到 `example` 文件夹里，然后运行 `yarn serve` 来启动我们的第一个应用程序。

![app](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/11.png)

示例程序包含了几个源文件，包括 `package.json` ：

![app](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/12.png)

这里定义了所有的 CLI 命令，包括之前我们用过的 `yarn serve` ，其它命令有：

- `yarn build` 启动生产构建
- `yarn lint` 运行 linter
- `yarn test:unit` 运行单元测试

我将在单独的教程中介绍 Vue CLI 生成的示例程序。

## Git 仓库
