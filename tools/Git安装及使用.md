---
title: Git安装及使用
tags: [Git]
date: 2016-3-6 08:38:24
categories: tools
---


## Git控制流程图
![image](http://www.zhanglian2010.cn/wp-content/uploads/2014/07/XwVzT.png)

## Git安装

> 以下说明均是基于Window环境(本人电脑环境)

msysgit是windows版的git，从https://git-for-windows.github.io下载，然后按默认选项安装即可。

在开始菜单里找到“git”->“git bash”，蹦出一个类似命令行窗口的东西，说明git安装成功

安装完成后进行以下设置:

```
$ git config --global user.name "your name"
$ git config --global user.email "email@example.com"
```
注意:git config命令的--global参数，用了这个参数，表示你这台机器上所有的git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和email地址

## 创建版本库
> 版本库又名仓库，英文名repository

1. 创建空目录
```
$ mkdir learngit
$ cd learngit
$ pwd
/users/michael/learngit
```
`pwd`命令用于显示当前目录。在我的mac上，这个仓库位于`/users/michael/learngit`

为防止出现不可预知的bug,请使用英文目录

2. 通过`git init`命令把这个目录变成git可以管理的仓库：

```
$ git init
initialized empty git repository in /users/michael/learngit/.git/
```
瞬间git就把仓库建好了，而且告诉你是一个空的仓库（empty git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

## 把文件添加到版本库
创建一个说明文件readme.txt,输入以下内容

```
git is a version control system.
git is free software.
```
第一步，用命令git add告诉git，把文件添加到仓库：

```
$ git add readme.txt
```
第二步，用命令git commit告诉git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"
[master (root-commit) cb926e7] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```
## 版本控制
继续修改readme.txt文件

```
git is a distributed version control system.
git is free software.
```
使用`git status`命令当前状态：

```
$ git status
# on branch master
# changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```
使用`git diff`命令查看修改内容:

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-git is a version control system.
+git is a distributed version control system.
 git is free software.
```
### 提交版本
第一步是`git add`：

```
$ git add readme.txt
```
第二步git commit

```
$ git commit -m "add distributed"
```
小结

- 要随时掌握工作区的状态，使用git status命令。

- 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

### 版本回退
修改readme.txt内容:

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```
提交当前版本:

```
$ git add readme.txt
$ git commit -m "append GPL"
[master 3628164] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```
- 使用git add对文件做修改
- 使用git commit创建版本序列

##### 回顾以下提交过的历史版本:
版本1：wrote a readme file

```
Git is a version control system.
Git is free software.
```
版本2：add distributed

```
Git is a distributed version control system.
Git is free software.
```
版本3：append GPL


```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

使用`git log`查看commit历史记录

```
$ git log
commit 3628164fb26d48395383f8f31179f24e0882e1e0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 15:11:49 2013 +0800

    append GPL

commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 14:53:12 2013 +0800

    add distributed

commit cb926e7ea50ad11b8f9e909c05226233bf755030
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 19 17:51:55 2013 +0800

    wrote a readme file
```

使用`$ git log --pretty=oneline`简化显示历史记录

```
$ git log --pretty=oneline
3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file
```

在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（每个人的id不同），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

回退到上一个版本“add distributed”，使用git reset命令：

```
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed
```
查看readme.txt当前内容:

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```
还可以继续回退到上一个版本wrote a readme file，不过且慢，然我们用git log再看看现在版本库的状态：

```
$ git log
commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 14:53:12 2013 +0800

    add distributed

commit cb926e7ea50ad11b8f9e909c05226233bf755030
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 19 17:51:55 2013 +0800

    wrote a readme file
```
只要上面的命令行窗口还没有被关掉,通过commit id可以回到未来版本:

```
$ git reset --hard 3628164
HEAD is now at 3628164 append GPL
```
即使关掉命令条,也可以通过`git reflog`命令查看版本操作记录:

```
$ git reflog
ea34578 HEAD@{0}: reset: moving to HEAD^
3628164 HEAD@{1}: commit: append GPL
ea34578 HEAD@{2}: commit: add distributed
cb926e7 HEAD@{3}: commit (initial): wrote a readme file
```
#### 小结

- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
- 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

### 工作区和暂存区
> git和其他版本控制系统如svn的不同之处就是有暂存区的概念

### 工作区(working directory)
就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：
### 版本库(repository)
工作区的隐藏目录`.git`,就是git的版本库

git版本库中存了很多东西,其中最重要的就是stage(或称index)的暂存区,以及git自动创建的第一个分值`master`,还有指向`master`的指针`head`

![image](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

向git版本库添加,分两步执行:
1. `git add`,将文件添加到暂存区
2. `git commit`,将暂存区的内容提交到当前分支

因为git自动创建master分支,git commit就是想master分支提交更改

### 管理修改
git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中。

`git commit`只会提交暂存区上的修改到分支上,如本地修改没有提交到暂存区,那同样不会提交到master分支上

### 撤消更改
1. 本地做了修改,未提交到暂存区
`git checkout -- file`可以丢弃工作区的修改：

```
$ git checkout -- readme.txt
```
可以让这个文件回到最近一次git commit或git add时的状态

2. 修改已添加到暂存区,未添加到分支
用命令`git reset head file`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

```
$ git reset head readme.txt
unstaged changes after reset:
m       readme.txt
```

#### 小结

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset head file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 删除文件
命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

命令`rm`可以删除本地文件

## 远程仓库
### 添加远程仓库
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而svn在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 克隆远程仓库
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
## 分支管理
### 创建与合并分支

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 解决冲突
`git log --graph`命令可以看到分支合并图

### 分支管理策略
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了

### Bug分支
突然接到任务需要放下工作修改bug分支,可以将工作区内容暂存(需要被git管理)
```
$ git stash
```
建立`issue`分支,修复big

查看暂存文件的列表
```
$ git stash list
```


修复完毕后取回暂存的文件
```
$ git stash apply //取出暂存文件但文件不删除
$ git stash pop //取出暂存文件并删除
```

### Feature分支
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

### 多人协作
查看远程库信息
```
$ git remote
origin
```
显示更详细的信息
```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```
### 推送分支
推送主分支:
```
$ git push origin master
```
推送其他分支:
```
$ git push origin dev
```
### 抓取分支

```
$ git clone git@github.com:michaelliao/learngit.git
```
要在dev分支上开发，就必须创建远程origin的dev分支到本地:
```
$ git checkout -b dev origin/dev
```
可以将dev上的修改push到远程:
```
$ git commit -m "add /usr/bin/env"
$ git push origin dev
```
### 推送冲突
远程库上最新提交和你试图推送的提交有冲突，Git提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：
```
$ git pull
```
git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接,再pull：
```
$ git branch --set-upstream dev origin/dev
Branch dev set up to track remote branch dev from origin.

$ git pull
Auto-merging hello.py
CONFLICT (content): Merge conflict in hello.py
Automatic merge failed; fix conflicts and then commit the result.
```
git pull成功，但是合并有冲突，需要手动解决:
```
$ git commit -m "merge & fix hello.py"
$ git push origin dev
```


多人协作的工作模式通常是这样：

- 首先，可以试图用git push origin branch-name推送自己的修改；

- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并(失败的话需要对本地和远程分支做关联)；

- 如果合并有冲突，则解决冲突，并在本地提交；

- 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

- 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

#### 小结

- 查看远程库信息，使用git remote -v；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致
- 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
- 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

## 标签管理
### 创建标签
```
$ git tag v1.0
```
用命令git tag查看所有标签：
```
$ git tag
v1.0
```
- 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

- git tag -a <tagname> -m "blablabla..."可以指定标签信息；
 
- git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
 
- 命令git tag可以查看所有标签。

### 操作标签
- 命令git push origin <tagname>可以推送一个本地标签；
 
- 命令git push origin --tags可以推送全部未推送过的本地标签；

- 命令git tag -d <tagname>可以删除一个本地标签；

- 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

## Fork参与开源项目
在GitHub上，可以任意Fork开源仓库；

自己拥有Fork后的仓库的读写权限；

可以推送pull request给官方仓库来贡献代码

## Git自定义
### 忽略特殊文件
在.gitignore文件内编写忽略列表

ex1:
```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini
```
ex2:
```
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build
```
ex3:
```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```
#### 小结

- 忽略某些文件时，需要编写.gitignore；
 
- .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

### 配置别名
使用st就表示status：
```
$ git config --global alias.st status
```
还有
```
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```

### 搭建Git服务器
搭建Git服务器非常简单，通常10分钟即可完成；

要方便管理公钥，用Gitosis；

要像SVN那样变态地控制权限，用Gitolite。











































