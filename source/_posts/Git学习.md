---
title: Git学习
---

### 关于Git

<!--more-->

#### 1. 什么是git

- Git是一个分布式版本控制工具

#### 2. 集中式和分布式的区别

- 集中式版本控制的系统（svn）
  
  1. 版本库是集中存放在中央服务上的，往往在开发时，需要从中央服务器上获取最新的版本，
  
  2. 是按照文件进行存取的，导致版本库的体积会很大
  
  3. 每个分支和标签都是一个完整的目录，每次拉取都要下载分支上的所有内容，***且无法在分支级别做到提交隔离***。
  
  4. svn必须在有网的情况下才能使用。

- 分布式版本控制的系统（Git）
  
  1. 不存在中央服务区的概念，每个人的电脑都是一个完整的版本库，因此不需要网络就可以使用。
  
  2. 版本库是以元数据进行存取的，因此版本库的体积更小。
  
  3. ***原声的支持分支和标签***

- 为什么选择Git
  
  1. **更好的发布控制能力**，能够设置只有发布管理员才有权限推送的版本库版本和分支，用于稳定发布版本的维护。
  
  2. **隔离开发，提交审核**，Git服务器上可以实现用户自建分支和自建版本库的能力，这样团队中的新成员既能够将本地代码提交推送到服务器已对工作进行备份，又能够方便团队中的其他成员对自己的提交进行审核。
  
  3. **对合并更好的支持**，Git的分支设计比svn提供更好的合并追踪，这是开发者选择Git，抛弃svn的重要原因。
  
  4. **拥有更完成的生态**，例如github，gitlab...

#### 3. Git的基本概念

- **工作区**，简单的理解就是包含了.git目录的普通目录，

- **本地仓库**，在Git管理的项目下，都会有一个**.git目录，这个就是Git的本地仓库，这个目录里面存储了所有和该项目相关的所有历史记录。每次 git commit 的结果都会保存在这里。**本地仓库的概念是Git作为分布式版本控制的核心概念，也是区别与其他版本控制的地方，每个人员都拥有一个完整的版本库。

- **暂存区**，可以理解为Git暂时存放被改动文件的地方，是Git本地仓库的一部分。在工作区修改的文件是没有办法直接提交到Git版本库的，必须要**先通过 git add命令提交到暂存区**。暂存区内所存储的文件会被下一次 git commit 的时候作为一个新的版本提交到git仓库.
  
  - 如果直接在工作目录里修改了文件，使用git commit是不会提交到Git仓库的。
  
  - 一个文件一旦被暂存了，你可以继续修改工作目录的同一个文件，但是暂存区的文件是不会修改的，除非你再次使用 git add暂存最新的改动。
  
  - 每次先通过 git add命令提交到暂存区git commit的只会提交暂存区的文件，工作目录中的文件无论修改与否Git都会忽略，只提交暂存区的文件。

- **远端仓库**，我们的改动知识存储到本地仓库，如果想要其他人看到你的改动，就需要将本地版本push远端仓库。

#### 4. Git命令

- 版本库 
  1. 通过`git init`命令把这个目录变成Git可以管理的仓库。
  
  2. 用命令`git add`告诉Git，将这个文件添加到仓库。`git add <fielname1> <filename2>`，添加多个文件。
     
     - 参数`-f` 强制添加。
  
  3. 使用命令`git commit`,将这个文件提交到仓库。`git commit -m '<message>'`
  
  4. `git status`命令可以让我们随时掌握仓库的状态
     
     ```git
     $ git status
      On branch master
      Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
      modified: readme.txt
      no changes added to commit (use "git add" and/or "git commit -a")
     ```
     
     示例中的输出信息代表我们的 `readme.txt`文件被修改了。
  
  5. `git diff <filename>`可以告诉我们这个文件做了什么修改。
  ---
  
- 版本控制
  1. `git log`查看从最近到最远的提交日志。
     
     - `git log --pretty=oneline`可以查看版本号。
     
     - `git  log --graph`可以查看分支图。
  
  2. `git reset --hard HEAD^`回退到上一个版本。
     
     - `git reset --hard <版本号>`回退到指定版本。
  
  3. `git reflog`查看命令历史。
  
  4. `git checkout -- <filrname>`命令让这个文件回退到最近一次`git commit`或者`git add`时的状态。
  
  5. `git rm <filename>`从版本库中删除这个文件。
  ---
  
- 远程仓库
  1. `ssh-keygen -t rsa -C "<邮箱地址>"`生成关于github的ssh key。
  
  2. `git remote add origin <ssh地址>`关联github远程库。
     
     1. 关联后，使用命令`git push -u origin master`第一次推送master上的所有内容。
        - 此后每次提交，就可以使用命令`git push origin master`推送最新修改。
        
        - 远程库的名字就是`origin`，Git的默认叫法。
     2. `git remote`查看远程库的信息。使用参`-v`查看更详细的信息。
  
  3. `git clone <仓库地址>`，克隆到本地库。
     
     - Git支持SSH协议，和https协议。
  
  4. `git push origin <branch-name>`推送最新修改。
  ---
  
- 分支管理
  1. `git branch <branch-name>`新建分支。
     
     - 在提示没有该分支的跟踪信息时，使用`git branch --set-upstream-to=origin/<branch-name> <branch-nam>`将与远程仓库的分支链接。
  
  2. `git checkout <branch-name>`选择分支。
  
  3. `git checkout -b <branch-name>`新建分支并且选择这个分支。
     
     - `git checkout -b <branch-name> origin/<branch-name>`创建远程仓库地址的分支到本地。
  
  4. `git branch`列出所有分支。
  
  5. `git merge <branch-name>`将该分支内容合并到当前分支上。
     
     - `git merge --no-ff -m "<message>" <分支名>`，加上`--no-ff`参数就可以使用普通模式合并分支，合并后有历史记录。
  
  6. `git branch -d <branch-name>`删除该分支。
     
     - `git branch -D <branch-name>`强行删除分支。
  
  7. `git stash`可以把当前未提交的工作暂时“储存”在堆栈缓存中。
     
     - `git stash save "<message>"`，附带提示信息。
     
     - `git stash list`查看“储存”的工作。
     
     - `git stash apply`可以恢复内容，但是并不会删除，需要使用`git stash drop`来删除。
     
     - `git stash pop`恢复的同时并删除。
  
  8. `git rebase`优化提交历史，把本地未push的分叉提交历史整理为直线，使得我们查看跟容易，缺点是本地的分叉会被修改。
  ---
  
- 标签管理
  1. `git tag`查看所有标签。
     
     - `git tag <tag-name>`新建一个标签，默认打在最近的一次commit上。
     
     - `git tag <tag-name> <commit-id>`，为commit id的提交打上标签。
     
     - `git tag -a <tag-name> -m "<message>" <commit-id>`，打上带有说明的标签。
     
     - `git tag -d <tag-name>`，删除本地的标签。
  
  2. `git show <tag-name>`，查看标签的详细信息。
  
  3. `git push origin <tag-name>`，将标签推送到远程仓库。
     
     - `git push origin --tags`，提交所有未推送的标签。
  
  4. 删除标签
     
     - `git tag -d `，先删除本地的标签。
     
     - `git push origin :refs/tags/<tag-name>`，删除远程的标签。
  ---
	 
- 自定义Git
  1. 使用`git config --global alias.<别名> 'git命令'`给命令起别名。
  
  2. 
