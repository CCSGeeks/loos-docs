# GIT
Git 是一个免费、开源的分布式版本控制系统，非常推荐大家掌握必要的 Git 操作，这对大家协作进行大作业的开发非常有效。

## Git 有哪些好处？

- 版本控制：Git 可以跟踪代码的变化，允许开发人员随时访问以前的版本。这意味着您可以轻松地撤消错误更改，恢复文件状态，比较代码差异等等。在团队协作中，Git 可以允许团队成员协同工作，对同一份代码进行修改和版本管控，保证了代码的稳定性与质量。
- 分支管理：Git 的分支管理可以使多个团队成员同时在代码库上开发不同的功能和新特性，简化了并行开发的过程。开发人员可以使用 Git 分支来隔离他们的工作，不影响其他开发人员的工作，同时可以方便地合并多个分支，使开发环节更清晰明确。
- 回溯和还原：Git 允许开发人员回到任何一个历史版本，并进行修改和还原操作，这是非常简单、快速和可靠的。这意味着开发人员可以找到系统中的问题，快速定位问题源头，并进行解决。
- 远程协作：Git 可以让团队成员在不同的地点协作开发，而不会受到距离和时间限制。团队成员可以在本地上建立与远程代码库的连接，并轻松推送和拉取变更。这使得团队内成员的协作更加自然、高效。



## 日常操作流程
1. 初始化仓库
```bash
git init          # 本地新建仓库
git clone <URL>   # 克隆远程仓库（如GitHub/GitLab项目）:cite[9]
```
2. 提交更改
```bash
git add .          # 添加所有修改到暂存区
git commit -m "feat: add new feature"  # 提交并附规范化的提交信息:cite[9]
git push origin main  # 推送至远程仓库
```
commit 可以一次提交很多文件，所以可以多次 add 不同的文件。不过，你可以在仓库目录下编写一个名为 .gitignore 的文本文件，并将不需要维护版本的文件/文件夹的相对路径逐行写入，这可以避免维护对项目版本无影响的文件，如人工智能项目中的模型文件就可以避免维护。
3. 拉取与更新
```bash
git pull origin main  # 拉取远程最新代码并合并
git fetch origin     # 仅获取远程变更记录（不自动合并）
```
## 分支管理
一般来说，main / master 分支中的内容是程序员认为稳定、可靠的版本。如果想要在此基础上进行新特性的开发，又担心产生 bug，则可以创建一个新的分支，在新的分支上改动，等测试稳定后再与主分支合并，或者因 bug 而直接丢弃新分支。这比起回退版本来说更加可靠和容易。

1. 常用命令
```bash
git branch               # 查看本地分支
git checkout -b dev      # 创建并切换到新分支
git merge dev            # 将dev分支合并到当前分支
git branch -d dev        # 删除本地分支
git push origin --delete dev  # 删除远程分支:cite[9]
```
2. 解决冲突: 合并时若发生冲突，手动编辑文件后执行：
```bash
git add .
git commit -m "fix: resolve merge conflict"
```
## 远程仓库协作
1. 关联远程仓库
```bash
git remote add origin <URL>    # 添加远程仓库（默认命名origin）
git remote -v                 # 查看关联的远程仓库:cite[9]
```

2. 多人协作流程
```bash
git fetch upstream      # 获取上游仓库更新（如团队主仓库）
git rebase upstream/main  # 变基以保持提交历史线性:cite[9]
```

## 撤销与回退
```bash
git reset --soft HEAD^   # 撤销上一次提交，保留修改
git checkout -- <file>   # 丢弃工作区未暂存的修改
git revert <commit-id>   # 生成新提交以撤销指定提交
```