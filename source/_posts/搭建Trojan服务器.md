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

## 客户端

依然使用V2rayN，但是要使用高版本[V2rayN v4.20](https://github.com/2dust/v2rayN/releases)，V2rayN还需要单独下载内核[v2ray-core v4.42.2](https://github.com/v2fly/v2ray-core/releases)。

![image-20210928161626546](搭建Trojan服务器/image-20210928161626546.png)

## 遇到的问题

用的好好的，突然不行了（但是Trojan的管理web依然可以访问登录）。

![image-20211008222742934](搭建Trojan服务器/image-20211008222742934.png)