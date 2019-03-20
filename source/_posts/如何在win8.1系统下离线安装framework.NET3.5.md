title: 在win8.1系统下离线安装framework.NET3.5
date: 2015-07-26 16:28:23
tags:
- win
- 应用
- 教程
---
如何在win8.1系统下离线安装framework.NET3.5？需要镜像文件
<!--more-->

# 如何在win8.1系统下离线安装framework.NET3.5？

## 需要的工具
	win8.1的镜像文件

## 1.加载win8.1的镜像文件

## 2.打开命令提示符（管理员），在上面输入以下命令，然后执行，其中F为镜像文件加载的盘符。

```bash
dism.exe /online /enable-feature /featurename:NetFX3 /Source:F:\sources\sxs
```