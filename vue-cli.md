# 学习如何使用 Vue.js CLI

> Vue 是一个非常令人印象深刻的项目，除了框架的核心之外，它还维护着许多能够使 Vue 开发者生活更加轻松的实用工具。其中之一就是 Vue CLI 。

<!-- TOC -->

- [学习如何使用 Vue.js CLI](#学习如何使用-vuejs-cli)
  - [安装](#安装)
  - [Vue CLI 提供了什么？](#vue-cli-提供了什么)
  - [如何使用 CLI 创建 Vue 项目？](#如何使用-cli-创建-vue-项目)
  - [如何启动新创建的 Vue CLI 应用程序？](#如何启动新创建的-vue-cli-应用程序)
  - [Git 仓库](#git-仓库)
  - [在命令行中使用预设](#在命令行中使用预设)
  - [预设存储在哪里？](#预设存储在哪里)
  - [插件](#插件)
  - [远程存储预设](#远程存储预设)
  - [Vue CLI 的另一种用法：快速制作原型](#vue-cli-的另一种用法快速制作原型)
  - [Webpack](#webpack)

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

比较酷的在于这是一个可交互的过程。你需要选择预设 ，默认情况下，它会提供一个集成了 Babel 和 ESLint 的预设。

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

注意到 VS Code 左下角的 `master` 单词了么？那是因为 Vue CLI 自动创建了仓库，并进行了第一次提交。所以我么可以直接进来，改动一些东西，我们知道我们改动了什么。

![git-log](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/13.png)

这是很酷的工具。有多少次你在项目中改动一些代码，直到你想要提交，你才意识到没有进行初始化提交。

## 在命令行中使用预设

你可以跳过可交互式面板，并让 Vue CLI 使用指定的预设：

```shell
vue create -p favourite example-2
```

## 预设存储在哪里？

预设存储在 home 目录下的 `.vuerc` 文件中。这是我的创建的第一个 “favourite” 预设：

```javascript
{
  "useTaobaoRegistry": true,
  "packageManager": "yarn",
  "presets": {
    "favourite": {
      "useConfigFiles": true,
      "plugins": {
        "@vue/cli-plugin-babel": {},
        "@vue/cli-plugin-eslint": {
          "config": "prettier",
          "lintOn": [
            "save"
          ]
        },
        "@vue/cli-plugin-unit-jest": {}
      },
      "router": true,
      "vuex": true
    }
  }
}
```

## 插件

通过阅读配置可以看出来，预设基本是插件的集合以及一些可选配置项。

创建一个项目后，你可以通过使用 `yarn add` 来添加更多的插件。

```shell
vue add @vue/cli-plugin-babel
```

这些所有的插件都使用最新版本。你可以通过为其指定一个版本号来使用特定的版本：

```javascript
"@vue/cli-plugin-eslint": {
  "version": "^3.0.0"
}
```

如果一个版本出现 bug 或者破坏性升级，并且需要你等待一段时候后才能使用，这将非常有用。

## 远程存储预设

通过创建一个包含 `preset.json` 文件，其中包含单个预设配置，这样它就可以存储在 Github （或者其他服务）。我制作了一个包含这些配置的预设[https://github.com/flaviocopes/vue-cli-preset/blob/master/preset.json](https://github.com/flaviocopes/vue-cli-preset/blob/master/preset.json) ：

```json
{
  "useConfigFiles": true,
  "plugins": {
    "@vue/cli-plugin-babel": {},
    "@vue/cli-plugin-eslint": {
      "config": "prettier",
      "lintOn": [
        "save"
      ]
    },
    "@vue/cli-plugin-unit-jest": {}
  },
  "router": true,
  "vuex": true
}
```

它可以用来创建新的应用程序：

```shell
vue create --preset flaviocopes/vue-cli-preset example3
```

## Vue CLI 的另一种用法：快速制作原型

到现在为止，我已经介绍了如何使用 Vue CLI 从头创建一个项目。但是对于正真的快速原型制作，你可以创建一个正真简单的 Vue 程序，甚至是一个独立的 .vue 文件来提供服务，而不用将所有依赖都下载到 `node_modules` 文件夹。

怎么做？首先全局安装 `cli-service-global` ：

```shell
npm install -g @vue/cli-service-global

//or

yarn global add @vue/cli-service-global
```

创建 app.vue 文件：

```html
<template>
    <div>
        <h2>Hello world!</h2>
        <marquee>Heyyy</marquee>
    </div>
</template>
```

然后运行：

```shell
vue serve app.vue
```

![vue-serve-app](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/14.png)

你也可以为由 Javascript 和 HTML 文件组成的更有组织的项目提供服务。Vue CLI 默认使用 main.js/index.js 作为入口点，而且你也可以有一个 package.json 文件和其它任何工具的初始化配置。`vue serve` 会选择它。

因为这样使用的是全局依赖，除了演示和测试之外，这不是一个最佳解决方案。

运行 `vue build` 将会为部署 `dist/` 做好准备，并且会生成所有的代码依赖，也包括 vender 依赖。

## Webpack

Vue CLI 在内部使用了 webpack ，但是配置项被抽象了，甚至在工作目录下看不到。但是你还是可以通过使用 `vue inspect` 来访问它：

![vue-inspect](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/vue-cli/15.png)
