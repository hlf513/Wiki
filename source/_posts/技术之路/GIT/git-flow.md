---
title: GitFlow
date: '2016-01-02 17:06'
tag:
  - git
toc: true
---

目前收集到的工作流有以下几种，其中最常用的是 Gitflow 和 Forking + Pull request   

# Git工作流
##  集中式
![git-svn](/images/2017/11/git-svn.png)
1. svn 形式的代码管理
2. 所有的代码都在 master 分支上开发（线性开发）


## 功能分支

![git-branch](/images/2017/11/git-branch.png)

1. 以集中式工作流为基础，不同的是新功能在新分支上开发
2. 开发完毕发起 pull request
3. 讨论通过后合并到 master

## Gitflow

![git-flow](/images/2017/11/git-flow.png)

### 来源
2010 年初，荷兰的程序员 Vincent Driessen 在他自己的博客 http://nvie.com/ 发表了一篇文章 《A successful Git branching model》

###  简述
1. 在功能分支的基础上，增加了维护和开发的便利性
2. 两个长期分支：master 和 develop；三个短期分支：feature，release，hotfix

### 5个分支说明
- master 分支只做发布
- develop 分支做开发功能集成
- feature 分支来自 develop 分支，用于新特性开发，被合并到 develop 分支
- release 分支来自 develop 分支，用于预发布与测试，被合并到 master 分支和 develop 分支
- hotfix 分支来自 master 分支，用于修改线上bug，被合并回 master 分支 和 develop 分支/release 分支

### 插件

```
 git flow feature start  // 开始一个特性的开发
 git flow feature finish // 完成一个特性的开发
 git flow release start // 开始一次 release
 git flow release finish // 完成一次 release
 git flow hotfix start // 开始一个线上bug修复
 git flow hotfix finish // 完成一个线上bug修复
```
>详情见 git flow

##  Forking + Pull request
1. 利用分支合并，方便接受其他贡献者的提交，而无须开放项目权限
2. 贡献者 push 自己的代码到自己的服务端仓库，发起 pull request
3. 项目的维护者review 后合并

>github，osc 等都使用此种工作流

###  本地添加远程项目

```
git remote add [upstream-name] [upstream-url]
```

### pull request的注意事项

1. base fork 与 head fork 的区别  

  **base fork** : 请求 pull requeat 的分支，通常是被 fork 的分支（维护者）； **head fork** ：希望被合并的分支，通常是自己fork 的分支（贡献者）

2. fork 的项目和原有项目保持同步  

**方式1：分步处理**

需要预览和对比，则使用：

```
$ git remote update [upstream-name]
$ git checkout [branch-name]
$ git rebase [upstream/branch-name]
```

​**方式2：一步处理**

不需要预览则使用：

```
$ git pull --rebase [upstream-name] [branch-name]
```

**方式3：脚本处理**

```
#!/bin/bash
gup() {
  local br
  br=\`git branch 2> /dev/null|\\grep '^*'|sed -e 's/..//;s/\\n//'\`
  tainted=\`git status --porcelain | \\grep -v '^\\?\\?'\`
  if [[ $br == master ]]; then
    if [[ $tainted == '' ]]; then
      echo git stash
      git stash
    fi
    echo git fetch
    git fetch
    echo git rebase FETCH_HEAD $br
    git rebase FETCH_HEAD $br
  else
    if [[ -n $br ]]; then
      if [[ $tainted == '' ]]; then
        echo git stash
        git stash
      fi
      echo git pull --rebase origin $br
      git pull --rebase origin $br
    else
      echo seems not in any branch
    fi
  fi
}
```

**方式4：使用 github 等自带的页面处理**

>**base fork** 选择自己的 fork 分支（贡献者）； **head fork** 选择被 fork 的分支（维护者）；然后发起 pull request


### 使用 merge 还是 rebase？
  * 开发以 pull request + review 为主的模式，merge 最合适
  * 第一次合并/单人开发/同步上游改动时，rebase 最合适

# 参考资料
1. [一个成功的 Git 分支模型](http://www.juvenxu.com/2010/11/28/a-successful-git-branching-model/)
2. [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
3. [Git Workflows and Tutorials](https://github.com/xirong/my-git/blob/master/git-workflow-tutorial.md)
4. [git 里 push request 注意事项](https://ruby-china.org/topics/22748)
