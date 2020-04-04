# pip3 报错：pip is being invoked by an old script wrapper.

这个问题的原因是不正确的安装。pip的升级部分有的文章属于 CSDN 博主的误导，在这点上我也吃了亏，特别写这一篇文章提醒大家。碰到问题多搜索，不要听信一家之言。要不然很容易把自己的工作进度搞崩。



这类问题的issue在github上，链接地址：[https://github.com/pypa/pip/issues/5599](https://github.com/pypa/pip/issues/5599)



## 对这篇issue开始分析：

>After upgrading to pip 10 or higher, many users are encountering error like
>
>- `ImportError: cannot import name 'main'`
>- [`TypeError: 'module' object is not callable`](https://github.com/pypa/pip/issues/5599#issuecomment-541960822)
>- [`ImportError: No module named _internal`](https://github.com/pypa/pip/issues/5253#issue-314720523)
>- [`ModuleNotFoundError: No module named 'pip._internal'`](https://github.com/pypa/pip/issues/5373#issue-320586093)
>
>These are caused by an incorrect attempt to upgrade pip, which has typically resulted in (parts of) multiple versions of pip being installed in the same Python installation, and those parts being incompatible.

文中提到以上的错误原因是不正确的pip升级导致。这些升级使得在一个python中有了多种pip的版本从而导致不兼容。

我们继续：

>It should be noted that these issues are invariably *not* problems with pip itself, but are due to incorrect use of pip, or unexpected interactions with system scripts that are not controlled by pip. So while we'll try to help you solve your issue, this is *not* the "fault" of pip, and you will have to be prepared to do at least some of the debugging and fixes on your own.

但是这些问题并不是出自pip它本身。而是使用不规范或不被pip控制的系统脚本的意外交互所致。所以我们帮你一起解决。



## 问题的建议

>## General Advice
>
>First, some general advice. It is assumed that anyone encountering issues will have failed to follow some part of this advice. Listing these items here is not intended to imply that "it's your fault and we won't help", but rather to help users to identify what went wrong, so that they can work out what to do next more easily.
>
>1. **Only ever use your system package manager to upgrade the system pip**. The system installed pip is owned by the distribution, and if you don't use distribution-supplied tools to manage it, you will hit problems. Yes, we know pip says "you should upgrade with pip install -U pip" - that's true in a pip-managed installation, ideally distributions should patch this message to give appropriate instructions in the system pip, but they don't. We're working with them on this, but it's not going to happen soon (remember, we're looking at cases where people are upgrading old versions of pip here, so patches to new versions won't help).
>2. **Never use sudo with pip**. This follows on from the first point. If you think you need to use sudo, you're probably trying to modify a distribution-owned file. See point 1.
>3. **Prefer to use `--user`**. By doing this, you only ever install packages in your personal directories, and so you avoid interfering with the system copy of pip. But there are `PATH` issues you need to be aware of here. We'll cover these later. Put simply, it's possible to follow this advice, and still hit problems, because you're not actually running the wrapper you installed as `--user`.

首先，是建议:

第一条：

**只用系统管理包去升级系统的`pip`**

系统的包是发行版，不使用系统发行版提供的工具去管理就会发生问题。

第二条：

**不要`sudo` 和`pip`一起用**，这会改变系统的`pip`发行版本

第三条：

**积极使用`--user`**，这样就可以在个人目录下安装软件包。



## 问题的调试

>## Debugging the Issue
>
>Before trying to work out what's going on, it's critically important that you understand precisely what scripts you are running and what versions of pip the relevant Python interpreter is using.
>
>First, identify the full absolute path of the executable script that you are running. That's often in the traceback you get, but if it isn't, you can use OS tools like `which`, or Python's `shutil.which` to identify the right script. Watch out for your shell confusing the issue, with aliases or potentially stale hashed commands.
>
>Once you have identified the script, make sure you can reproduce the problem using the absolute path to that script. If you can't, you probably found the wrong script, so check again.
>
>Second, work out which version of Python the script is using to run pip. You'll often be able to get that from the shebang line of the script. This can often be the trickiest problem, as wrapper scripts can take many forms depending on what tool created them. In the worst case, you can simply make an intelligent guess at this point.
>
>Once you know which Python is running pip, you can run `python -m pip` to invoke pip. In most cases, this will work fine, as it's the wrapper causing the issue, and not pip itself. Running pip via `python -m` in this way is often a perfectly acceptable workaround for the issue (at least in the short term).
>
>Now run `python -m pip --version`. This will give you the exact version and location of the installation of pip that your Python is seeing.
>
>At this point, you're usually done - the fundamental cause of *all* these problems is running a wrapper script which is written expecting to see a version of pip older than pip 10 (that's why it imports `pip.main`) under a Python interpreter that sees a copy of pip that's version 10 or later.

先要确定你使用的`python`和`pip`的版本以及路径。可以使用`which`等命令去定位出错pip所在的脚本。

脚本确定后，再看看这种情况能否复现。再确定知道是哪个版本的`python ` `pip`，这个信息一般可以在错误的提示找到。以上的问题全都确定后，可以使用`python - m pip`去调用`pip`，多数情况下是很有效的。在短期内使用`python -m pip`是解决这个问题的好办法。

现在运行`python -m pip`可以看到`python` `pip`的安装位置。

导致这个问题出现的原因是：**运行了包装器脚本**，这个脚本是用来查看是否有比`pip 10`更老的`pip`包来判断`pip`的新旧。



## 问题的解决

>## Fixing the Issue
>
>The problem, of course, is fixing the issue. And that's where you really are on your own. If you have changed your system installation, you really need to put it back the way that the distribution installed it. That may well require an uninstall and reinstall of your distribution's pip package. Reinstalling is easy, of course - but uninstalling may require manually removing incorrect files. Or you may be able to force-reinstall with your distribution package manager, simply overwriting the incorrect files. Of course, this reverts you to the system-supplied version of pip. If you need a newer version, you should ask your distribution vendor, or use something like virtualenv to install it independently of your system packages.
>
>It may be that you're simply running the "wrong" wrapper script. Maybe you did a `--user` install of a new version of pip, but your PATH is set to run the system version of the wrapper rather than the user-local one installed with pip. In that case, you can simply fix your PATH. That's usually the issue for people who do `pip install --user --upgrade pip` and get the `pip.main` error.
>
>As already noted, `python -m pip` is a reliable workaround, at the cost of using a more verbose command to invoke pip.
>
>

最后我们来解决问题：

如果是你自己改了系统的`pip`包。你需要把系统的`pip`包还原。如果实在需要升级，使用虚拟环境`virtualenv`等进行安装。

如果是使用的系统原装的`pip`运行了脚本.尽管安装了新的`pop`，但是没有使用新的`pip`运行。那就可能是你的`PATH`设置不正确，修复`PATH`即可。这一情况通常是因为使用`pip install --user --upgrade pip`，并得到了`pip.main`错误

最后，`python -m pip`是一种可靠的办法，但是是以加了`-m`为代价的。





个人建议：

可以使用：

```shell
python3 -m pip uninstall pip setuptools wheel
sudo apt-get --reinstall install  python3-setuptools python3-wheel python3-pip
```

试试修复回系统版本