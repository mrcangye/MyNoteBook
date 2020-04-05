# Git 学习历程(二)Git分支操作

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

`git fetch`将远程仓库的**分支信息**下载到本地。下载好之后，可以使用`git brach -avv`查看。

值得注意的是，这个命令只是更新分支信息，并不会拉取最新的数据

例如：

```bash
# mrcangye @ mrcangye in ~/Documents/Github/MyNoteBook on git:master x [12:08:15] 
$ git branch -avv
* master                05f8502 [origin/master] add somethings
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 05f8502 add somethings
```

`git pull`拉取远程仓库的数据到本地，这个命令会拉取最新的数据到本地

```bash
# mrcangye @ mrcangye in ~/Documents/MyNoteBook on git:master x [12:20:46] 
$ git pull
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
展开对象中: 100% (3/3), 684 字节 | 228.00 KiB/s, 完成.
来自 https://github.com/mrcangye/MyNoteBook
   05f8502..4631b57  master     -> origin/master
更新 05f8502..4631b57
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 README.md
```

`git branch [分支名]`可以创建新的分支

`git checkout [分支名]`切换到某个分支

比如我们创建一个`testbranch`分支，并转到这个分支上

```bash
# mrcangye @ mrcangye in ~/Documents/Github/MyNoteBook on git:master x [12:23:56] 
$ git branch testbranch

# mrcangye @ mrcangye in ~/Documents/Github/MyNoteBook on git:master x [12:25:55] 
$ git checkout testbranch
D	"Linux/Git \345\255\246\344\271\240\345\216\206\347\250\213(\344\272\214)Git\347\256\200\345\215\225\344\275\277\347\224\250.md"
切换到分支 'testbranch'

```



 `git push [主机名] [本地分支名]:[远程分支名]` 将本地分支推送到远程仓库的分支中，通常冒号前后的分支名是相同的，如果是相同的，可以省略 `:[远程分支名]`如果远程分支不存在，会自动创建。

如果后面还是使用该分支，可以使用 `git branch -u [主机名/远程分支名] [本地分支名]` 将本地分支与远程分支关联。这样以后`push`就到该分支去了。取消的话 `git branch -u [主机名/远程分支名] [本地分支名]` 

如果想在推送时就跟踪远程分支：`git push -u [主机名/远程分支名] [本地分支名]`

删除远程的分支：`git push [主机名] :[远程分支名]`或`git push [主机名] --delete [远程分支名]`

删除多个远程分支：`git push [主机名] :[远程分支名] :[远程分支名] :[远程分支名]`看代码的格式就知道，所谓的删除分支其实是将空分支推送到远程分支，使得分支被删除

### 本地分支删除与改名

本地分支的删除`git branch -D [分支名]`

本地分支的改名`git branch -m [原分支名] [新分支名]`

注意删除的时候，不可以删除自己现在在的分支。比如自己是`testbranch`分支就不可以删除`master`分支，自残是不允许的：

```bash
$ git branch -D testbranch 
error: 无法删除检出于 '/home/mrcangye/Documents/Github/MyNoteBook' 的分支 'testbranch'。
```

```bash
$ git checkout master    
D	"Linux/Git \345\255\246\344\271\240\345\216\206\347\250\213(\344\272\214)Git\347\256\200\345\215\225\344\275\277\347\224\250.md"
切换到分支 'master'
您的分支与上游分支 'origin/master' 一致。

# mrcangye @ mrcangye in ~/Documents/Github/MyNoteBook on git:master x [12:27:49] 
$ git branch -D testbranch
已删除分支 testbranch（曾为 4631b57）。
```



## 标签管理

`git tag [标签名] -m [备注信息] [提交版本号]` 创建标签。其中 `-m [备注信息]` 可以省略不写，但建议不要省略。`[提交版本号]` 如果是给当前分支最新的提交创建标签，则可以省略。

`git tag`显示仓库中的全部标签列表

`git show [标签名]`显示标签详情

`git tag -d [标签名]`删除本地标签

`git push origin [标签名]`推送标签到远程仓库

`git push origin :refs/tags/[标签名]`删除远程仓库里的标签

## 签出版本

`git checkout [标签名]` 切换到之前的某个提交版本

然后执行 `git checkout -b [新的分支名]` 将此提交版本固定到一个新分支上并切换到此分支