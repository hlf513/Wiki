---
layout: post
title: Git 常用命令
date: '2017-11-16 21:21'
toc: true
tags:
  - git
top: 9999
---

# 工作流程图
![git](/images/2017/11/16/git.png)

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

# GIT 配置

Git的设置文件为.git/config，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

- 显示当前的Git配置:
`git config --list`
- 编辑Git配置文件:
`git config -e [--global]`
- 设置提交代码时的用户信息
`git config [--global] user.name [name]`
`git config [--global] user.email [email address]`

# 新建代码库

- 当前目录
_空目录/拷贝的 git 目录_
`git init`
- 创建目录
`git init [project-name]`
- 下载一个项目(包含历史)
`git clone [url]`
- 本地初始化远端
`git remote add origin [http://xxx.git]`
`git push -u origin master`


# 增加/删除文件

- 添加指定文件到暂存区
`git add [file1] [file2] ...`
- 添加指定目录到暂存区，包括子目录
`git add [dir]`
- 添加文件的部分到暂存区
`git add -e [file]`
- 删除工作区文件，并且将这次删除放入暂存区
`git rm [file1] [file2] ...`
- 停止追踪指定文件，但该文件会保留在工作区
`git rm --cached [file]`
- 改名文件，并且将这个改名放入暂存区
`git mv [file-original] [file-renamed]`


# 代码提交

- 提交暂存区到仓库区
`git commit -m [message]`
- 提交暂存区的指定文件到仓库区
`git commit [file1] [file2] ... -m [message]`
- 提交工作区自上次commit之后的变化，直接到仓库区
`git commit -a`
- 提交时显示所有diff信息
`git commit -v`
- 使用一次新的commit，替代上一次提交
_如果代码没有任何新变化，则用来改写上一次commit的提交信息_
`git commit --amend -m [message]`
- 重做上一次commit，并包括指定文件的新变化
`git commit --amend ...`
- 修改提交信息
`git rebase HEAD^^ -i`
`//-i 是交互模式`
`//reword 修改注释信息`
`//squash 与之前的注释合并`

# 暂存数据（Stage）

- 把文件写入 Stage
`git stash save [file]`
- 查看 Stage 列表
`git stash list`
- 应用最后一个 Stage
`git stash pop`
- 应用指定的 Stage
`git stash apply [stash@{1}]`

# 远程同步

- 下载远程仓库的所有变动
`git fetch [remote]`
- 下载远程仓库的所有tag
`git fetch —tags`
- 只下载远程仓库的分支
`git fetch [remote] <本地master>:<新建分支名>`
- 显示所有远程仓库
`git remote -v`
- 显示某个远程仓库的信息
`git remote show [remote]`
- 增加一个新的远程仓库，并命名
`git remote add [shortname] [url]`
- 取回远程仓库的变化，并与本地分支合并
`git pull [remote] [branch]`
- 上传本地指定分支到远程仓库
`git push [remote] [branch]`
- 强行推送当前分支到远程仓库，即使有冲突
`git push [remote] --force`
- 推送所有分支到远程仓库
`git push [remote] --all`
- 在已有项目中添加子项目
`git submodule add [remote] [dir]`

# 撤销
- 恢复暂存区的指定文件到工作区
`git checkout [file]`
- 恢复某个commit的指定文件到工作区
`git checkout [commit] [file]`
- 恢复上一个commit的所有文件到工作区
`git checkout . `
- 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
`git reset [file]`
- 重置暂存区与工作区，与上一次commit保持一致
`git reset --hard`
- 重置暂存区，保留已修改
`git reset —soft`
- 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
`git reset [commit]`
- 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
`git reset --hard [commit]`
- 重置当前HEAD为指定commit，但保持暂存区和工作区不变
`git reset --keep [commit]`
- 新建一个commit，用来撤销指定commit
`#_后者的所有变化都将被前者抵消，并且应用到当前分支_`
`git revert [commit]`

# 分支
- 列出所有本地分支
`git branch`
- 列出所有远程分支
`git branch -r`
- 列出所有本地分支和远程分支
`git branch -a`
- 新建一个分支，但依然停留在当前分支
`git branch [branch-name]`
- 新建一个分支，并切换到该分支
`git checkout -b [branch]`
- 新建一个分支，指向指定commit
`git branch [branch] [commit]`
- 新建一个分支，与指定的远程分支建立追踪关系
`git branch --track [branch] [remote-branch]`
- 切换到指定分支，并更新工作区
`git checkout [branch-name]`
- 建立追踪关系，在现有分支与指定的远程分支之间
`git branch --set-upstream [branch] [remote-branch]`
- 合并指定分支到当前分支
`git merge [branch]`
- 保留 commit 信息（即使在 fast-forward模式）
`git merge –no-ff [branch]`
- 合并多个commit为一次提交
`git merge —squash [branch]`
- 选择一个commit，合并进当前分支
`git cherry-pick [commit]`
- 删除分支
`git branch -d [branch-name]`
- 删除远程分支
`git push origin --delete`
`git branch -dr`
- 衍合分支到主干
`git checkout [branch]`
`git rebase [master]`
- 合并分支到主干
`git checkout [master]`
`git merge [branch]`

# 标签
**若 tag 和 branch 重名，操作时可指定，比如：tags/v2**
- 列出所有tag
`git tag`
- 新建一个tag在当前commit
`git tag [tag]`
- 新建一个tag在指定commit
`git tag [tag] [commit]`
- 查看tag信息
`git show [tag]`
- 提交指定tag
`git push [remote] [tag]`
- 提交所有tag
`git push [remote] --tags`
- 新建一个分支，指向某个tag
`git checkout -b [branch] [tag]`

# 查看信息
**多个条件默认是or 关系，使用 --all-match 变为 AND 关系**

- 显示有变更的文件
`git status`
- 显示当前分支的版本历史
`git log`
- 显示commit历史，以及每次commit发生变更的文件
`git log --stat`
- 显示某个文件的版本历史，包括文件改名
`git log --follow [file]`
`git whatchanged [file]`
- 显示指定文件相关的每一次diff
`git log -p [file]`
- 显示两个 tag 之间的版本历史
`git log [tag1]…[tag2]`
- 显示指定时间范围的版本历史
`git log --since=2.months.ago --until=1.day.ago`
- 显示在分支2而不在分支1上的版本历史（branch2没有提交的历史）
`git log [old_branch1]…[new_branch2]`
- 搜索特定提交者
`git log —author=[name]`
- 搜索历史信息
`git log —grep='search message'`
- 显示指定文件是什么人在什么时间修改过
`git blame [file]`
- 显示暂存区和工作区的差异
`git diff`
- 显示暂存区和上一个commit的差异
`git diff --cached`
- 显示工作区与当前分支最新commit之间的差异
`git diff HEAD`
- 显示两次提交之间的差异
`git diff [first-branch]...[second-branch]`
- 显示某次提交的元数据和内容变化
`git show [commit]`
- 显示某次提交发生变化的文件
`git show --name-only [commit]`
- 显示某次提交时，某个文件的内容
`git show [commit]:[filename]`
- 显示分支发生的变化
`git show [branch]`
- 显示最后一次提交发生的变化
`git show HEAD`
- 显示当前分支的最近几次提交
`git reflog`
- 查找信息
`git grep [pattern] [search_scope]`
- 查看分支间的关系
`git show-branch`
`//(加号)表示所在分支 包含 此行所标识的 commit`
`//(空格)表示所在分支 不包含 此行所标识的 commit`
`//(减号)表示所在分支是经过 merge 得到的,而所在行的内容即是 merge 的基本信息`
`//(星号)表示如果需要在某列标识+(加号),且此列为当前分支所在列,那么则将+(加号)转变为*(星号)`
- 显示两个分支的差异
`git whatchanged -p [branch1]…[branch2]`

# 其他命令
- 仓库移了位置
`git remote set-url origin <new repo addr>`
- 生成一个可供发布的压缩包
`git archive`
- 压缩信息，清理垃圾
`git gc`
- 一致性检查
`git fsck`
- 查看对象数据库
`#四种对象类型：blob、commit、tag、tree`
`#commit 指向 tree`
`#tree 显示一个目录的状态，指向 tree 或者 blob`
- 显示对象类型
`git cat-file -t [ID]`
- 显示对象信息
`git cat-file [type] [ID]`
- 显示 tree 信息
`git ls-tree [ID]`

# 忽略本地修改

- 忽略本地修改（仓库存在）
`git update-index --assume-unchanged [file]`
- 删除忽略的本地修改
`git update-index --no-assume-unchanged [file]`

# 意外情况

- 找回删除的文件
`git reflog / git fsck --lost-found`
`git checkout [hash值]`
- 从本地分支生成 patch（用于 email 提交）
`git format-patch origin`
- 与主干同步
`git fetch origin`
`git rebase origin/master`
- 提交到了错误的分支
`git branch experimental //创建一个指向当前master的位置的指针`
`git reset --hard master~3 //移动master分支的指针到3个版本之前`
`git checkout experimental`
