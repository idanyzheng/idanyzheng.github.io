# Git简明教程


# Git简明教程

## 前言

这不是开发手册，不是原理讲解。只会告诉你，这个功能是什么。

## 整体概述

Git 是一个版本控制系统，它是分布式的。

未追踪区：初始化之后，还么有进入追踪区

工作区：所有的修改，都会被自动记录在工作区当中，可以撤销修改，可以查询修改内容，可以删除刚才修改的内容。

暂留区：添加到追踪区之后，所有记录开始 手动更新到这里

## 安装与配置

#### 安装

https://git-scm.com/downloads

#### 配置

git config --global user.name "你的名字"

git config --global user.mail "你的邮箱"

--global ：此时，本机子上所有的 git 就会用这个信息

gitconfig --global color.ui true

别名：

```
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

配置文件在：.git/config 中，可以直接修改

## 一、版本管理

#### 开始追踪

开始追踪：将文件设置成需要版本管理的文件夹时，文件夹中的文件还未进行追踪。只有当加入（git add）追踪之后，所有的文件才开始进行追踪。

自动记录：git add 之后，所有的文件修改的内容，将会自动保存在 工作区（working directory )

保存记录：任意时候，再做一次git add，就会将工作区的内容清空，并且保存到暂存区(stage 或 index)

保存版本：git commit 操作可以将修改的内容 记录到版本库中。



![git简明教程1](F:\touzi\总汇\git简明教程1.jpg)

#### 忽略追踪

创建.gitignore文件，将要忽略追踪的文件名填进去

通配符：

```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore和App.class:
!.gitignore
!App.class
```

#### 记录修改

0	指定文件夹，进入版本管理： git init 

1	将需要追踪管理的文件，加入追踪 : 

​	git add 文件名

​	`参考操作：git add . （全部加入追踪）`   

​	`创建.gitignore文件，将要忽略追踪的文件名填进去`

2	当加入追踪之后，所有的文件的增、删、改都会在工作区（working directory )中进行记录

3	修改完成后，可将修改的记录，提交到暂存区(stage或index)

​	`git add 文件名`

​	`git add .`

4	根据实际情况，随时可以将暂存区的修改进行保存

​	`git commit -m “备注内容”`

​	`或者不进行暂存，直接保存修改记录：git commit -am “”`

#### 查看修改

1	查看整个文件夹下面的修改情况：`git status`

2	查看工作区与暂存区的差别：`git diff`

3	查看暂存区与仓库中最后一次保存的修改记录的差别： `git diff --staged`

4	查看工作区与仓库中最后一次保存修改记录的差别：`git diff HEAD` 

#### 查看版本历史

1	git log

2	git reflog 查看所有记录

3	git log --pretty=oneline --abbrev-commit 好看的格式

#### 恢复到历史版本

1	将暂存区的内容恢复到工作区（删除工作区的内容）git checkout -- 文件名

2	恢复版本库的内容到工作区 git reset HEAD -- 文件名

3	恢复版本库中的指定内容到工作区	`git reset --hard 版本号`

​		--hard：硬回滚。工作区、暂存区全部替换成指定的版本。

​		--mixed：默认。覆盖暂存区，不覆盖工作区。

​		--soft：软回滚。工作区、暂存区不覆盖。

查看版本库中的保存记录： `git reflog` 

#### 删除文件

当文件保存的版本库之后，就不用删除掉的文件都可以从版本库中恢复

`git rm 文件`

## 二、将版本号打上标签

总是寻找版本id，是不便于管理的，所以加上标签。就可以任意跳跃到相应记录了。

#### 打标签

1 给当前版本号打标签：git tag 标签名

2 给历史版本号打标签：git tag 标签名 版本短号

版本短号：`git log --pretty=oneline --abbrev-commit`

#### 查看标签

查看所有标签：git tag

查看当个标签的内容： git show 标签名

#### 删标签

如果打错标签可以删除 git tag -d 标签名



## 三、多分支版本管理

#### 分支的增删查并

查看分支： git branch

创建分支:	git branch 问文件名

切换分支： git switch <分支名>  或 git checkout -b 分支名

​					创建+切换：git checkout -b 文件名，或 git switch -c <分支名>

合并分支： git merge <分支名>, 一般是 先跳到主分支上，然后合并另一个分支

​					git 合并时，会用Fast forward 模式，但这种模式下，删除分治后，会丢掉分支信息

​					--no-ff，参数，就会禁止用Fast forward模式，merge时会生成一个新的commit，从分支历史上可以看出分支信息

删除分支	git branch -d <分支名>

​					没有被合并过的分支是删除不了的，用：git branch -D 分支名强行删除

查看分支历史：`git log --graph --pretty=oneline --abbrev-commit`

#### 分支管理一般策略

master是不能动，dev分支用来合并，个人分支来修改实际内容

#### bug分支与cherry-pick

1、有bug的时候，我们会在master 下面开一条分支 

​	新建、切换分支： git checkout -b issue-101(bug名)

​	修改完：git add 文件名

​	提交版本：git commit -m "issue-101"

在处理完bug之后，就会将这条分支合并到master，且删掉。

​	切换回master：git switch master

​	合并： git merge --no-ff -m "merge bug fix 101" issue-101（分支名）

这个时候再到dev分支的时候，不需要重新再做一次bug修改

只需将master 保存的 commit it 提取过即可。

​	git cheery-pick comm_it（前面提交issu的commit）

## 四、暂存管理

一次版本的提交，是一次完整的修改。

如果临时有bug需要紧急处理，但是手头工作还没做完，怎么办？

先将暂存区的内容进行保存

#### 查看保存的暂存区内容

​	git stash list

#### 保存暂存区

​	git stash 将暂存区的内容进行保存。每一次的git stash是一次压栈。可以保存多次的暂存区的内容。

#### 提取暂存区

​	git stash pop 恢复并删除

​	git stash apply 恢复，然后 git stash drop 删除

## 五 、同步远程库

#### 远程账号设置

1	生成ssh key： ssh-keygen -t rsa -C “你的邮箱@youxiang.com"

2	在.ssh目录下，找到id_rsa.pub(公钥)，上传到 ssh key里

#### 远程创建空库

Create a new repo

#### 链接本地远程库

git remote add origin git@github.com:i你的名字/你创建的库.git

origin: 就代表远程的库，这个名字可以自定义；origin 是约定俗称的。

建立远程的分支链接：

​	git checkout -b dev origin/dev

#### 上传本地库

将本地master分支推送到远程库 `git push -u origin master`

-u参数不但将本地master分支推送到远程master，并且建立了联系

普通修改，普通推送：git push origin master

上传分支： git push origin dev

#### 上传合并本地库到远程

#### 下载远程库

先有远程库 git clone git@github.com:你的名字/你的库.git

#### 下载合并远程库到本地

抓取远程分支：git pull

当有人已经先提交了 分支时，此时 git pull 就会出错(提示：no tracking information)，需要先建立链接

​	git branch --set-upstream-to=origin/dev

建立链接，再 git pull 时，就会成功，并且会提示有冲突，此时手动合并后，再提交

​	`git push origin dev`

#### 查看本地链接的远程库

查看本地对应链接远程库的名字：git remote

查看链接库的名字：git remote -v

#### 删除本地连接的远程库

git remote rm origin

## 六、其他

#### git rebase

将分支都直线化

