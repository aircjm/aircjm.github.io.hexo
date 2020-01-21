---
title: curl wget 不验证证书进行https请求
date: 2020-01-21 11:07
tags:
- curl
- wget
- ssh

---

运维给我们服务部署的时候,https证书安错了，导致浏览器可以通过访问（强制忽略https验证）
但是通过curl wget 无法访问，直接报错，对应的证书出错

又比较急调用接口，这个怎么办呢？？

可以通过参数忽略校验https

<!--more-->




```shell
wget 'https://domain.com/get' --no-check-certificate

curl 'https://domain.com/get' -k
```