---
title: git常用基本命令（上）
copyright: true
date: 2019-05-14 15:55:46
tags:
- git
categories:
- git
comments: true
keywords: git命令
---

>简单说一下git常用的命令，包括上下两个部分，下期主要讲git分支管理部分

***
<!-- more -->

**1.克隆现有的仓库**
`git clone [url]`

**2.初始化仓库**
`git init`

**3.把文件提交到暂存区**
`git add`

**4.把所有的文件提交到暂存区**
`git add .` 或 `git add *`

注：包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件

**5.提交文件到仓库**
`git commit -m '对本次提交的描述'`

注：添加的描述最好是英文，中文也可以

**6.检查当前文件状态**
`git status`

**7.查看提交历史**
`git log`

扩展：
![git log](pic1.png)

**8.撤消操作**
`git commit --amend`
有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。这时就可以用这个命令。

例：提交后发现忘记了暂存某些需要的修改

```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
最终你只会有一个提交，第二次提交将代替第一次提交的结果

**9.取消暂存的文件**
`git reset HEAD 文件名`

**10.回退版本**

 - 回退到上一个版本`git reset --hard HEAD^`
 - 回退到某一个版本：`git reset --hard <git版本号>`(版本号可在`git log`中查看)
