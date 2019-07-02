## Git 基本操作

- [`git add`](#git-add)
- [`git commit`](#git-commit)
- [`git reset`](#git-reset)
- [`git merge`](#git-merge)
- [`git status`](#git-status)
- [`git log`](#git-log)
- [`git tag`](#git-tag)

#### git add

加入暂缓区1

#### git commit

提交暂缓区文件

```
git commit --amend

```

#### git reset

用来取消提交到暂存区

```
git reset HEAD CONTRIBUTING.md
#虽然在调用时加上 --hard 选项可以令 git reset 成为一个危险的命令
#（译注：可能导致工作目录中所有当前进度丢失！），但本例中工作目录内的文件并不会被修改。
#不加选项地调用 git reset 并不危险 — 它只会修改暂存区域。
```

#### git checkout

用来取消对文件的所有修改

```
git checkout -- [file]
```

#### git merge

#### git status

#### git log

你可以简单地使用 git log 命令查看各个分支当前所指的对象。 提供这一功能的参数是 --decorate。

```
git log --oneline --decorate
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

推送到远程仓库

```
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

```
git branch testing
```

#### git checkout

切换分支

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
