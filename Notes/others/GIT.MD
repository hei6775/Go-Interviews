## Git 基本操作

- [`git add`](#git-add)
- [`git commit`](#git-commit)
- [`git reset`](#git-reset)
- [`git merge`](#git-merge)
- [`git status`](#git-status)
- [`git log`](#git-log)
- [`git tag`](#git-tag)
- [`git reflog`](#git reflog)
- [`git diff`](#git diff)
- [`git rm`](#git rm)
- [`git stash`](#git stash)

#### git add

加入暂缓区

#### git commit

提交暂缓区文件

```
git commit --amend

```

#### git reset

用来取消已经提交到暂存区的修改，但是修改完还需要使用`git checkout -- file`来取消工作区的修改

用来回滚版本

```
git reset HEAD CONTRIBUTING.md
git reset --hard commit_id
#虽然在调用时加上 --hard 选项可以令 git reset 成为一个危险的命令
#（译注：可能导致工作目录中所有当前进度丢失！），但本例中工作目录内的文件并不会被修改。
#不加选项地调用 git reset 并不危险 — 它只会修改暂存区域。
```

#### git checkout

切换分支

用来取消对文件的所有修改

```
git checkout -- [file]
```

`git checkout -- file`命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令

#### git merge

`git merge`命令用于合并指定分支到当前分支,合并完成后，就可以放心地删除 dev 分支了：

```bash
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
$ git branch -d dev
Deleted branch dev (was b17d20e).
#准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward
git merge --no-ff -m "merge with no-ff" dev
```

Git 用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

#### git status

#### git log

你可以简单地使用 git log 命令查看各个分支当前所指的对象。 提供这一功能的参数是 --decorate。

```bash
git log --oneline --decorate
#用带参数的git log也可以看到分支的合并情况
git log --graph --pretty=oneline --abbrev-commit
```

#### git tag

标签分为两种，轻量标签和附注标签。

```
git tag
git tag -l 'v1.8.5*' #只对v1.8.5的感兴趣
git tag -a v1.4 -m 'my version 1.4' #创建附注标签
git show v1.4-lw #查看v1.4-lw的标签信息
git tag -a [标签] [检验和]
git push origin --tags # 上传所以标签
git push origin [tagname] #上传指定标签
git tag -d v1.4-lw #删除指定标签
git push <remote> :refs/tags/<tagname> #将删除的指定标签更新到远程仓库中

```

#### git reflog

记录你的每一次命令

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

#### git diff

`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

`git diff readme.txt` 查看文件修改

#### git rm

在删除了工作区的文件后，Git 知道你删除了文件，因此，工作区和版本库就不一致了，git status 命令会立刻告诉你哪些文件被删除了

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令 git rm 删掉，并且 git commit

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git checkout -- test.txt
```

#### git stash

可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

```bash
git stash
#查看
git stash list
#恢复但是不会删除stash，需要使用git stash drop来删除
git stash apply
#恢复的同时把stash内容也删了
git stash pop
#你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令
git stash apply stash@{0}
```

## Git 操作远程仓库

- [`git clone`](#git-clone)
- [`git remote`](#git-remote)
- [`git fetch`](#git-fetch)
- [`git push`](#git-push)
- [`git merge`](#git-merge)

#### git clone

#### git remote

显示、添加、重命名、删除远程仓库

```
git remote -v
git remote add <shortname> <url>
git remote show [remote-name]
git remote rename [old-name] [target-name]
git remote rm [remote-name]
```

#### git fetch

从远程仓库中抓取与拉取

```
git fetch [remote-name]
```

#### git push

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支 master 推送到远程。

由于远程库是空的，我们第一次推送 master 分支时，加上了`-u`参数，Git 不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令。

```
git push -u origin master
git push [remote-name] [branch-name]
```

## Git 分支操作

- [`git branch`](#git-branch)
- [`git checkout`](#git-checkout)
- [`git rebase`](#git-rebase)

#### git branch

创建分支，不加参数则打印分支列表
常用参数-v 查看每一个分支的最后一次提交

--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。

```bash
git branch testing
#如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
#小写的d适用于必须合并过的分支
```

建立本地分支和远程分支的关联，使用

`git branch --set-upstream branch-name origin/branch-name`

#### git checkout

切换分支,加上`-b`参数表示创建并切换

```
git checkout testing
```

#### git rebase

变基操作

```
git rebase origin
git rebase --continue
git rebase --abort
```
