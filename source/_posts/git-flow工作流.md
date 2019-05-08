---
title: git flow工作流
copyright: true
date: 2019-05-08 17:12:19
tags:
- gitFlow
categories: 
- git
comments: true
keywords: gitFlow
---
>git flow使用介绍
***
<!-- more -->
### 1.GitFlow是什么
`GitFlow`是一套基于git的工作流程，这个工作流程围绕着项目发布定义了一个严格的如何建立分支的模型。
`GitFlow`规定了如何建立、合并分支，如何发布，如何维护历史版本等工作流程。简单说就是每一个功能特性的开发是在分支上开发，而不是在主干开发，分支开发完毕后再合并到主干上。
![gitFlow](pic1.png)


### 2.GitFlow的优势
GitFlow工作流程的优势在于：

 - 还处于半成品状态的feature不会影响到主干
 - 各个开发人员之间做自己的分支，互不干扰
 - 主干永远处于可编译、可运行的状态


### 3.GitFlow分支简述
**2.1主干分支（master和develop）**
1.master分支存储了发布版本的历史，各个版本通过tag来标记（`git tag -a v0.1`）
2.develop分支是一个集成分支，用来整合各个功能feature分支，也方便给master分支上的所有提交分配一个版本号。
![主干分支](pic2.png)

除了`master`和`develop`主分支线，其他的分支都是临时的分支，有一定的生命周期的，其余的工作流程分支都是围绕这两个分支之间的区别进行的。

**2.2功能分支(feature)**
![功能分支](pic3.png)
开发每个功能都必须新开个`feature`分支，`feature`分支派生自`develop`分支。开发完成后要合并回`develop`分支。`feature`分支永远不会和`master`分支打交道。

**2.3待发布分支(release)**
![待发布分支](pic4.png)
`release`分支不是一个放正式发布产品的分支，可以理解为“**预发布**”或者“**待发布**”分支。
当开发的功能完成并满足发布的条件时，将这些满足条件的`feature`分支合并到`develop`分支上，然后从`develop`分支开出一个`release`分支，开始准备一个发布版本。
在`release`分支上，**不能再添加新的功能**，但是我们**可以**:
 1. 将分支打包给测试人员测试
 2. 在这个分支上修改bug 
 3. 编写发布文档

当到发布日时，发布相关的工作都完成后，`release`分支合并回`master`分支，并打出版本标签，发布完成后，`release`分支合还要并回`develop`分支。

**2.4维护分支(hotfix)**
![维护分支](pic5.png)
维护分支也就是bug修复分支，用来快速修复生产环境的紧急问题。

项目发布后或多或少会有一些bug存在，而bug的修复工作并不适合在`develop`上做，这是因为：

 1. develop分支上可能包含还未验证过的feature 
 2. 用户未必需要develop上的feature 
 3. develop还不能马上发布，而客户急需这个bug的修复。

这个分支是唯一一个开放过程中直接从`master`分支派生来的分支。
快速的修复问题后，`hotfix`分支应该被合并回`master`分支，同时也要合并回`develop`分支，这样`develop`分支也能享受到bug修复的好处。然后`master`分支需要打一个版本标签，例如v0.11。

### 4.GitFlow的命名约定

 - 主分支名称：**master**
 - 主开发分支名称：**develop**
 - 标签（tag）名称：**v##**，如：v1.0.0
 - 新功能开发分支名称：**feature-##**  or **feature/##**，如：feature-games或feature/games
 - 发布分支名称：**release-##**  or **release/##**，如：release-1.0.0或release/1.0.0
 - 维护分支名称：**hotfix-##** or **hotfix/##** ，如：hotfix-update或hotfix/update

### 5.GitFlow的工作流程

**5.1 创建develop分支**
在本地`master`基础上创建一个`develop`分支，然后push到服务器；
```javascript
git branch develop
git push -u origin develop
```
以后这个分支将会包含项目的全部历史，而`master`分支将只包含了部分历史。
其它开发者这时应该克隆中央仓库（如果已经克隆过该项目，则不需要执行以下第一条命令。），建好`develop`分支的跟踪分支：

```javascript
git clone ssh://user@host/path/to/repo.git
git checkout -b develop origin/develop
```
**5.2 新建feature分支**
![新建feature分支](pic6.png)
基于`develop`分支创建新功能分支：
```javascript
git checkout -b feature/demo develop
```
推送到远程仓库，共享：
```javascript
git push
```
在此分支上开发提交代码:
```javascript
git status
git add
git commit -m '***'
```
**5.3 完成新功能开发（合并feature分支到develop）**
![完成新功能开发](pic7.png)
当确定新功能开发完成，且联调测试通过，并且新功能负责人已经得到合并`feature`分支到`develop`分支的允许；这样才能合并`feature`分支到`develop`。
```javascript
git pull origin develop
git checkout develop
git merge --no-ff feature/demo
git push
git branch -d feature/demo
```
第一条命令是确保在合并新功能之前，`develop`分支是最新的。
**注**：**新功能分支，永远不可以直接合并到master分支。合并可能会有冲突，应该谨慎处理冲突。**

**5.4 新建待发布分支release**
![新建待发布分支](pic8.png)
项目准备发布时，基于`develop`分支新建一个待发布分支`release`,确立版本号:
```javascript
git checkout -b release/v0.1 develop
```
推送到远程仓库共享：
```javascript
git push
```
这个分支是清理发布、执行所有测试、更新文档和其它为下个发布做准备操作的地方，像是一个专门用于改善发布的功能分支。

**5.5 release分支合并到master发布**
如果准备好了对外发布，就将`release`分支合并到`master`分支和`develop`分支上，并删除发布分支。
![发布分支](pic9.png)
`release`分支合并到`master`分支:
```javascript
git checkout master
git merge --on-off release/v0.1
git push
```
`release`分支合并到`develop`分支，合并完成后并删除发布分支:
```javascript
git checkout develop
git merge --on-off release/v0.1
git push
git branch -d release/v0.1
```
合并回`develop`分支很重要，因为在发布分支中已经提交的更新需要在后面的新功能中也要是可用的。

发布分支是作为功能开发（`develop`分支）和对外发布（`master`分支）间的缓冲。只要有合并到`master`分支，就应该打好Tag以方便跟踪。
```javascript
git tag -a v0.1 -m 'Initial public release' master
git push --tags
```

**5.6 线上Bug修复流程**
![线上Bug修复](pic10.png)
当终端用户，反馈系统有bug时，为了处理bug，需要从`master`中创建出维护分支`hotfix`，等到bug修复完成，需要合并回`master`

基于`master`新建`hotfix`分支：
```javascript
git checkout -b hotfix/v0.1.0.1 master
```
当问题修复完成，并测试通过后，将`hotfix`分支合并到`master`分支和`develop`分支，并打出一个标签。

将`hotfix`分支合并到`master`分支:
```javascript
git checkout master
git merge --on-off hotfix/v0.1.0.1
git push
```
将`hotfix`分支合并到`develop`分支,合并完成后删除`hotfix`分支：
```javascript
git checkout develop
git merge --on-off hotfix/v0.1.1
git push
git branch -d hotfix/v0.1.1
```
打标签：
```javascript
git tag -a v0.1.1 -m 'Initial public release' master
git push --tags
```
### 6.git flow工作流程也可以使用git flow工具完成

GitFlow不仅仅是一种规范，还提供了一套方便的工具。大大简化了执行GitFlow的过程。

**6.1 安装**
1.OSX
```javascript
$ brew install git-flow
```
2.Debian/Ubuntu Linux
```javascript
$ apt-get install git-flow
```
3.Windows(cygwin
```javascript
$ wget -q -O - --no-check-certificate https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-installer.sh | bash
```
**6.2 Initialize**
对一个git仓库配置一下git flow。主要是一些命名规范，比如`feature`分支的前缀，`hotfix`分支的前缀等。一般用默认值就行。

```javascript
git flow init
```

**6.3 新建feature分支**
从`develop`开启一个新的分支：
```javascript
git flow feature start MYFEATURE
```
这个命令会从`develop`分出一个分支，然后切换到这个分支上面。

**6.4 合并feature分支到develop**
一个`feature`分支开发完毕后，要做以下事情：

 - 把 MYFEATURE 合并到 develop
 - 把这个分支干掉
 - 切换回develop分支
```javascript
git flow feature finish FEATURE_NAME
```
**6.5 把feature分支推送到服务器**
果你想让别人和你一起开发MYFEATURE分支，那就把这个分支push到服务器上
```javascript
git flow feature publish MYFEATURE
```
获得一个别人push到服务器上的`feature`分支：
```javascript
git flow feature pull origin MYFEATURE
```
**6.6 新建release分支**
创建一个`release`分支，派生自`develop`分支：
```javascript
git flow release start RELEASE
```
**6.7 把feature分支推送到服务器**
```javascript
git flow release publish RELEASE
```
**6.8 合并release分支到master和develop**
一个`release`分支结束后，需要做以下工作：
 - 把release分支合并回master
 - 给本次发布打tag
 - 同时把release分支合并回develop
 - 干掉release分支

```javascript
git flow release finish RELEASE
```
最后不要忘记把tag push到服务器`git push --tags`

**6.9 新建hotfix分支**
开启一个hotfix分支：
```javascript
git flow hotfix start VERSION
```
结束一个`hotfix`分支，和`release`一样，同时合并回`develop`和`master`：
```javascript
git flow hotfix finish VERSION
```