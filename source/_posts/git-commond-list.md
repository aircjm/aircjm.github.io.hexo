---
title: git_commond_list
date: 2016-09-23 16:50:48
tags:
- git

---

git可以说越来越流行了，GitHub上面有很多开源项目可供参考学习，如果git都不会用，就比较尴尬了。



git的学习其实很简单的。

下面这个伯乐在线的博文零基础教你如何使用git，希望对你的使用有帮助。

[推荐！手把手教你使用Git](http://blog.jobbole.com/78960/)



这个正常来讲，你跟着这个教程手动操作一遍，基本上对于简单的git操作就会了，但是git操作的精髓就在于使用命令行进行操作。

git的命令行有很多，我这边引用了阮一峰的博文日志，[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html?utm_source=tool.lu)。有兴趣可以看下。

<!--more-->

### 1.新建代码库

在当前目录新建一个Git代码库

`git init`

新建一个目录，并且将它初始化Git代码库

`git init [project-name]`

下载一个项目和它的整个代码历史

`git clone [url]`

### 2.配置

Git的配置文件为.gitconfig，他可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）

显示当前的Git配置

`git config --list`

编辑Git配置文件(不怎么常用，基本上一次配置结束之后，很少会对配置进行操作)

`git config -e [--global]`

设置提交代码时的用户信息

`git config [--global] user.name "[name]"`

`git config [--global] user.email "[email address]"`

### 3.增加/删除文件

 添加指定文件到暂存区

`git add [file1][file2] ...`

添加指定目录到暂存区，包括子目录

`git add [dir]`

添加当前目录的所有文件到暂存区

`git add .`

删除工作区文件，并且将这次删除放入暂存区

`git rm [file1][file2] ...`

 停止追踪指定文件，但该文件会保留在工作区

`git rm --cached [file]`

改名文件，并且将这个改名放入暂存区

`git mv [file-original][file-renamed]`

### 4.代码提交

提交暂存区到仓库区

`git commit -m [message]`

提交暂存区的指定文件到仓库区

`git commit [file1][file2] ... -m [message]`

提交工作区自上次commit之后的变化，直接到仓库区

`git commit -a`

提交时显示所有diff信息

`git commit -v`

使用一次新的commit，替代上一次提交

`git commit --amend -m [message]`

重做上一次commit，并包括指定文件的新变化

`git commit --amend [file1][file2] ...`

### 5.分支

列出所有本地分支

`git branch`

列出所有远程分支

`git branch -r`

列出所有本地分支和远程分支

`git branch -a`

新建一个分支，但依然停留在当前分支

`git branch [branch-name]`

新建一个分支，并切换到该分支

`git checkout -b [branch]`

新建一个分支，指向指定commit

`git branch [branch][commit]`

新建一个分支，与指定的远程分支建立追踪关系

`git branch --track [branch][remote-branch]`

切换到指定分支，并更新工作区

`git checkout [branch-name]`

建立追踪关系，在现有分支与指定的远程分支之间

`git branch --set-upstream [branch][remote-branch]`

合并指定分支到当前分支

`git merge [branch]`

选择一个commit，合并进当前分支

`git cherry-pick [commit]`

删除分支

`git branch -d [branch-name]`

删除远程分支

`git push origin --delete [branch-name]`

`git branch -dr [remote/branch]`

