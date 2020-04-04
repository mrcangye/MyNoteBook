# Git 学习历程(二)Git

[toc]

## 给命令设置别名

Git 可以对这些命令设置别名，如果原命令中有选项，需要加引号。

```bash
git config --global alias.[别名] [原命令]
```

例如：

```bash
git config --global alias.st status
git config --global alias.br 'branch -avv'
git config --global alias.com 'commit -m'
```

如果自己设置的命令，哪天忘记了，可以使用`git config -l`查询，按`q`退出

## Git分支操作

### 远程分支

`git fetch`将远程仓库的**分支信息**下载到本地。下载好之后，可以使用`git brach -avv`查看

`git pull`拉取远程仓库的数据到本地

`git branch [分支名]`可以创建新的分支

`git checkout [分支名]`切换分支

 `git push [主机名] [本地分支名]:[远程分支名]` 将本地分支推送到远程仓库的分支中，通常冒号前后的分支名是相同的，如果是相同的，可以省略 `:[远程分支名]`如果远程分支不存在，会自动创建。

如果后面还是使用该分支，可以使用 `git branch -u [主机名/远程分支名] [本地分支名]` 将本地分支与远程分支关联。这样以后`push`就到该分支去了。取消的话 `git branch -u [主机名/远程分支名] [本地分支名]` 

如果想在推送时就跟踪远程分支：`git push -u [主机名/远程分支名] [本地分支名]`

删除远程的分支：`git push [主机名] :[远程分支名]`或`git push [主机名] --delete [远程分支名]`

删除多个远程分支：`git push [主机名] :[远程分支名] :[远程分支名] :[远程分支名]`看代码的格式就知道，所谓的删除分支其实是将空分支推送到远程分支，使得分支被删除

### 本地分支

本地分支的删除`git branch -D [分支名]`

本地分支的改名`git branch -m [原分支名] [新分支名]`