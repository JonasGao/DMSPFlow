DMSPFlow
====

主要定义四个分支

1. develop
2. master
3. staging
4. production

## 通用

1. 所有 pull 操作，均使用 pull -r （pull rebase）

## 场景

### 常规开发

1. 直接拉取远程 develop 分支进行开发，`git checkout develop`；
1. 如果远程 develop 不存在；
    1. 如果当前本地分支不是 master；
        1. 签出 master 分支，`git checkout master`；
        1. 更新远程仓库，`git pull -r`；
    1. 创建新的 develop 分支，`git checkout -b develop`；
    1. 创建新的远程 develop 分支，`git push --set-upstream origin develop`;
1. 继续开发；
1. 提交更新；
    1. `git add <your changes>`
    1. `git commit`
    1. `git push`

### 发布测试环境

1. 如果本地分支不是 develop；
    1. 签出 develop，`git checkout develop`；
1. 如果远程 staging 分支不存在；
    1. 如果当前本地分支不是 master；
        1. 签出 master 分支，`git checkout master`；
        1. 更新远程仓库，`git pull -r`；
    1. 创建新的 staging 分支，`git checkout -b staging`；
    1. 创建新的远程 staging 分支，`git push --set-upstream origin staging`;
    1. 签出 develop，`git checkout develop`；
1. 更新 develop 分支，`git pull -r`；
1. 签出 master 分支，`git checkout master`；
1. 更新 master 分支`git pull -r`；
1. 合并 develop 分支到 master 分支，`git merge develop`；
1. 如果有冲突；
    1. 解决冲突并提交；
1. 提交合并，`git push`；
1. 如果需要重建 develop；请参考《常规开发》一节；
1. （可选）签回 develop 分支，`git checkout develop`；

### 发布生产环境

1. 更新 staging 分支，保证内容正确；
1. 签出 staging 分支，`git checkout staging`；
1. 更新远程仓库，`git pull -r`； 
1. 如果远程 production 分支不存在；
    1. 创建新的 production 分支，`git checkout -b production`；
    1. 创建新的远程 production 分支，`git push --set-upstream origin production`;
1. 签出 production 分支，`git checkout production`；
1. 合并 staging 分支到 production 分支，`git merge staging`；
1. 理论上不应有冲突，如果有冲突说明有人直接对 production 分支进行了提交，或是 hotfix 过；
    1. 确认是否需要重建（覆盖）production 分支；
    1. 重建 production 分支；
    1. 签出 staging 分支，`git checkout staging`；
    1. 删除本地 production 分支，`git branch -D production`；
    1. 创建新的 production 分支，`git checkout -b production staging`；
    1. 创建新的远程 production 分支，`git push --set-upstream origin production --force`;
1. 如果没有冲突；
    1. 提交合并，`git push`；
1. 创建 master tag，需要保证 master 没有新 commit；
1. 签出 master 分支，`git checkout master`；
1. 创建 tag，`git tag -a 'v2021.4.2'`；
1. 提交 tag，`git push --tags`；
1. （可选）签回 develop 分支，`git checkout develop`；
1. （可选）删除老版本的 hotfix 分支；
    1. `git branch -D hotfix/v2021.4.1`；
    1. `git push origin :hotfix/v2021.4.1`；

### 线上问题修复

1. 找到最近的 tag 标签（以下以 tag `v2021.4.2` 为例）；
1. 查找该 tag 是否已经存在对应的 hotfix 分支；
1. 如果不存在，创建对应 hotfix 分支；
    1. 签出 tag 为 hotfix 分支，`git checkout -b hotfix/v2021.4.2 v2021.4.2`；
    1. 创建新的远程 hotfix 分支，`git push --set-upstream origin hotfix/v2021.4.2`;
1. 如果已存在，直接签出 hotfix 分支，`git checkout hotfix/v2021.4.2`；
1. 修改代码，提交；
1. 提交 hotfix 到当前开发；
    1. 签出 master 分支，`git checkout master`；
    1. 更新远程仓库，`git pull -r`；
    1. 合并 hotfix 分支到 master 分支，`git merge hotfix/v2021.4.2`；
    1. 如果有冲突；
    1. 解决冲突并提交；
    1. 提交合并，`git push`；
1. 提交 hotfix 到生产发布；
    1. 签出 production 分支，`git checkout production`；
    1. 合并 hotfix 分支到 production 分支，`git merge hotfix/v2021.4.2`；
    1. 提交合并，`git push`；

## develop

develop 分支主要用于日常开发，对应开发环境。

1. 如果不需要对功能的开发进行隔离和拆分的，所有开发人员都在该分支上进行开发。
1. 在这个分支上，只要不会扰乱其他在本分支上开发的开发人员。那么，这个分支几乎允许一切行为。例如随时进行回退，或者任意提交不稳定的代码等。
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
