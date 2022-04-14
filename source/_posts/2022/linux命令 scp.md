---
createAt: 2022-04-14 15:33:45
updateAt: 2022-04-14 15:33:45
publish: true
draft: false
cards-deck: Default
---

tags: #linux #scp

linux复制文件和文件夹可以使用 `scp`命令来进行操作。

其实也可以使用[[lszrz]]来进行传输文件，但是需要进行安装。考虑自带的[[openssh]]来进行传输文件。

## 用法
```shell
scp 参数  源文件 目标文件

➜  /tmp scp    
usage: scp [-346BCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file]
            [-J destination] [-l limit] [-o ssh_option] [-P port]
            [-S program] source ... target
```

常用参数：
```
-r 递归操作 （支持目录） 


-C  压缩
```



### 本地到远程
```
scp local_file remote_username@remote_ip:remote_folder 
或者 
scp local_file remote_username@remote_ip:remote_file 
或者 
scp local_file remote_ip:remote_folder 
或者 
scp local_file remote_ip:remote_file
```


实例：
![[Pasted image 20220414154531.png]]


### 远程到本地
```
scp remote_username@remote_ip:remote_folder local_file
或者 
scp remote_username@remote_ip:remote_file  local_file
或者 
scp remote_ip:remote_folder  local_file
或者 
scp remote_ip:remote_file local_file
```



![[Pasted image 20220414155020.png]]

```shell
➜  scp_test tree                        
.
├── dir
│?? └── test.txt
├── test.txt
└── xxxtmp.log

1 directory, 3 files
```








