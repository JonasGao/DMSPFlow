DMSPFlow
====

简单的，基于四个主要分支的分支管理方法

## 概述

本方法主要基于 develop、master、staging、production 四个分支管理代码。四个分支分别代表开发过程中的四个环境。

**develop**

develop 分支主要用于日常开发，对应开发环境。

**master**

master 分支作为核心分支，作为其他分支的来源分支。

**staging** 和 **production**

staging 分支和 production 分支，分别对应测试环境和生产环境。作为环境的发布分支。所有的修改都不允许基于这两个分支开发。

## 引用

1. http://www.ruanyifeng.com/blog/2015/12/git-workflow.html
1. https://nvie.com/posts/a-successful-git-branching-model/
