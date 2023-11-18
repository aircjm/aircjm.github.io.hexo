---
title: Mac 下完全删除java环境
date: 2023-03-02 16:04:21
createDate: [[2023-03-02]]
updateAt: 2023-03-02 16:04:21
publish: true
draft: true
cards-deck: Default
tags: #Java #mac 
---

tags: #Java #mac 

公司发的mac上面已经有java环境了，但是环境是有问题的，需要完全卸载掉。

```bash
sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin


sudo rm -rf /System/Library/Frameworks/JavaVM.framework
sudo rm -rf /usr/bin/java
sudo rm -rf /usr/bin/javac
sudo rm -rf /usr/bin/javadoc
sudo rm -rf /usr/bin/javah
sudo rm -rf /usr/bin/javap
sudo rm -rf /usr/bin/javaws
sudo rm -rf /var/db/receipts/com.oracle.jdk8u291.bom
sudo rm -rf /var/db/receipts/com.oracle.jdk8u291.plist
sudo rm -rf /var/db/receipts/com.oracle.jre.bom
sudo rm -rf /var/db/receipts/com.oracle.jre.plist
sudo rm -rf /var/root/Library/Preferences/com.oracle.javadeployment.plist
sudo rm -rf ~/Library/Preferences/com.oracle.java.JavaAppletPlugin.plist
sudo rm -rf ~/Library/Preferences/com.oracle.javadeployment.plist
sudo rm -rf ~/.oracle_jre_usage
```
有的可能出现无权限删除 这个不要紧，重启下就可以看到完全删除了。