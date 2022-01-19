库工程版本
====

主要定义四个分支

1. dev/xxxx
2. master
3. staging
4. hotfix/xxxx

## 场景

### 开发新功能

1. 更新 master 分支，保证最新；
    1. `git checkout master`
    1. `git pull -r`
1. 从 master 新建 dev/xxxx 分支。`git checkout -b dev/xxxx master`；
1. 进行修改代码；提交等；
1. 提交到远程，准备发布测试；
    1. 第一次提交时，`git push origin HEAD:dev/xxxx`

### 发布测试

1. 发布测试改用线上进行合并
1. 在 <Repo> （可以是 gitlab / github 等）创建 dev/xxxx 合并到 staging 的请求
1. 管理员进行合并后，流水线自动发布

### 发布生产

1. 发布测试改用线上进行合并
1. 在 <Repo> 创建 dev/xxxx 合并到 master 的请求
1. 管理员进行合并后，并删除源分支

### 线上问题修复

1. 更新 master 分支，保证最新；
    1. `git checkout master`
    1. `git pull -r`
1. 从 master 新建 hotfix/xxxx 分支。`git checkout -b hotfix/v2022.1.1 v2022.1.1`；
1. 进行修改代码；提交等；
1. 提交到远程，准备发布测试；
    1. 第一次提交时，`git push origin HEAD:hotfix/v2022.1.1`
