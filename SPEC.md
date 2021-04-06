DMSPFlow
====

主要定义四个分支

1. develop
2. master
3. staging
4. production

## 通用

1. 所有 pull 操作，均使用 pull -r （pull rebase）

## develop

develop 分支主要用于日常开发，对应开发环境。

1. 如果不需要对功能的开发进行隔离和拆分的，所有开发人员可以都在该分支上进行开发。
1. 在这个分支上，只要不会扰乱其他在本分支上开发的开发人员。那么，这个分支几乎允许一切行为。例如随时进行回退（当然不会回退超过 master 分支），或者任意提交不稳定的代码等。
1. 不允许 merge master。如果需要 master 分支代码，可以 rebase master。或重建分支。

## master

master 分支作为核心分支，作为其他分支的来源分支。staging 分支和 production 分支都直接来自 master 分支。

1. master 分支不允许 Owner 和 Maintainer 之外的人提交代码
1. 不允许 merge staging/production。
1. 不允许重建（非必要情况下）。
1. merge develop 前，需要保证 develop 代码基本的可用性。
1. 发布代码（merge to staging 和 production）前，需要添加 tag。

## staging

用于发布测试环境代码

1. 只允许 merge master。
1. 不允许提交任何其他 commit。
1. 允许从 master 重建。

## production

用于发布生产环境代码

1. 只允许 merge staging 和 hotfix。
1. 不允许提交任何其他 commit。
1. 允许从 staging 重建。

## hotfix

修复生产的紧急 Bug。

1. 只能从 develop 或 master 签出。
1. 签出需要根据最近的 tag 签出。
1. 从 develop 签出的，需要 merge 回 develop。从 master 签出的，需要 merge 回 master。不允许 merge 到其他分支。
1. 发布时，直接从 hotfix merge 到 production。
1. 命名格式 hotfix/\<bug-id>

## tag

生产发布标签，格式 。如 ，

1. 必须严格遵守《日历化版本 / Calendar Versioning》。
1. 必须严格遵照如下格式制定。
    1. YYYY.MM.MICRO
    1. 示例
        1. 2021/4/6 日发布：v2021.4.1
        1. 2021/4/7 日发布：v2021.4.2

## 退化

对于需要多功能隔离开发的情况，可以自行进行其他选择。如 Git Flow。

## 引用

1. 《Git 工作流程》http://www.ruanyifeng.com/blog/2015/12/git-workflow.html
1. 《A successful Git branching model》https://nvie.com/posts/a-successful-git-branching-model/
1. 《日历化版本 / Calendar Versioning》https://calver.org/overview_zhcn.html
