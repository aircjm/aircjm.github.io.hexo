---
title: Linux定时任务实例-shadowsocks进程停止后自动重启_linux-timer-task-instance_shadowsocks-process-automatically-restarts-after-stopping
date: 2016-10-17 22:36:21
tags:
- shadowsocks
- 教程

---

在自己128M的小内存VPS上安装了锐速和SSR，可能内存比较小的问题吧，老是用用shadowsocks的然后进程就挂了，导致无法使用，郁闷。

然后就到网上找了一些资料，通过crontab定时任务刷新的情况，使shadowsocks停止服务之后自动重启服务，避免无法使用的尴尬局面出现。
<!-- more -->

环境：

1. CentOS 6.5 x86
2. SSR
3. crontab

### 创建脚本检测进程情况：

`vi monitor.sh`

内容为

```bash
#! /bin/sh

proc_name="procname"                            # 待监控进程名

number=`ps -ef | grep $proc_name | grep -v grep | wc -l`

if [ $number -eq 0 ]                            # 判断进程是否存在
then
        /etc/init.d/procname restart              # 重启进程的命令，请相应修改
fi
```



将`procname`换成对应的进程名称即可。

下面是替换后的脚本：

```Shell
#! /bin/sh

proc_name="shadowsocks"                             # 待监控进程名

number=`ps -ef | grep $proc_name | grep -v grep | wc -l`

if [ $number -eq 0 ]                             # 判断进程是否存在
then
        /etc/init.d/shadowsocks restart               # 重启进程的命令，请相应修改
fi
```



进程名在

`ps -ef`

查出的列表的`CMD`中可以找到。

命令创建好了，kill掉进程，然后通过`./monitor.sh`执行下看看有没有效果。

可能提示权限有问题。添加一下权限就好了。

```shell
chmod 777 monitor.sh
```

再通过`ps -ef`可以看看，服务是不是起来的。

### 创建定时任务：

为了定时进行自动操作。我们就需要crontab的帮忙了。

```Shell
crontab -e
```

加入以下内容：

`*/1 * * * * ./monitor.sh`

加入之后重新启动下crontab即可。

`/etc/init.d/crond restart`



这样的话就可以了，每分钟都会执行shell脚本，看看服务有没有挂，挂的话，会自动把服务起来的。

参考：

[http://kenua.blog.sohu.com/280457820.html](http://kenua.blog.sohu.com/280457820.html)

[http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html](http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html)

[http://www.tennfy.com/3517.html](http://www.tennfy.com/3517.html)