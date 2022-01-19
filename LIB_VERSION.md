库工程版本
====

## 概述

本方法定义 dev 分支、master 分支、hotfix 分支和 staging 分支。

## master

master 分支主要用于生产发布，以及作为开发的主分支（或称基分支）。但是不允许在 master 直接提交任何代码修改。所有的需要开发的内容，都从 master 签出新分支进行编写。最终发布时，使用 master 进行发布（Release）并修改版本号。以及打版本 tag。每次发布后，又有新的内容要开发，则继续从 master 签出新分支。这里要注意一下！旧分支务必删掉！因为如果 master 有 bug 需要修正，则从 release 的 tag 处直接签出新分支进行修改。所以旧分支没有存在的必要，没有用处。

## staging

staging 分支主要用于进行测试发布。即所有 SNAPSHOT 版本发布。本地开发时务必不要 `deploy` SNAPSHOT 版本。本地开发请使用 `install`。
