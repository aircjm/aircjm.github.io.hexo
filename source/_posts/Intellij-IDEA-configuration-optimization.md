---
title: idea 配置优化 Intellij_IDEA_configuration_optimization
date: 2016-09-21 15:32:04
tags:
- config
- idea
- java
---

平时主要开发IDE使用的是Intellij IDEA 14，有的时候需要对IDE的配置进行微调，提高开发效率。
下面就是比较常用的修改项。



<!-- more -->



### 项目启动不默认打开最后项目：

File -> Settings -> Appearance & Behavior -> System Settings -> Startup/Shutdown 标签项 -> 去掉 Reopen last project on startup

### 关闭程序自动更新检查：

File -> Settings -> Appearance & Behavior -> System Settings -> Updates -> 去掉勾选 Automatically check updates for

### 设置字符编码格式：

File -> Settings -> Editor -> File Encodings -> 设置 IDE Encoding、Project Encoding、Defalut encoding for properties files 都为 UTF-8 并且勾选 Transparent native -to-ascii conversion

### 过滤的文件类型和目录：

File -> Settings -> Editor -> File Types -> Ignore files and folders -> 添加 `*.iml;*.idea;*.classpath;*.project;*.settings;target;`

### 显示代码行号和方法分割线：

File -> Settings -> Editor -> General -> Apperance -> 勾选 Show line numbers、Show method separators

代码折叠：

### 有时候我们需要阅读每一行代码，所以需要让每行代码都显示

File -> Settings -> Editor -> General -> Code Folding 去除勾选 Collapse by default 中的File header Imports， One-line methods

idea import比较多的话，会自动转成*，对于开发不是很友好，所以把import调大点。

File -> Settings -> Editor -> Code Style -> Java ->  Class count to use import with '*' 和  Names count to use static improt with 调到1000.



### Maven镜像地址修改

关于maven的配置，idea中自己集成了maven的插件，可以无缝进行使用。

但是为了加快maven的速度，切换了maven的阿里云镜像

在maven的配置文件settings.xml的mirrors节点添加

```xml
<mirror>    
    <id>nexus-aliyun</id>    
    <mirrorOf>central</mirrorOf>      
    <name>Nexus aliyun</name>    
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>    
</mirror>  
```

