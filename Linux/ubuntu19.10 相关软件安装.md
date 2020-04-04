@[TOC](本文目录)

## 1.Typora

[Typora](https://www.typora.io/#linux)是一款Markdown撰写软件，Linux版本也相当好用.

Typora的安装方式如下：

```bash
# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository

sudo add-apt-repository 'deb https://typora.io/linux ./'

sudo apt-get update

# install typora

sudo apt-get install typora
```



## 2.zsh

搭配了[oh my zsh](https://ohmyz.sh/) zsh 是较为好用的shell工具

先下载zsh:

```bash
sudo apt-get install zsh
```

再安装oh my zsh

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

在对话框的设置zsh为默认bash选项，填y确认即可。

配置文件在用户目录.zshrsc文件中



## 3.Vscode

[vscode](https://code.visualstudio.com/docs/?dv=linux64_deb)是常用的编程软件，下载链接点击即可

## 4.gnome 美化

```bash
sudo apt-get update
sudo apt-get install gnome-tweak-tool
sudo apt-get install chrome-gnome-shell
```

用火狐打开[https://extensions.gnome.org](https://extensions.gnome.org)，点击网页提示安装拓展，安装后刷新界面

在网页的搜索框输入user themes，找到后点击

在user themes界面将网页的右边OFF点击为ON就可以啦

gnome主题还可以在[https://www.gnome-look.org](https://www.gnome-look.org)查找

## 5.Dash to Dock

用火狐打开[https://extensions.gnome.org](https://extensions.gnome.org)，网页的搜索框输入Dash to Dock，找到后点击

在Dash to Dock界面将网页的右边OFF点击为ON就可以

## 6.NetSpeed

同上，网页连接为[https://extensions.gnome.org/extension/104/netspeed/](https://extensions.gnome.org/extension/104/netspeed/)，类似以上方法安装即可

## 7.XDM下载器

这个下载器挺不错的。
[XDM](https://github.com/subhra74/xdm)下载后解压运行
`./install.sh`
安装完成后在软件列表就可以看到啦

## 8.system-monitor

[https://extensions.gnome.org/extension/120/system-monitor/](https://extensions.gnome.org/extension/120/system-monitor/)
这个也是gnome里的插件。用来显示系统内存和CPU等情况。
首先需要打开终端安装依赖：

```bash
sudo apt install gir1.2-gtop-2.0 gir1.2-nm-1.0 gir1.2-clutter-1.0
```

安装完依赖后，像之前的插件一样安装即可

------

## 9.gTile

[gTile](https://extensions.gnome.org/extension/28/gtile/)也是在gnome下的窗口分屏软件，十分好用。
使用简介可以参考这个[gTile](https://github.com/gTile)的Github.

> Make sure the window you want to resize has focus
> Click on the gTile icon on the tool bar, or press Super+Enter (default)
> The gTile dialog pop-up will show up in the center of your screen

意思就是按`window` + `Enter`就可以结合鼠标或者键盘使用啦

## 10.grub启动美化(不是双系统不建议使用)

我的是`window10`和`ubuntu 19.10`双系统，开启启动页面是那个`grub`没得选。所以我们可以进行美化。
主题在这里[下载](https://www.pling.com/s/gnome/browse/cat/109/order/latest/)
我的主题是[Tela](https://www.pling.com/s/gnome/p/1307852/)
![tela.jpg](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5jYW5neWUubWUvMjAyMC8wMy8xMy90ZWxhLmpwZw?x-oss-process=image/format,png)
下载好主题后，我们新建一个文件夹：

```shell
sudo mkdir /boot/grub/themes/
```

再将下载的主题压缩包解压到`themes`文件夹中
我们找到主题文件夹中的`theme.txt`文件，复制它的地址
接下来设定配置：

```shell
sudo gedit /etc/grub.d/00_header
```

写入这两个配置，路径和像素：

```shell
GRUB_THEME="/boot/grub/themes/Tela/theme.txt"
GRUB_GFXMODE="1920x1080"
```

如下(别为了美观乱加空格,等于号前后不要有空格)

```shell
#! /bin/sh
set -e

# grub-mkconfig helper script.
# Copyright (C) 2006,2007,2008,2009,2010  Free Software Foundation, Inc.
#
# GRUB is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# GRUB is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GRUB.  If not, see <http://www.gnu.org/licenses/>.


GRUB_THEME="/boot/grub/themes/Tela/theme.txt"
GRUB_GFXMODE="1920x1080"
```

接下来使得配置生效：

```shell
sudo update-grub
```

如果显示是这样：

```shell
Sourcing file `/etc/default/grub'
Sourcing file `/etc/default/grub.d/init-select.cfg'
Generating grub configuration file ...
Found theme: /boot/grub/themes/Tela/theme.txt
Found linux image: /boot/vmlinuz-5.3.0-40-generic
Found initrd image: /boot/initrd.img-5.3.0-40-generic
Found linux image: /boot/vmlinuz-5.3.0-18-generic
Found initrd image: /boot/initrd.img-5.3.0-18-generic
Found memtest86+ image: /boot/memtest86+.elf
Found memtest86+ image: /boot/memtest86+.bin
Found Windows 10 on /dev/sda1
done

```

那就是成功了

## 11.Clipboard Indicator

[剪切板](https://extensions.gnome.org/extension/779/clipboard-indicator/)插件，和windows下的`Ditto`类似的功能



## 12.Drop down terminal

[快速终端](https://extensions.gnome.org/extension/442/drop-down-terminal/)插件

自从自定义。默认情况下按`Esc`下面的那个键可以调出来，不过因为和markdown代码快捷键冲突了，我自定义成了`Ctrl` + `空格`



## 13.Hide Top Bar

[隐藏顶栏](https://extensions.gnome.org/extension/545/hide-top-bar/)插件。把顶栏的标题栏可以动态隐藏



## 14.No Title Bar - Forked



[隐藏软件标题栏](https://extensions.gnome.org/extension/2015/no-title-bar-forked/)插件，软件的标题栏可以隐藏。最好还是自己设定一下。

## 15.Recent Items

[最近文件](https://extensions.gnome.org/extension/72/recent-items/)插件，这个就不多说了吧



## 16.主题安装

首先，主题和shell主题都是要放在`/usr/share/themes/`文件夹下面

图标在`/usr/share/icons/`目录下

打开[主题下载网站](https://www.gnome-look.org/)，左侧的菜单栏`Full Icon Themes`是图标主题

`GTK3 Themes`是系统主题，`Gnome Shell Themes`是shell主题

注意选择稳定更新的，或者最近更新，且评价好的主题下载。

下7.载后解压移动到相应的文件夹下

打开`显示应用程序`图标，就是那个9个点的那个。找到`优化`——>`外观`

选择自己下好的主题就可以啦。



## 17.字体安装
我推荐的是这个字体

```shell
sudo apt-get install fonts-wqy-microhei 
```
打开`显示应用程序`图标，就是那个9个点的那个。找到`优化`——>`字体`，选择即可



## 18.Xmind

[Xmind](https://www.xmind.cn/download/)是思维导图的无敌软件。支持ubuntu哦。

有`deb`包可以下载

如果下载下来的是`rpm`格式。我们需要使用`alien`将其转化为`deb`的包

```shell
sudo apt-get install alien
```

下载好了之后我们开始转化：

```shell
sudo alien XMind-ZEN-for-Linux-x86-64bit-10.0.0-201911260056.rpm
```

转化完成后，`dpkg`安装即可

```shell
sudo dpkg -i xmind-vana_10.0.0-201911260057_amd64.deb
```



---
未完待续...