---
title: 搭建Trojan服务器
date: 2021-09-27 16:42:10
categories: Torjan
tags: [VPN,Torjan]
---

## 环境

系统：CentOS7

官网：https://github.com/Jrohy/trojan

## 一键安装

```shell
#安装/更新
source <(curl -sL https://git.io/trojan-install)

#卸载
source <(curl -sL https://git.io/trojan-install) --remove

##trojan管理
trojan
```
安装过程中输入域名，自动生成证书。

![image-20210927173218010](搭建Trojan服务器/image-20210927173218010.png)

## Torjan Web端

https://域名

输入刚才设置的web的admin密码，进入后可以用web管理用户。

![image-20210927173355184](搭建Trojan服务器/image-20210927173355184.png)

