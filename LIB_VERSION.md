库工程版本
====

## 一、概述

本方法定义 dev 分支、master 分支、hotfix 分支和 staging 分支。

1. 所有 dev 分支和 hotfix 分支允许进行开发；
1. master 和 staging 不允许直接修改；
1. 发布到测试时，dev 合并到 staging；
1. 发布到生产是，dev 合并到 master；
1. hotfix 测试时，hotfix 合并到 staging；
1. hotfix 发布生产时，hotfix 合并到 master；
1. 所有发布生产的内容合并后，对 master 进行 release。版本号按格式递增；
1. 版本号使用“年.月.递增”格式；
1. hotfix 使用 release 的 tag 签出；
1. dev 和 hotfix 必须按照格式进行命名；
1. dev 和 hotfix 分支不允许 deploy 操作。

## 二、详细说明

### Master

master 分支主要用于生产发布，以及作为开发的主分支（或称基分支）。

不允许在 master 直接提交任何代码修改。所有的需要开发的内容，都从 master 签出新分支进行编写。最终发布时，使用 master 进行发布（Release）并修改版本号。以及打版本 tag。

每次发布后，又有新的内容要开发，则继续从 master 签出新分支。

这里要注意一下！旧分支务必删掉！因为如果 master 有 bug 需要修正，则从 release 的 tag 处直接签出新分支进行修改。所以旧分支没有存在的必要，没有用处。

### Staging

staging 分支主要用于进行测试发布。即所有 SNAPSHOT 版本发布。

因为要保证测试环境运行的代码和本地开发环境代码，不互相干扰。所以，本地开发时务必不要 `deploy` SNAPSHOT 版本。本地开发请使用 `install`。

同时为了保证 staging 最大可能的与最终生产发布的版本无差别，也不允许直接在 staging 提交任何代码修改。

不允许直接从 master 合并到 staging，以及从 staging 合并到 master。防止产生多余的 commit，以及造成代码混乱。

### Dev 前缀

所有需要新开发的内容，均从 master 签出。以 dev/xxxx 命名，“xxxx” 为需求或功能的 ID。需要进行测试时，直接合并到 staging。测试完成后，需要发布则直接合并到 master 分支。

禁止从 staging 和 master 反向合并到 dev/xxxx 分支。

如果需要与 dev/zzzz 共享功能，则新建单独的 dev/yyyy 分支共享开发。之后 dev/xxxx 合并 dev/yyyy，dev/zzzz 合并 dev/yyyy。

### Hotfix 前缀

当需要对生产环境的问题进行修复时，从上一次 release 的 tag 处签出。以 hotfix/xxxx 命名，“xxxx” 是版本号码。需要进行测试时，直接合并到 staging。测试完成后，需要发布则直接合并到 master 分支。

禁止从 staging 和 master 反向合并到 hotfix/xxxx 分支。

因为每次发布都会 release 新的 tag。所以每次修复完之后的 hotfix 分支也是没有任何用处的。请务必删掉！
