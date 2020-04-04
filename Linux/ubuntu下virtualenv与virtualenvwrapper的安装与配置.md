# ubuntu下virtualenv与virtualenvwrapper的安装与配置

[toc]

对`pipenv`,`pyenv`持观望态度。暂时还是使用`virtualenv`与`virtualenvwrapper`进行项目开发

本机`Python`版本是`Python3.7`，ubuntu 19.10

## 检查`pip3`镜像源

如果`pip3`没有设置国内镜像源，需要先设置一下，下面设置的是清华源：

```shell
pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## 安装`virtualenv`

```shell
pip3 install virtualenv
```

## 安装`virtualenvwrapper`

```shell
pip3 install virtualenvwrapper
```

如果`virtualenvwrapper`一直卡着不动，可以先试试安装一下`pbr`

```shell
pip3 install pbr
```

## 开始设置环境变量

```shell
sudo gedit /etc/profile
```

这样就用`gedit`打开了`etc/profile`

在末尾加上：

```shell
export WORKON_HOME=$HOME/.virtualenvs
```

这样设置的意思是，将虚拟环境统一放置在`$HOME/.virtualenvs`文件夹中

使之生效：

```shell
source /etc/profile
```



接下来继续设置

## bash版本

如果你用的是默认的bash shell，那么打开`~/.bashrc`

```shell
sudo gedit ~/.bashrc
```

在文件末尾加入：

```shell
source ~/.local/bin/virtualenvwrapper.sh
```

使之生效：

```shell
source ~/.bashrc
```



## zsh版本

如果你使用的是zsh，那么打开`~/.zshrc`

```shell
sudo gedit ~/.zshrc
```

在文件末尾加入：

```shell
source ~/.local/bin/virtualenvwrapper.sh
```

使之生效：

```shell
source ~/.zshrc
```



## 验证是否安装成功：

先输入`virtualenv`，有输出则成功

```shell
# mrcangye @ mrcangye in ~ [12:00:13] 
$ virtualenv
usage: virtualenv [--version] [--with-traceback] [-v | -q] [--app-data APP_DATA] [--clear-app-data] [--discovery {builtin}] [-p py] [--creator {builtin,cpython3-posix,venv}] [--seeder {app-data,pip}] [--no-seed]
                  [--activators comma_sep_list] [--clear] [--system-site-packages] [--symlinks | --copies] [--download | --no-download] [--extra-search-dir d [d ...]] [--pip version] [--setuptools version] [--wheel version] [--no-pip]
                  [--no-setuptools] [--no-wheel] [--symlink-app-data] [--prompt prompt] [-h]
                  dest
virtualenv: error: the following arguments are required: dest
```

输入`mkvirtualenv`，有输出则成功

```shell
# mrcangye @ mrcangye in ~ [12:00:16] C:2
$ mkvirtualenv     
usage: virtualenv [--version] [--with-traceback] [-v | -q] [--app-data APP_DATA] [--clear-app-data] [--discovery {builtin}] [-p py] [--creator {builtin,cpython3-posix,venv}] [--seeder {app-data,pip}] [--no-seed]
                  [--activators comma_sep_list] [--clear] [--system-site-packages] [--symlinks | --copies] [--download | --no-download] [--extra-search-dir d [d ...]] [--pip version] [--setuptools version] [--wheel version] [--no-pip]
                  [--no-setuptools] [--no-wheel] [--symlink-app-data] [--prompt prompt] [-h]
                  dest
virtualenv: error: the following arguments are required: dest
```

输入`rmvirtualenv`，有输出则成功

```shell
# mrcangye @ mrcangye in ~ [12:00:25] C:2
$ rmvirtualenv     
Please specify an environment.
```



