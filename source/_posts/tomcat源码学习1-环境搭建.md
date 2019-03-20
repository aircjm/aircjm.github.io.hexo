---
title: tomcat源码学习1-环境搭建
date: 2016-07-02 20:12:16
tags:
- tomcat
- 源码
---

# 搭建一个tomcat的源码阅读环境

受同事面试情况的影响，觉得是时候看一会源代码了，选择了最熟悉的Tomcat(现在使用最多的Servlet容器)，里面运用了很多设计模式和很好的编程思想，代码非常优秀，值得学习，并且熟悉了之后也方便以后工作中定位问题。

<!--more-->

## 前期准备

本机环境：

1. 操作系统版本：10.11.5版本mac os x
2. jdk版本：1.8.0_92
3. Maven 3
4. Tomcat7

去官网下载一份Tomcat7源码[tomcat7](https://tomcat.apache.org/download-70.cgi)

选择Source Code Distributions下面的zip文件进行下载，下载好之后解压，可以看到Tomcat项目是通过Ant进行构建的，没用过这个东西（傻眼），网上找了一下，发现也可以通过maven的方式来进行管理依赖关系。

新建一个Maven的配置文件pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>Tomcat7.0</artifactId>
    <name>Tomcat7.0 src</name>
    <version>7.0</version>

    <build>
        <finalName>Tomcat7.0</finalName>
        <sourceDirectory>java</sourceDirectory>
        <resources>
            <resource>
                <directory>java</directory>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        <dependency>
            <groupId>ant</groupId>
            <artifactId>ant</artifactId>
            <version>1.7.0</version>
        </dependency>
        <dependency>
            <groupId>ant</groupId>
            <artifactId>ant-apache-log4j</artifactId>
            <version>1.6.5</version>
        </dependency>
        <dependency>
            <groupId>ant</groupId>
            <artifactId>ant-commons-logging</artifactId>
            <version>1.6.5</version>
        </dependency>
        <dependency>
            <groupId>wsdl4j</groupId>
            <artifactId>wsdl4j</artifactId>
            <version>1.6.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.xml/jaxrpc-api -->
      <dependency>
          <groupId>javax.xml</groupId>
          <artifactId>jaxrpc-api</artifactId>
          <version>1.1</version>
      </dependency>
        <dependency>
            <groupId>org.eclipse.jdt.core.compiler</groupId>
            <artifactId>ecj</artifactId>
            <version>4.4</version>
        </dependency>
    </dependencies>
</project>
```

网上那个教程不知道为什么我这边直接就报错，没有找到对应的jar包，自己去Maven找了一个，发现也是可以的。

Tomcat的启动方法是在源码下面的/java/org/apache/catalina/startup/Bootstrap.java类的main方法。

现在需要配置一个Application，点击菜单栏`Run`-`Edit configurations...`，点击加号新建配置项`Application`。

配置Main class为`org.apache.catalina.startup.Bootstrap`，网上有教程说需要配置`VM options`，不知道为什么我在我自己mac电脑上配置好之后没有办法正常使用，只有去掉才可以，还没有搞清楚原因。



配置好了之后，我们先使用Maven的compile编译下，看看有没有问题，一切OK，正常编译通过。

运行我们之前配好的Application。

![tomcat源码启动](http://upload-images.jianshu.io/upload_images/1013655-0e9249eb7dc61195.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

访问，[http://localhost:8080/](http://localhost:8080/)。熟悉的Tomcat界面表示环境搭建成功。底下就可以打断电调试Tomcat7的源码。



### 参考：

[用Intellij IDEA调试Tomcat7.0源码](http://www.jianshu.com/p/9e3f99f2d5bb)

