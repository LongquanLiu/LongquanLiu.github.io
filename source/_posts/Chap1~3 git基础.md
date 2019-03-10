---
title: pro git
date: 2019-3-10
tags: Git
---

## 1.1 关于版本控制

* 本地版本控制系统(VCS)：简单数据库记录文件的差异（补丁集）；**查阅恢复特定版本**  
* 集中化的版本控制系统(CVCS)：单一的一个集中管理服务器，客户端通过连接服务器，取出最新的 *文件* 或者提交更新；**多个系统协同工作**  
* 分布式版本控制系统(DVCS)：客户端不仅提取最新版本快照，而把整个代码仓库完整镜像下来；**任一服务器宕机亦可恢复**

## 1.2 git简史

* 91-02年，提交补丁，归档；02-05年，BitKeeper（商用）；05年-至今，Git

## 1.3 git基础

* Git与其它版本控制系统(CVS/Subversion/Perforce)区别是：
  1. 直接记录快照，而非差异比较：  
    other:存储一组文件与初始版本随时间累积的 *差异*；
    Git：存储项目随时间改变的 *快照*，like小型文件系统的一组快照，保存全文件快照的 *索引*，未修改旧文件，直接用链接，新文件则存储并建立链接。
  2. 近乎所有操作都在本地执行：不需连接服务器去获取历史，离线工作
  3. Git保证完整性：所有数据存储前都计算校验和，机制是基于文件内容或目录结构计算SHA-1 Hash（40个十六进制字符）组成字符串；Git数据库中保存的信息都是以 *文件内容的Hash值来索引，而非文件名*

* Git三种状态与三个工作区域
  1. 三个工作区域：本地工作目录(Working Directory)/暂存区域(Staging Area)/Git仓库(git directory/Repository)  
  2. 三种状态：已修改(modified)/已暂存(staged)/已提交(committed)  
  工作目录是对项目的某个版本独立提取出来的内容;Git仓库目录是Git用来保存项目的元数据和对象数据库的地方;暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中;

## 1.6 Git配置

* 初始配置：  
  检查配置信息：$git config --list;
  设置你的用户名称与邮件地址：$git config --global user.name "LongquanLiu"; $git config --global user.email 773970134@qq.com

## 2.1 获取Git仓库  

1. 取得Git仓库的方法(git init/git clone)            
  1. 在现有目录中初始化仓库
  ```git
  $git init    //创建一个.git的子目录，含有初始化Git仓库中所有的必须文件
  $git add *.c     //对指定文件的跟踪
  $git commit -m 'initial project version'     //提交
  ```   
  2. 克隆现有的仓库  
  ```git
  $git clone https://github.com/LongquanLiu/LongquanLiu.github.io.git name  //将Git仓库中的每个文件的每个版本都拉取下来(Default)
  ps:git支持 https://  git:// user@server:path/to/repo.git(SSH) 协议
  ```    

## 2.2 记录每次更新到仓库

1. 检查当前文件状态/跟踪新文件/暂存已修改文件(git status/git add)
  ```git
  $git status
  On branch master

  nothing to commit, working directory clean    //所有已跟踪文件在上次提交后都未被更改过；没有任何处于未跟踪状态的新文件

  - 创建一个新的 README 文件
  Untracked files:README
  nothing added to commit but untracked files present     //未跟踪的文件，意味之前的快照（提交）中没有这些文件，不自动跟踪，使一些生成的二进制文件不会包含进来

  - 跟踪新文件
  $git add README
  &git status
  Changes to be committed:    //说明已暂存状态
   new file:   README

  - 修改CONTRIBUTING.md的已被跟踪的文件
  $git status
  Changes to be committed:    //说明已暂存状态
   new file:   README
  Changes not staged for commit:  //说明已跟踪文件的内容发生变化，但未放到暂存区，要暂存需要git add
    modified:   CONTRIBUTING.md
  ps: git add 是个多功能命令：可跟踪新文件；把已跟踪文件放到暂存区；合并时把有冲突的文件标记为已解决状态；可理解为‘添加内容到下一次提交中’

  - 将"CONTRIBUTING.md"放到暂存区
  $ git add CONTRIBUTING.md
  $ git status
  Changes to be committed:  //两个文件都已暂存
    new file:   README
    modified:   CONTRIBUTING.md

  - 在CONTRIBUTING.md里再加条注释，重新编辑存盘后
  $ git status
  Changes to be committed:    //此CONTRIBUTING.md是最后一次运行git add的版本
    new file:   README
    modified:   CONTRIBUTING.md
  Changes not staged for commit:
    modified:   CONTRIBUTING.md
  ps:有做修订，需重新运行 git add 把最新版本重新暂存起来
  ```    

2. 忽略文件(.gitignore)
  ```git
  $cat .gitignore
  *.[oa]    //忽略所有以 .o 或 .a 结尾的文件
  *~    //忽略所有以波浪符（~）结尾的文件
  ps:创建名为 .gitignore 的文件，列出要忽略的文件模式
  ```
  文件 .gitignore 的格式规范如下：
  所有空行或者以＃开头的行都会被Git忽略；可以使用标准的glob模式匹配；匹配模式可以以（/）开头防止递归；匹配模式可以以（/）结尾指定目录；要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。   

3. 查看已暂存和未暂存的修改(git diff)
  ```git
  $git diff  // 查看尚未暂存的文件改动（本地工作目录与暂存区域快照间的差异）
  $git diff --cached(staged)    //查看已暂存文件改动（暂存区域快照与git仓库最新版本的差异）
  通过文件补丁的格式显示改变
  ```

4. 提交更新&跳过使用暂存区域(git commit)
  ```git
  $git commit -m "something you want to annotation"     //提交更新
  $git commit -a -m '...'     //自动把所有已经跟踪过的文件暂存起来一并提交
  ```
5. 移除文件(git rm)
  ```git
  $git rm ..md    //从暂存区域移除，连带从本地工作目录中删除，不再跟踪
  $git rm --cached README     //从暂存区域移除，文件依旧保存在磁盘
  ```
6. 移动文件(git mv)
  ```git
  $ git mv README.md README     //对文件改名
  =
  $ mv README.md README
  $ git rm README.md
  $ git add README
  ```

## 2.3查看提交历史(git log)

  ```git
  git log     //SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明
  git log -p -2     // -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交
  ```

## 2.4撤销操作
  1. 提交完发现漏掉了几个文件没有添加，或者提交信息写错(git commit --amend)
  ```git
  $ git commit -m 'initial commit'
  $ git add forgotten_file
  $ git commit --amend    //最终你只会有一个提交 - 第二次提交将代替第一次提交的结果
  ```

  2. 取消暂存的文件(git reset HEAD <file>)
  ```git
  git reset HEAD *.md     //取消暂存
  ```

  3. 撤消对文件的修改(git checkout -- <file>)
  ```git
  git checkout -- *.md    //撤销修改，还原成上次提交时的样子
  ps:git checkout -- [file] 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。
  如果你仍然想保留对那个文件做出的修改，但是现在仍然需要撤消，我们将会在 Git 分支 介绍保存进度与分支；这些通常是更好的做法。
  ```

## 2.5 远程仓库的使用

  1. 查看远程仓库
  ```git
  git remote -v     //显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL（需在本地工作目录下）.
  git remote show origin    //查看特定仓库
  ```

  2. 添加远程仓库
  ```git
  git remote add pandasync https://github.com/LongquanLiu/PandaSync     //运行 git remote add <shortname> <url> 添加一个新的远程 Git 仓库，使用字符串 pandasync 来代替整个 URL
  ```

  3. 从远程仓库中抓取与拉取
  ```git
  git fetch [remote-name]     //会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作
  ```

  4. 推送到远程仓库
  ```git
  git push [remote-name] [branch-name]    //只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送
  ```

  5. 远程仓库的移除与重命名
  ```git
  git remote rename pandasync pandasync2    //重命名
  git remote rm pandasync     //删除
  ```

## 2.6 打标签

  1. 列出标签
  ```git
  git tag
  git tag -l 'v1.8.5*'
  ```

  2. 创建标签
  ```git
  git tag -a v1.4 -m 'my version 1.4'     //附注标签
  ps:附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息
  git show v1.4     //命令可以看到标签信息与对应的提交信息
  git tag v1.4-lw   //轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。
  ```

  3. 共享标签
  ```git
  git push origin [tagname]
  git push origin --tags    //把所有不在远程仓库服务器上的标签全部传送。
  ```

  4. 删除标签
  ```git
  git tag -d <tagname>
  git push <remote> :refs/tags/<tagname>     //更新你的远程仓库（删除了标签）。
  ```

## 2.7 Git别名

  ```git
  $ git config --global alias.co checkout
  $ git config --global alias.br branch
  $ git config --global alias.ci commit
  $ git config --global alias.st status

  eg:
  git config --global alias.unstage 'reset HEAD --'
  既有:$ git unstage fileA
  $ git reset HEAD -- fileA     //等价

  git config --global alias.last 'log -1 HEAD'
  git last    //看到最后一次提交
  ```

## 3.1 分支简介

1. Git仓库一般包含：提交对象；树对象；blob对象；
    > 提交对象：会包含一个指向暂存内容快照的指针。 还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针（可能有多个，如多分支合并）；   
    树对象：记录着目录结构和blob对象索引   
    blob对象：保存着文件快照   

    ps:git分支本质仅仅是指向提交对象的可变指针

2. 分支创建(git branch)
  ```git
  $ git branch testing    //在当前所在的提交对象上创建一个指针
  ps:创建了一个可以移动的新的指针；HEAD 指向当前所在的分支
  git log --oneline --decorate    //git log 命令查看各个分支当前所指的对象
  ```

3. 分支切换(git checkout)
  ```git
  git checkout testing    //此时HEAD 就指向 testing 分支
  若此时对文件修改提交，则testing分支向前移动
  git checkout master   //一是使 HEAD 指回 master 分支，二是将工作目录恢复成 master 分支所指向的快照内容。
  ps:branch,checkout,commit;
  $ git log --oneline --decorate --graph --all //查看项目分叉历史
  优点：由于分支实质上是指向提交对象的可变指针（所指对象校验和（长度为 40 的 SHA-1 值字符串））文件，只需写入40字符和1换行符---简单快
  ```

## 3.2 分支的新建与合并

* 新建分支/分支合并/遇到冲突时的分支合并
  ----------新建分支
  ```git
  $ git checkout -b iss53     //新建分支iss53并同时切换到iss53分支上
  Switched to a new branch "iss53"
  ...     //modified ..++
  $ git checkout master
  Switched to branch 'master'
  $ git checkout -b hotfix    //新建分支hotfix并同时切换到hotfix分支上
  Switched to a new branch 'hotfix'
  ...     //modified ..++
  $ git checkout master
  $ git merge hotfix    //当你试图合并两个分支时，如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候，只会简单的将指针向前推进（指针右移）,合并操作没有需要解决的分歧Fast-forward
  ...Fast-forward...
  $ git branch -d hotfix    //删除分支hotfix
  ----------分支合并
  $ git checkout iss53
  Switched to branch "iss53"
  ...     //modified ..++
  $ git checkout master
  Switched to branch 'master'
  $ git merge iss53         //非直接祖先，Git 会使用两个分支的末端所指的快照（C4 和 C5）以及这两个分支的工作祖先（C2），做一个简单的三方合并（有两个父指针）。
  $ git branch -d iss53     //删除分支iss53
  ----------遇到冲突时的分支合并
  // 你对 #53 问题的修改和有关 hotfix 的修改都涉及到同一个文件的同一处，在合并它们的时候就会产生合并冲突
  $ git checkout master
  Switched to branch 'master'
  $ git merge iss53     // Git 做了合并，但是没有自动地创建一个新的合并提交,需处理完冲突手动提交
  $ git status      //查看那些因包含合并冲突而处于未合并（unmerged）状态的文件
  //  HEAD/master 所指示的版本（======= 的上半部分）iss53 分支所指示的版本在 ======= 的下半部分;冲突全部解决之后git add然后commit，最后删除多余分支
  ps:git mergetool  //图形化工具解决冲突
  ```

## 3.3 分支管理

  ```git
  $ git branch    //当前所有分支的一个列表; * 字符,代表当前 HEAD 指针所指向的分支
  $ git branch -v     //查看每一个分支的最后一次提交
  $ git branch --merged     //已经合并到当前分支的分支,该列表除*以外的分支通常可用 git branch -d 删除掉
  $ git branch --no-merged    //未合并到当前分支的分支；git branch -d 命令删除它时会失败，unless -D
  ```

## 3.4 分支开发工作流

  长期分支：master/develop/topic   //渐进稳定分支
  特性分支: 短期分支，它被用来实现单一特性或其相关工作。eg:上述分支

## 3.5 远程分支

## 3.6 变基

  Git 整合不同分支有两种方法：merge(三方合并) 以及 rebase
  rebase: 你可以提取在 C4（分支祖先c2）中引入的补丁和修改，然后在 C3 （分支祖先c2）的基础上应用一次
  ps:合并结果没有区别，只是变基使得提交历史更加整洁

参考文献：[Pro Git](https://git-scm.com/book/zh/v2)
