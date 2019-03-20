---
title: how to use Mybatis generator base in Maven
date: 2017-03-13 11:11:05
tags:
- Mybatis
- java
---

Mybatis属于半自动ORM，在使用这个框架中，工作量最大的就是书写Mapping的映射文件，由于手动书写很容易出错，我们可以利用Mybatis-Generator来帮我们自动生成文件。

<!--more-->

需要配置pom.xml文件在plugins中加入以下代码：

```java
			<!--mybatis-generator插件-->
			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.2</version>
				<configuration>
					<overwrite>true</overwrite>
					<verbose>true</verbose>
				</configuration>
				<!-- 数据库驱动  -->
				<!--直接在这里配置相应的数据库驱动文件-->
				<dependencies>
					<dependency>
						<groupId>mysql</groupId>
						<artifactId>mysql-connector-java</artifactId>
						<version>5.1.35</version>
					</dependency>
				</dependencies>
			</plugin>
```

现在为该plugin加入配置文件，在`src/main/resources`下面加入两个配置文件，
一个为`dbconfig.properties`
```java
#Oracle的配置
#hibernate.dialect=org.hibernate.dialect.OracleDialect
#jdbc.driver=oracle.jdbc.driver.OracleDriver
#validationQuery=SELECT 1 FROM DUAL
#jdbc.url=jdbc:oracle:thin:@localhost:1521:orcl
#jdbc.username=scott
#jdbc.password=oracle

#mysql的配置
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/fsi?useUnicode=true&characterEncoding=utf-8
jdbc.username=root
jdbc.password=root



#直接修改这里的配置，指定生成的model，dao和mapper文件的位置
model.package=cjm.mybaits.model
dao.package=cjm.mybaits.dao
xml.mapper.package=cjm.mybaits.dao

target.project=src/main/java
```

一个为`generatorConfig.xml`
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 配置文件路径 -->
    <properties resource="dbconfig.properties"/>

    <!--数据库驱动包路径 已经在Maven的pom配置过了，这里就不需要了-->
    <!--<classPathEntry location="${drive.class.path}"/>-->

    <context id="MySQLTables" targetRuntime="MyBatis3Simple">
        <!--关闭注释 -->
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--数据库连接信息 -->
        <jdbcConnection driverClass="${jdbc.driver}" connectionURL="${jdbc.url}" userId="${jdbc.username}"
                        password="${jdbc.password}">
        </jdbcConnection>
        <!--
        默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
            true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal
         -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>


        <!--生成的model 包路径 -->
        <javaModelGenerator targetPackage="${model.package}" targetProject="${target.project}">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!--生成xml mapper文件 路径 -->
        <sqlMapGenerator targetPackage="${xml.mapper.package}" targetProject="${target.project}">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 生成的Dao接口 的包路径 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="${dao.package}" targetProject="${target.project}">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!--对应数据库表名 -->
        <table schema="fsi" tableName="school"></table>
    </context>
</generatorConfiguration>
```
在`dbconfig.properties`文件中，修改数据库连接的配置和生成实体文件的配置，然后在`generatorConfig.xml`里面只需要添加对应的`table`属性

![tables.png](http://upload-images.jianshu.io/upload_images/1013655-1a797411ae0035dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行`mvn mybatis-generator:generate` 命令就可以生成对应的实体和mapper文件。

![eclipse执行maven命令.png](http://upload-images.jianshu.io/upload_images/1013655-607c7b369d23bc04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

生成示例如下：

![效果.png](http://upload-images.jianshu.io/upload_images/1013655-f213108eab3a04cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样的方法适用于直接部署在项目中，生成的文件直接就是在项目中，非常方便，不需要挪来挪去。
