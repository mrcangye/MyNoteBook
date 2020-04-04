# Git 学习历程(一)Git简单使用



[toc]

## Git的安装与更新

如果操作系统里预先有`git`，那么我们可以使用以下命令查看`git`版本：

```bash
git --version
```

```bash
$ git --version
git version 2.20.1
```

上面显示我的版本是`2.20.1`，我们需要更新一下

接下来是`git`的安装与更新

```bash
sudo apt update  # 更新源
sudo apt install software-properties-common # 安装 PPA 需要的依赖
sudo add-apt-repository ppa:git-core/ppa    # 向 PPA 中添加 git 的软件源
```

软件源安装好之后，我们还要更新一下源，才能继续安装`git`

```bash
sudo apt-get update
sudo apt install -y git #安装Git
```

接下来再查看以下`git`的版本，是最新的就是安装成功

```bash
git --version
```

```bash
$ git --version              
git version 2.26.0
```

## Git个人信息的配置

```bash
git config --global user.email "XXXXXXXXXX"
git config --global user.name "XXXXX"
```

以上是设置个人信息，让`git`知道你是谁。`email`是你的`github`用来登录的邮箱地址。`name`是你的`github`用户名

我们可以使用`git config -l`查看自己的信息配置，按`q`退出。配置文件在仓库目录的`.gitconfig`文件中

## Git的克隆

首先我们要复制仓库的地址，复制下来后是一段网页链接，例如我的博客项目[https://github.com/mrcangye/Dblog.git](https://github.com/mrcangye/Dblog.git)

如果想要克隆它，那么可以使用`git clone`命令

```bash
git clone https://github.com/mrcangye/Dblog.git
```

```bash
$ git clone https://github.com/mrcangye/Dblog.git      
正克隆到 'Dblog'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
接收对象中: 100% (4/4), 完成.
```

等待克隆成功，项目就下载到你的电脑里了。

克隆一个 GitHub 仓库（也叫远程仓库）到电脑里后，本地仓库和远程仓库会自动关联执行，如果想查看本地仓库所关联的远程仓库信息，可以使用`git remote -v`命令。当然执行这个命令时，我们的当前目录应该是这个仓库的目录下。

```bash
# mrcangye @ mrcangye in ~/Github/Dblog on git:master x [19:09:35] C:1
$ git remote -v
origin	https://github.com/mrcangye/Dblog.git (fetch)
origin	https://github.com/mrcangye/Dblog.git (push)
```

## Git仓库的新建

`git init`命令是将当前的目录初始化为一个`git`仓库。emmm这个命令用得比较少

## Git仓库的区域

`Git`仓库有工作区，暂存区，版本区这三个区域

```mermaid
graph LR
	开发者--平时写代码-->工作区;
	工作区--git add-->暂存区;
	暂存区--git commit-->版本区;
	版本区--git push-->远程仓库;
	
```

平时对`git`的操作一般是以上的流程。

## 查看仓库的状态

如果需要查看仓库的状态，我们可以使用`git status`

如果我们想把所有的文件加入暂存区，可以使用`git add .`

如果不小心写错了，想撤销某个文件的修改怎么办？

```bash
git reset -- [文件名字]
```

或者

```bash
git rm --cached [文件名字]
```

例如我们想撤销`test.txt`文件的修改

```bash
git reset -- test.txt
```

或者

```bash
git rm --cached test.txt
```

就可以啦

既然文件都修改好了。我接下来要看看文件的修改状态怎么办

```bash
git diff
```

可以查看工作区文件的修改详情，按`q`可以退出

那，我想看暂存区的修改详情呢？

那就用：`git diff --cached`



## Git的提交

文件都修改好了，可以使用`git commit`提交到远程仓库

```bash
git commit -m 'XXXXXXXXXX'
```

`XXXXX`里写的是今天提交备注，备注是自己写的，比如今天提交了一个bug，我们可以写：

```bash
git commit -m "i want a bug"
```

然后我们推送到远程仓库，使用以下命令

```bash
git push
```

在出现的提示框里输入你的账户名和密码后，就可以提交成功了

提交成功后，我们可以使用`git log`查看版本区提交的历史记录：

```bash
git log [分支名] 查看某分支的提交历史，不写分支名查看当前所在分支
git log --oneline 一行显示提交历史
git log -n 其中 n 是数字，查看最近 n 个提交
git log --author [贡献者名字] 查看指定贡献者的提交记录
git log --graph 图示法显示提交历史
```

也可以执行 `git branch -avv` 查看分支情况。

## 版本回退

如果因为某些原因想撤销刚刚的提交

首先执行 `git reset --soft HEAD^` 撤销最近的一次提交，将修改还原到暂存区。

`--soft` 表示软退回，对应的还有 `--hard` 硬退回

`HEAD^` 表示撤销一次提交， 

`HEAD^^` 表示撤销两次提交， 

 撤销 n 次可以简写为 `HEAD~n`

软退回后记得执行 `git branch -avv` 命令查看分支信息，确定退回符合预期



如果是比较古老的版本，可以使用

```bash
git reflog
```

这个命令是显示本地仓件所有分支的每次版本变化，同样按`q`退出。

接下来

```bash
git reset --hard [版本号]
```

或者

```bash
git reset --hard HEAD@{ } 
```

大括号里需要填`git relog`显示的版本序号

## 本地强制推送覆盖远程

```bash
git push -f
```

