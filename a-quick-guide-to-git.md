# Git 快速入门指南

> Git 是一个开源、免费的*版本控制系统（VCS）*，用来跟踪文件版本的技术，同时它提供回滚和维护不同版本的功能。

<!-- TOC -->

- [Git 快速入门指南](#git-快速入门指南)
  - [什么是 Git](#什么是-git)
  - [分布式版本控制系统](#分布式版本控制系统)
  - [安装 Git](#安装-git)
    - [OSX](#osx)
    - [Windows](#windows)
    - [Linux](#linux)
  - [初始化仓库](#初始化仓库)
  - [添加文件到仓库](#添加文件到仓库)
    - [添加文件到暂存区](#添加文件到暂存区)
  - [提交更改](#提交更改)
  - [分支](#分支)
  - [Push 和 Pull](#push-和-pull)
    - [添加远程库](#添加远程库)
    - [Push](#push)
    - [Pull](#pull)
    - [冲突](#冲突)
  - [命令行 vs 图形界面](#命令行-vs-图形界面)
    - [GitHub Desktop](#github-desktop)
    - [Tower](#tower)
    - [GitKraken](#gitkraken)
  - [良好的 Git 工作流](#良好的-git-工作流)
    - [快速功能](#快速功能)
    - [需多次提交的功能](#需多次提交的功能)
    - [热修复](#热修复)
    - [develop 不稳定。master 是最新的稳定版本](#develop-不稳定master-是最新的稳定版本)

<!-- /TOC -->

## 什么是 Git

Git 是一个开源、免费的版本控制系统（VCS），用来跟踪文件版本的技术，同时它提供回滚和维护不同版本的功能。

Git 是两种非常流行的版本控制系统 SVN 和 CVS 的继承者。由 Linus Torvalds （Linux 的创始人）开发，今天如果你要使用开源软件，你就无法绕开它。

## 分布式版本控制系统

Git 是一个分布式系统。开发者可以从中心位置克隆仓库，然后独立处理代码的某些部分，之后把这些更改提交，以便其他开发者可以更新。

Git 开发人员在代码库上协同工作变得更加轻松，它也提供了可以合并更改的工具。Github 是非常流行的一个用于托管 Git 仓库的服务，特别是对开源软件而言，但是我们也会提到 BitBucket 和 GitLab 等其它许多服务，他们被世界各地的团队广泛用于托管公开或私密的代码。

## 安装 Git

在所有平台上安装 Git 非常容易：

### OSX

使用 [Homebrew](http://brew.sh/)，运行下面的命令：

```shell
brew install git
```

### Windows

下载并安装 [Git for Windows](https://git-for-windows.github.io/)

### Linux

使用发行版的包管理器来安装 Git，例如：

```shell
sudo apt install git
```

或者：

```shell
sudo yum install git
```
---

## 初始化仓库

在系统上安装完 Git 之后，你可以通过在命令行键入 `git` 来访问它。

![git](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/git.svg)

假设你有一个干净的文件夹。你可以键入下面的命令来初始化一个仓库

```shell
git init
```

![git-init](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/2.png)

这个命令做了什么呢？它在你运行命令的文件里创建了一个 `.git` 文件夹。如果你看不见，那是因为它是一个隐藏的文件夹，所以它可能不会显示在任何地方，除非你将工具设置为”显示隐藏的文件“。

![ll](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/3.png)

在新创建的仓库中任何与 Git 有关的东西都会存储在 `.git` 目录下，除 `.gitignore` 之外，我将在下一篇文章里讨论这些内容。

---

## 添加文件到仓库

让我们看看如何将一个文件添加到 Git，键入以下命令创建一个文件：

```shell
echo "Test" > README.md
```

这个文件现在位于运行命令的目录里，但 Git 目前并没有被告知将文件添加在 Git 索引中，因为你可以看到 `git status` 告诉我们的信息：

![git-status](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/4.png)


### 添加文件到暂存区

我们需要如下命令使文件对 Git 可见，并且将其放到**暂存区**。

```shell
git add README.md
```

![git-add](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/5.png)

一旦文件位于暂存区后，你可以通过键入以下命令来删除它：

```shell
git reset README.md
```

但是通常添加一个文件后你要做的是提交它。

---

## 提交更改

如果在暂存区有一个或多个更改，你可以使用以下命令来提交：

```shell
git commit -am "Description of the change"
```

![git-commit](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/6.png)

![git-status](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/7.png)

这个命令将会清空暂存区的状态，并将所做的编辑永久记录到存储中，你可以通过键入 `git log` 来检查。

![git-log](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/8.png)

---

## 分支

当你把文件提交到 Git 时，你同时会把它提交到当前分支。

Git 允许你在多个独立分支上协同工作，不同的开发线代表主分支的不同开发分支。

Git 非常灵活，你可以同时拥有无限数量的分支，它们可以独立开发，直到你想要合并一个分支到另一个分支。

Git 默认会创建一个名为 `master` 的分支。他没有任何特别之处，除了它是在初始化时被创建的。

你可以通过键入下面的命令来创建一个名为 `develop` 的分支：

```shell
git branch develop
```

![git-branch](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/a-quick-guide-to-git/9.png)

如你所见，`git branch` 列出了仓库所拥有的分支，星号表示当前所在分支。

创建一个分支时，这个分支会指向当前分支所做的最后一次改动。如果你切换到新分支（使用 `git checkout develop` ）并运行 `git log` 时，你会看到和之前所在分支同样的日志。

## Push 和 Pull

在 Git 中你总是在本地提交。与 SVN 和 CVS 相比，这是一个很大的优势，因为后两者总是必须立即将改动提交到服务器。

你可以离线工作，执行任意次你想要的提交，一旦准备就绪，就可以将它们推送到服务器，如果你上传到 GitHub ，你的团队成员或者社区都可以访问到最新和最优秀的代码。

> Push 发送你的改动

> Pull 将远程改动下载到工作副本

不过，在使用 `push` 和 `pull` 之前，你需要添加一个远程库。

### 添加远程库

远程库位于另一台计算机上的仓库的一个副本。

我将使用 [GitHub](https://github.com) 作为例子。如果你已经有一个仓库，你可以发布在 GitHub 上。这个过程包括通过 Web 界面在该平台创建一个仓库，然后将该仓库添加为远程库，并且将代码推送到这个仓库。

添加一个远程库：

```shell
git remote add origin https://github.com/You/RepoName.git
```

> 另一种方法是在 GitHub 上创建一个空仓库，然后将其克隆到本地，这种情况下，会为你自动添加远程库。

### Push

完成之后，你可以将代码推送到远程库，使用 `git push <remote> <branch>` 来完成，例子如下：

```shell
git push origin master
```

你可以指定使用 `origin` 作为远程，因为从技术来说可以拥有多个远程库。这是我们之前添加的一个名称，同时这也是一种约定。

### Pull

同样的语法适用于拉取：

```shell
git pull origin master
```

它告诉 Git 从 `origin` 的 `master` 分支拉取更新，并合并到本地当前分支。

### 冲突

在 pull 和 push 之前都需要考虑一个问题：如果远程库中包含和你所做的更改有不兼容的地方，这次操作将会失败。

这种情况发生在，当远程库包含了最后一次 pull 后改动的代码时，这会影响到你所处理的代码。

在 push 时通常可以通过拉取改动、分析冲突，然后进行一次新的提交来解决它们。

在 pull 时，你的工作空间将会被自动编辑，你需要解决这些有冲突的更改，并且进行新的提交，以便当前代码库能够包含在远程库中所做的改动。

## 命令行 vs 图形界面

到目前为止，我讨论的使命令行的 Git 程序。

这是想你介绍 Git 如何工作的关键，但是在日常操作中，你可能会使用一个给你提供这些命令并且拥有漂亮 UI 的应用程序，尽管我认识的大多数开发者喜欢使用 CLI 。

> 举例来说，如果你要在远程服务器上使用 SSH 设置 Git ，CLI（命令行）依旧会证明自己仍有用武之地。那些不是无用的知识。

有许多非常好的应用程序使为了简化开发人员的工作而开发的，当你深入了解 Git 存储库的复杂性之后，它们非常有用。

一些最受欢迎的程序有：

### GitHub Desktop

[https://desktop.github.com](https://desktop.github.com)

免费，适用于 Mac 和 Win

### Tower

[https://www.git-tower.com](https://www.git-tower.com)

付费，适用于 Mac 和 Win

### GitKraken

[https://www.gitkraken.com](https://www.gitkraken.com)

根据需要选择免费/付费，适用于 Mac 、Win 和 Linux

## 良好的 Git 工作流

不同的开发者和团队喜欢使用不同的策略来有效地管理 Git 。这是我在许多团队用过且被开源项目广泛采用的策略，我也看到有大大小小的项目都在使用它。

这个策略的灵感来自一篇著名的文章：[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/) 。

我只有两个永久分支： `master` 和 `develop` 。

这些都是我在日常工作中遵守的规则：

当我处理问题或者添加新功能时，有 2 条路：

### 快速功能

我所做的提交不会破坏代码（至少我希望如此）：我可以在 develop 分支提交，或者分出一个功能分支，然后将它合并到 develop 分支。

### 需多次提交的功能

在功能完成之前并使其稳定下来可能需要数天的提交：我会分出一个功能分支，然后在开发完成后（可能需要数周）将其合并到 develop 分支。

### 热修复

如果生产服务器上需要立即采取行动的某些问题，例如需要我尽快解决的错误修复，我会新建 hotfix 分支，修复该问题，在本地和测试机器上测试该分支，然后将它合并到 master 和 develop 分支。

### develop 不稳定。master 是最新的稳定版本

开发分支始终处于变动状态，这也是为什么在准备发布前将其置于“冻结”状态。测试代码并检查每个工作流程来验证代码质量，为合并到 master 分支做好准备。

每次 develop 或者 hotfix 分支合并到 master 分支时，我会为它加一个版本号，并且如果在 GitHub 我还会创建一个新的版本，如果出现问题，很容易回到之前的版本
