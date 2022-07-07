# git常用命令

## git设置用户名邮箱

```
git config --global user.name "vnightray"
git config --global user.email "vessalius_oz@163.com"
```





## git init——初始化仓库

###初始化仓库

```
$ mkdir git_test  #当前路径下创建一个文件夹git_test
$ cd git_test  #进入git_test文件夹
$ git init  #初始化仓库
```



## git status——查看仓库的状态

###查看仓库的状态

```
$ git status
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
```



```
$ touch README.md  #创建一个文件叫README.md
$ git status  #查看仓库状态
# On branch master
#
# Initial commit
## Untracked files:# (use "git add <file>..." to include in what will 
be committed)#
# README.md
nothing added to commit but untracked files present (use "git add" to 
track)
```



## git add——将文件加入暂存区

###将文件加入暂存区

```
$ git add README.md
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
# (use "git rm --cached <file>..." to unstage)
#
# new file: README.md
#
```



## git commit——保存仓库的历史记录 

###保存仓库的历史记录

将当前暂存区中的文件实际保存到仓库的历史记录中。



```
$ git commit -m "First commit"
[master (root-commit) 9f129ba] First commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

-m 参数后的“” 内容称作提交信息，是对提交内容的概述

若想记录详细信息，则输入git commit

在跳出的编辑器中记述提交信息的格式如下：

第一行：用一行文字简述提交的更改内容

第二行：空行

第三行：记述更改的原因和详细内容

以“#”开头的是注释内容



## git log——查看提交日志

###查看提交日志



查看简述信息

```
$ git log --pretty=short
commit 9f129bae19b2c82fb4e98cde5890e52a6c546922
Author: hirocaster <hohtsuka@gmail.com>
 First commit
```



###只显示指定目录、文件的日志

在git log命令后加上目录名，便会只显示该目录下的日志。如果是文件名，则只显示与该文件相关的日志。



###显示文件的改动

```
$ git log -p
```



## git diff——查看更改前后的差别

###查看更改前后的差别

```
$ git diff
diff --git a/README.md b/README.md
index e69de29..cb5dc9f 100644
--- a/README.md
+++ b/README.md
@@ -0,0 +1 @@
+# Git教程
```



###查看工作树和最新提交的差别

```
$ git diff HEAD
diff --git a/README.md b/README.md
index e69de29..cb5dc9f 100644
--- a/README.md
+++ b/README.md
@@ -0,0 +1 @@
+# Git教程
```

这里的HEAD是指当前分支中最新一次提交的指针。



## git branch——显示分支一览表

###显示分支一览表

git branch命令可以将分支名列表显示，同时确认当前所在分支。

```
$ git branch
* master
```

master分支左侧标有“*”，表示这是我们当前所在的分支。



## git checkout——分支

###git checkout -b  ——创建、切换分支

执行下面的命令，创建名为feature-A的分支

```
$ git checkout -b feature-A
Switched to a new branch 'feature-A'
```

以上命令等同于

```
$ git branch feature-A
$ git checkout feature-A
```



###切换分支至master

```
$ git checkout master
Switched to branch 'master'
```



###切换回上一分支

```
$ git checkout -
Switched to branch 'feature-A'
```



##特性分支

特性分支，是几种实现单一特性（主题）除此之外不尽兴任何作业的分支。

在日常开发中，旺旺或创建数个特性分支， 同时在此之外在保留一个随时可以发布软件的稳定分支。稳定分支的角色通常由master分支担当。



## 主干分支

主干分支是特性分支的原点，同时也是合并的重点。通常人们会把master分支作为主干分支。



## git merge——合并分支

将feature-A合并到主干分支master中，首先切换到master

```
$ git checkout master
Switched to branch 'master'
```

为了在历史记录中明确记录下本次分之合并，需要创建合并提交。因此，在合并时加上--no-ff参数

```
$ git merge --no-ff feature-A
```

合并完成显示如下结果：

```
Merge made by the 'recursive' strategy.
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```



## git log --graph——以图表形式查看分支

以git log --graph 命令进行查看的话，能很清楚地看到特性分支（feature-A）提交的内容已被合并。

```
$ git log --graph
* commit 83b0b94268675cb715ac6c8a5bc1965938c15f62
|\ Merge: fd0cbf0 8a6c8b9
| | Author: hirocaster <hohtsuka@gmail.com>
| | Date: Sun May 5 16:37:57 2013 +0900
| |
| | Merge branch 'feature-A'
| |
| * commit 8a6c8b97c8962cd44afb69c65f26d6e1a6c088d8
|/ Author: hirocaster <hohtsuka@gmail.com>
| Date: Sun May 5 16:22:02 2013 +0900
|
| Add feature-A
|
* commit fd0cbf0d4a25f747230694d95cac1be72d33441d
| Author: hirocaster <hohtsuka@gmail.com>
| Date: Sun May 5 16:10:15 2013 +0900
|
| Add index
|
* commit 9f129bae19b2c82fb4e98cde5890e52a6c546922
 Author: hirocaster <hohtsuka@gmail.com>
 Date: Sun May 5 16:06:49 2013 +0900
 First commit
```



## git reset ——回溯历史版本

要让仓库的HEAD、暂存区、当前工作树回溯到指定状态，需要用到git reset --hard命令。需要提供目标时间点的哈希值。

```
$ git reset --hard fd0cbf0d4a25f747230694d95cac1be72d33441d
HEAD is now at fd0cbf0 Add index
```



### git reflog——查看当前仓库的操作日志

```
$ git reflog
4096d9e HEAD@{0}: commit: Fix B
fd0cbf0 HEAD@{1}: checkout: moving from master to fix-B
fd0cbf0 HEAD@{2}: reset: moving to fd0cbf0d4a25f747230694d95cac1be72d33441d
83b0b94 HEAD@{3}: merge feature-A: Merge made by the 'recursive' strategy.
fd0cbf0 HEAD@{4}: checkout: moving from feature-A to master
8a6c8b9 HEAD@{5}: checkout: moving from master to feature-A
fd0cbf0 HEAD@{6}: checkout: moving from feature-A to master
8a6c8b9 HEAD@{7}: commit: Add feature-A
fd0cbf0 HEAD@{8}: checkout: moving from master to feature-A
fd0cbf0 HEAD@{9}: commit: Add index
9f129ba HEAD@{10}: commit (initial): First commit
```

注：即便开发者错误执行了git操作，基本也都可以利用git reflog命令回复到元宵的状态。



## 查看冲突并解决

```
# Git教程
<<<<<<< HEAD
 - feature-A
=======
 - fix-B
>>>>>>> fix-B
```

======== 以上的部分是当前HEAD的内容，一下的部分是要合并的内容。

冲突解决后，执行git add命令与git commit命令

```
$ git add README.md
$ git commit -m "Fix conflict"
Recorded resolution for 'README.md'.
[master 6a97e48] Fix conflict
```



##git commit --amend ——修改提交信息

要修改上一条提交信息，可以使用git commit --amend命令。





## git rebase -i ——压缩历史



```
$ git commit -am "Add feature-C"
[feature-C 7a34294] Add feature-C
 1 file changed, 1 insertion(+)
```

用git commit -am命令来一次完成add和commit命令，修正拼写错误。





### git rebase -i HEAD~2

```
$ git rebase -i HEAD~2
```

选定当初分支中包含HEAD（最新提交）在内的两个最新历史记录为对象，并在编辑器中打开。

```
pick 7a34294 Add feature-C
pick 6fba227 Fix typo
# Rebase 2e7db6f..6fba227 onto 2e7db6f
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```



```
pick 7a34294 Add feature-C
fixup 6fba227 Fix typo
```



结果显示：

```
$ git log --graph
* commit 51440c55b23fa7fa50aedf20aa43c54138171137
| Author: hirocaster <hohtsuka@gmail.com>
| Date: Sun May 5 17:07:36 2013 +0900
|
| Add feature-C
|
* commit 2e7db6fb0b576e9946965ea680e4834ee889c9d8
|\ Merge: 83b0b94 4096d9e
| | Author: hirocaster <hohtsuka@gmail.com>
| | Date: Sun May 5 16:58:27 2013 +0900
| |
| | Merge branch 'fix-B'
| |
| * commit 4096d9e856995a1aafa982aabb52bfc0da656b74
| | Author: hirocaster <hohtsuka@gmail.com>
| | Date: Sun May 5 16:50:31 2013 +0900
| |
| | Fix B
| |
```

这样一来，Fix typo就从历史中被抹去，也就相当于Add feature-C中从来没有出现过拼写错误。周三是一种良性的历史改写。



合并至master分支

```
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff feature-C
Merge made by the 'recursive' strategy.
 README.md | 1 +
 1 file changed, 1 insertion(+)
```



## git remote add——添加远程仓库

在GitHub上创建的仓库路径为“git@github.com:用户名/git-tutorial.git”

现在我们用git remote add命令将它设置成本地仓库的远程仓库。

```
$ git remote add origin git@github.com:github-book/git-tutorial.git
```

执行上述命令后，Git会自动将git@github.com:github-book/git-tutorial.git远程仓库的名称设置成origin（标识符）



## git push ——推送至远程仓库

### 推送至master分支

若想将当前分支下本地仓库中的内容推送给远程仓库，需要用到git push命令。

现在假定我们在master分支下进行操作：

```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (20/20), 1.60 KiB, done.
Total 20 (delta 3), reused 0 (delta 0)
To git@github.com:github-book/git-tutorial.git
 * [new branch] master -> master
Branch master set up to track remote branch master from origin.
```

像这样执行git push命令，当前分支的内容就会被推送给远程仓库origin的master分支。

-u参数可以在推送的同时，将origin仓库的master分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master分支获取内容。



### 推送至mater以外的分支

```
$ git checkout -b feature-D
Switched to a new branch 'feature-D'
```

除了master分支之外，远程仓库也可以创建其他分支。

我们在本地仓库中创建了feature-D分支，现在将它push给远程仓库并保持分支名称不变。

```
$ git push -u origin feature-D
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:github-book/git-tutorial.git
 * [new branch] feature-D -> feature-D
Branch feature-D set up to track remote branch feature-D from origin.
```



## git clone——获取远程仓库

```
$ git clone git@github.com:github-book/git-tutorial.git
Cloning into 'git-tutorial'...
remote: Counting objects: 20, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 20 (delta 3), reused 20 (delta 3)
Receiving objects: 100% (20/20), done.
Resolving deltas: 100% (3/3), done.
$ cd git-tutorial
```



执行git clone命令后我们会默认处于master分支下，同时系统会自动将origin设置成改远程仓库的标识符。也就是说，当前本地仓库的master分支与github端远程仓库（origin）的master分支在内容上是完全相同的。



```
$ git branch -a
* master
 remotes/origin/HEAD -> origin/master
 remotes/origin/feature-D
 remotes/origin/master
```

用git branch -a 命令查看当前分支的相关信息。-a参数可以同时显示本地仓库和远程仓库的分支信息。



```
$ git checkout -b feature-D origin/feature-D
Branch feature-D set up to track remote branch feature-D from origin.
Switched to a new branch 'feature-D'
```

将feature-D分支获取至本地仓库。

-b参数的后面是本地仓库新建分支的名称。为了便于理解，我们仍将其命名为feature-D，让它与远程仓库的对应分支保持同名。

例子中指定了origin/feature-D，就是说以名为origin的仓库的feature-D分支为来源，在本地仓库中创建feature-D分支。 





## git pull ——获取最新的远程仓库分支

```
$ git pull origin feature-D
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1)
Unpacking objects: 100% (3/3), done.
From github.com:github-book/git-tutorial
 * branch feature-D -> FETCH_HEAD
 First, rewinding head to replay your work on top of it...
 Fast-forwarded feature-D to ed9721e686f8c588e55ec6b8071b669f411486b8.
```

