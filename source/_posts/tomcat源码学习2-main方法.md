---
title: tomcat源码学习2-main方法
date: 2016-07-03 18:46:15
tags:
- tomcat
- 源码
---

环境搭建好了，将项目启动起来，就可以打断点进行调试了。看看tomcat到底是怎么启动的。

<!--more-->

之前我们配置环境的时候就已经说了，tomcat的启动是从`org.apache.catalina.startup.Bootstrap`的`main`方法开始的。我们就看下这个main方法里面有什么玄机吧。

```java
public static void main(String args[]) {
    
    if (daemon == null) {
        // Don't set daemon until init() has completed
        Bootstrap bootstrap = new Bootstrap();
        try {
            bootstrap.init();
        } catch (Throwable t) {
            handleThrowable(t);
            t.printStackTrace();
            return;
        }
        daemon = bootstrap;
    } else {
        // When running as a service the call to stop will be on a new
        // thread so make sure the correct class loader is used to prevent
        // a range of class not found exceptions.
        Thread.currentThread().setContextClassLoader(daemon.catalinaLoader);
    }

    try {
        String command = "start";
        if (args.length > 0) {
            command = args[args.length - 1];
        }

        if (command.equals("startd")) {
            args[args.length - 1] = "start";
            daemon.load(args);
            daemon.start();
        } else if (command.equals("stopd")) {
            args[args.length - 1] = "stop";
            daemon.stop();
        } else if (command.equals("start")) {
            daemon.setAwait(true);
            daemon.load(args);
            daemon.start();
        } else if (command.equals("stop")) {
            daemon.stopServer(args);
        } else if (command.equals("configtest")) {
            daemon.load(args);
            if (null==daemon.getServer()) {
                System.exit(1);
            }
            System.exit(0);
        } else {
            log.warn("Bootstrap: command \"" + command + "\" does not exist.");
        }
    } catch (Throwable t) {
        // Unwrap the Exception for clearer error reporting
        if (t instanceof InvocationTargetException &&
                t.getCause() != null) {
            t = t.getCause();
        }
        handleThrowable(t);
        t.printStackTrace();
        System.exit(1);
    }

}
```

`daemon`是在里面定义过的，类型也是Bootstrap类型的，值为null，流程继续往下走，执行初始化(init)方法。

我们看下初始化方法。

```java
public void init()
    throws Exception
{

    // Set Catalina path
    setCatalinaHome();
    setCatalinaBase();

    initClassLoaders();

    Thread.currentThread().setContextClassLoader(catalinaLoader);

    SecurityClassLoad.securityClassLoad(catalinaLoader);

    // Load our startup class and call its process() method
    if (log.isDebugEnabled())
        log.debug("Loading startup class");
    Class<?> startupClass =
        catalinaLoader.loadClass
        ("org.apache.catalina.startup.Catalina");
    Object startupInstance = startupClass.newInstance();

    // Set the shared extensions class loader
    if (log.isDebugEnabled())
        log.debug("Setting startup class properties");
    String methodName = "setParentClassLoader";
    Class<?> paramTypes[] = new Class[1];
    paramTypes[0] = Class.forName("java.lang.ClassLoader");
    Object paramValues[] = new Object[1];
    paramValues[0] = sharedLoader;
    Method method =
        startupInstance.getClass().getMethod(methodName, paramTypes);
    method.invoke(startupInstance, paramValues);

    catalinaDaemon = startupInstance;

}
```

其中这个方法主要干的事情我们通过源码可以了解到

1. 设置了`catalina.home`和`catalina.base`属性
2. 初始化类的加载器（initClassLoaders），分别创建了（Class loader creation）commonLoader，catalinaLoader，sharedLoader这几个ClassLoader。
3. 然后实例化了一个`org.apache.catalina.startup.Catalina`对象，通过反射方式调用了`Catalina`对象的`setParentClassLoader`方法。

好了这个init方法就完了，其实实际上里面还是很复杂的，有时间可以研究一下，为什么这样做，有么有更好的方法实现。



继续我们可以看到默认的`command`值为`start`，开始执行`load`方法。看下load的源码。

```java
private void load(String[] arguments)
    throws Exception {

    // Call the load() method
    String methodName = "load";
    Object param[];
    Class<?> paramTypes[];
    if (arguments==null || arguments.length==0) {
        paramTypes = null;
        param = null;
    } else {
        paramTypes = new Class[1];
        paramTypes[0] = arguments.getClass();
        param = new Object[1];
        param[0] = arguments;
    }
    Method method =
        catalinaDaemon.getClass().getMethod(methodName, paramTypes);
    if (log.isDebugEnabled())
        log.debug("Calling startup class " + method);
    method.invoke(catalinaDaemon, param);

}
```

这个里面的内容并不复杂，也是通过反射的形式调用`Catalina`类的`load`方法。

load执行完了之后，就执行start方法了。一样的内容也是通过反射调用的`Catalina`类的`start`方法。



到这里的时候，这个main方法也已经结束了。