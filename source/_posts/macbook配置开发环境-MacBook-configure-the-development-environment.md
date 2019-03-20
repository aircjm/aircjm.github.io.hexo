---
title: (macbook配置开发环境)MacBook configure the development environment
date: 2016-04-22 17:20:22
tags:
- mac
- 配置
---

由于误删了OSX的根目录文件导致系统出了问题没有办法正常使用了，所以想重装下系统，虽然OSX已经非常好用了，但是配置过的Macbook更加方便使用。

这里把对电脑的一些配置记录下来，方便以后重装电脑。（汗）

<!--more-->

比较得瑟的升级了OS X的版本到MAC OS Serria，结果就是好像公司的vpn用不了了，我去，所以没有办法只能重装降级版本了。

记录下如何在新电脑上面如何配置吧。

## brew包管理工具

使用brew进行软件的管理，之前使用windows的时候特别羡慕在ubuntu下面直接使用apt命令进行安装软件，想装什么一个命令就可以了，特别爽。在OS X下面也有这样的功能。

这个就是[Homebrew](http://brew.sh/)，非常棒的一个应用。

让你直接通过命令就可以安装软件。

作为一个Java Developer，目前本机上通过brew安装了

1. tomcat
2. maven
3. gradle
4. mysql
5. redis

由于brew不能直接安装jdk，可以通过`Homebrew-cask`，之前使用过程中老是出现cask安装的软件找不到文件位置，已经弃用`Homebrew-cask`，除了可以通过brew直接安装的软件通过brew安装之外，其他都是下载*.dmg安装包进行安装的，比较省心，毕竟mac下一个应用就是一个.app文件，没有那么麻烦。

感觉mac下brew比较厉害的功能，我们在windows下面后有后台进程，在mac下面我们就通过brew来挂载后台服务，例如`mysql`如果需要后台服务一直启动的话。

可以先使用brew info mysql来查看下对应的信息。

可以在输出信息的下面看到：

```shell
To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
```

所以可以通过`brew services start mysql`让他开机就启动一直开启服务。



也可用通`过brew services list`查看对应的服务信息。

例如我的电脑当前就是：

```shell
⇒  brew services list
Name  Status  User Plist
mysql stopped      
redis stopped   
```

mysql和redis服务在本机都没有开启服务。

不知道是电脑问题还是brew服务问题，重装系统之后感觉brew服务贼慢。不折腾了，心累，就这样用吧。

这些和Linux的软件安装需要根据文档来，毕竟linux更新比较快吧，版本兼容性没有windows做得好。

## 切换zsh

原来的bash用的不是很舒服，还是换成zsh，毕竟说得上是神器吧。

查看下现在使用的shell

`cat /etc/shells`

mac下已经默认安装了zsh，可以通过`chsh -s /bin/zsh`命令来切换zsh，然后重启下终端，也就是重新打开终端。

有了方天画戟`zsh`，还需要一匹赤兔马`Oh My Zsh`，既可以通过brew安装，也可以直接访问官网，通过[官网](http://ohmyz.sh)进行安装。



## zsh插件-autojump

推荐一款zsh的神器插件，实现快速跳转目录功能，就是一个字快。

通过brew安装

`brew install autojump`

通过安装提示在.zshrc文件中进行配置。

可以通过`brew info autojump`查看配置。

```shell
==> Caveats
Add the following line to your ~/.bash_profile or ~/.zshrc file (and remember
to source the file to update your current session):
  [[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh

If you use the Fish shell then add the following line to your ~/.config/fish/config.fish:
  [ -f /usr/local/share/autojump/autojump.fish ]; and source /usr/local/share/autojump/autojump.fish

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
```

在.zshrc 配置文件中添加这句话，别忘了在zsh使用的插件list加上去。

```shell
[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh
```

 

```shell
 # Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
 # Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
 # Example format: plugins=(rails git textmate ruby lighthouse)
 # Add wisely, as too many plugins slow down shell startup.
 plugins=(git autojump)
```











