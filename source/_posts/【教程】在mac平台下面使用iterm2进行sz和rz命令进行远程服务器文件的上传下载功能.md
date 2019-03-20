---
title: 【教程】在mac平台下面使用iterm2进行sz和rz命令进行远程服务器文件的上传下载功能
date: 2017-02-13 14:55:37
categories: 教程
tags: 
- 教程
- linux
---

在Windows下面使用xshell时，经常使用sz命令进行文件的上传下载非常方便。
但是在mac下面就不能直接使用了需要进行配置才能使用这么方便的功能。
<!--more-->

## 在mac电脑上安装lrzsz
```shell
brew install lrzsz
```

安装好了之后，需要进行配置了。

## 配置iterm2属性
拉取 https://github.com/mmastrac/iterm2-zmodem 两个sh文件，将他们拷贝到/usr/local/bin文件夹中。

![github图片](http://upload-images.jianshu.io/upload_images/1013655-17a759d3753f0935.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按照Setup的指南，在
Perfences的Profiles标签页的Advanced标签页下面，编辑Triggers

![iterm2图片](http://upload-images.jianshu.io/upload_images/1013655-6beca6dbbca1e483.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


设置为这样（这些参数参考github上面的README文件）：
![Triggers设置](http://upload-images.jianshu.io/upload_images/1013655-9e70f4a081453fa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在远程服务器上也安装好lrzsz就可以愉快的使用sz和rz命令了。
```shell
yum -y install lrzsz
```

大功告成。

该文章可以看做是github上面README的翻译版本。

1. Install lrzsz on OSX: brew install lrzsz
2. Save the iterm2-send-zmodem.sh and iterm2-recv-zmodem.sh scripts in /usr/local/bin/
3. Set up Triggers in iTerm 2 like so:
```shell
Regular expression: rz waiting to receive.\*\*B0100
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-send-zmodem.sh
    Instant: checked

    Regular expression: \*\*B00000000000000
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
    Instant: checked
```

### 参考文章：
http://www.tuicool.com/articles/EvemMfr
https://github.com/mmastrac/iterm2-zmodem