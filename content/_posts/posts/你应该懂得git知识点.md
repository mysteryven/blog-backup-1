---
title: "你应该懂得git知识点"
date: 2018-11-15T13:53:00+08:00
draft: false
---

在公司用到的 VSC 是 SVN, 并且用到的也只是简单的几个命令，昨天我学习了一下 Github 的一些操作， 觉得很有必要学习一个 git 了，这对我理解 Github 的用法也很有帮助，也对我以后有好处。于是我今天从 Coursera 上注册了一个课程，准备跟随他学习一下 Git，笔记将记录在这里.

<!--more-->

## 常用命令

git rest --hard 是清除工作区和暂存区里面的代码，也就是清除当前未 add，和已经 add 但是没有 commit 的代码
git mv readme readme.md git 重命名的简便方式
git log -n4 --oneline 最近的 4 次
git log --master --graph 图例化
git help --web log
git config --local user.name 'wenzhe'
gitk -all 图形界面
git checkout -b newBranch 创建新分支并切换到
git diff 比较两个 commit 版本
git diff HEAD HEAD^
git branch -d branch 删除分支，如果没有合并 git 会提示，此时再 -D 删除最保险
git commit --amend
git rebase -i 父亲的 commitid，来修改一个 commit 或者多个
上面那个命令修改多个的时候可能不能直接行，那就 git rebase --continue
git diff --cached 暂存区和 head 指向的 commit 之间的差别
git diff 工作区和暂存区的差别，后面可以加文件名详细的比较文件

git reset HEAD 暂存区的都不要了，还原回工作区
变化工作区用 checkout 变暂存区用 reset
git checkout -- index.html 把工作区的文件改为和暂存区一样的

## 宏观的理解 Git

- continuous improvement by commits
- simultaneous(同时) stability and development by branches
- improved quality by pull requests
- git 都是以 git 开头，然后后面接空格
- ![屏幕快照 2018-08-24 上午9.58.01.png](https://i.loli.net/2018/08/24/5b7f664156b51.png)

## 一些不知道但是有用的命令

- git --version 查看版本
- git help init 查看所有的帮助
- git config --global user.name
- git log --oneline -number 输出 log 只一行 number 为指定数量

## 概念

- working tree 包含了项目的一次 commit 的文件、目录
- staging area 通过 add 添加到里面
- local resposition 包含了本地的多次 commit
- staging area local 在.git 这个文件夹里
- remote repository 在远程
- git status 是查看的 work tree 和 staging area 之间状态
- git add add untracked and modify to staging area
- git commit create snapshot of current project
- git clone 得到 local repository，可以指定名字在第二个参数
- git remove -v
- git push [-u][rep] [branch]
- git push -u origin master 如果当前分支与多个主机存在追踪关系，则可以使用 -u 参数指定一个默认主机，这样后面就可以不加任何参数使用 git push

## git 的 DAG 模型

- commit object - a small text file
- Annotaed tag - 特定 commit 的索引
- `git log --oneline --graph` 很好用
- Git IDs 是一个 40 位的 16 进制数，也是 git Object 的名字，Git IDs 通常是四个字母的缩写或者更多的缩写
- git show to show all ids
- git hash-object get file's hash id
- acyclic means no cyclesj

## git branches

master 指向该分支最近的一次提交，head 指向 master
head 指向当前的分支
~ 指向上一次提交
~~ 上一次的上一次
^1 first parent of the commit
^2 second parent of merge commit (比如在 a 节点分出 b 和 c， b&c -> d 在 d 运行 ^2 就是 c)
^^ first parent's first parent

每个 branch 构成了一个项目，commit 属于一个 branch
git branch 列出所有项目
git branch freatureX create branch
git checkout freatureX -> change branch
git branch -b newBranch create and change to newBranch
git 如果指向某个 commit，并想回到它工作，那就要创建一个新的分支继续
git branch -d featureX delete branch

merge:

- git checkout master git merge featureX git branch -d featureX
