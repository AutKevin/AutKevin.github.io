---
title: 威联通TS-466C Docker Compose
date: 2025-04-18 15:50:17
categories: NAS
tags: [NAS]
---

## 常用类

### DDNS-GO

DDNS-GO是一款支持包含阿里云、腾讯云、华为云、Cloudflare等16个DNS解析商的**动态域名解析工具**，支持将安装了DDNS-GO设备的公网IPv4、IPv6定时自动绑定到自定义的域名上。

### OneNav

OneNav是开源的书签管理工具，支持导入chrome、firefox、edge等主流的浏览器数据，拥有账号功能，可以对不同浏览器的书签进行集中管理，方便多设备用户的使用。

### Vaultwarden

Vaultwarden是一款知名的开源密码管理工具，拥有网页、APP、浏览器插件等多种形式支持，可以自动生成密码、回填账号密码、管理账号密码、管理2FA等功能。

### HomeAssistant

大名鼎鼎的HA智能中枢，在小米开放了官方HA插件以后，HA在国内的易用性也越来越好，搭配各式各样的UI、模板、插件，我们可以将手机、平板等代替中枢网关，实现对智能家居的一键管理，创建各式各样的智能化任务。

### cat

## 家庭照片管理类

lomorage（全平台自托管照片管理）

immich（开源家庭相册管理）

photostruct（家庭照片管理工具）

photoview（瀑布流相册管理）

ente（端到端加密相册管理）

## 个人学习类

language-tool（语言在线校验）

memos（碎片化知识记忆、笔记）

habitica（用游戏的方式养成习惯）

traggo（以时间轴为核心的时间管理）

Reference（包含数据库、前端、后端等）‍

## 办公团队协作类

推荐 vikunja、affine、showdoc、heimdallr

erpnext（开源 erp 系统）

wbo（简易在线白板绘图）

o2oa（OA 系统）

panka（项目管理）

our shopping list（todo 工具）

twenty（开源 CRM 项目）

bookstack（知识库）

cloper（调研工具）

huntly（RSS 集成）

kimai（个人时间管理）

snipe-it（IT 设备管理）

vikunja（项目管理）

univer（文档）

iToP（IT 服务管理）

affine（白板、文档）

heimdallr（消息推送）

Superset（BI 数仓系统）

showdoc（知识库）

fiora（即时聊天）

Perlite（知识库）

## 音乐类

### xiaomusic

官网：https://github.com/hanxi/xiaomusic

支持小米音响

```yml
version: '3'
services:
  xiaomusic:
    image: hanxi/xiaomusic
    container_name: xiaomusic
    restart: unless-stopped
    ports:
      - 58090:8090
    environment:
      XIAOMUSIC_PUBLIC_PORT: 58090
    volumes:
      - /share/Multimedia/music:/app/music
      - /share/Container/xiaomusic/conf:/app/conf
```

### Navidrome / Music-tag-Web

Navidrome是目前docker中最完善的音乐媒体项目，搭配音流APP可以实现不亚于网易云的收听体验。结合music-tag-web这个强大的音乐元数据刮削工具，可以根据歌曲名字从互联网侧刮削几乎所有的音乐封面、歌词、歌手、专辑等信息，让音乐库打造的更完整。

## 漫画

### Komga/Bungumi

```yml
version: '3.3'

services:
  komga:
    image: gotson/komga
    container_name: komga
    volumes:
      - type: bind
        source: /share/Container/komga/config
        target: /config
      - type: bind
        source: /share/Container/komga/data
        target: /data
      - type: bind
        source: /etc/localtime
        target: /etc/localtime
        read_only: true
    ports:
      - 25600:25600
    restart: unless-stopped
```

完成度极高的一款漫画自托管库，搭配Bungumi可以实现漫画、轻小说、番的元数据刮削，是继海报墙之后，漫画/轻小说文库墙的一套解决方案。

teemii（在线漫画下载和管理）

## 影视

### Jellyfin

### Emby

### Plex

onelist（用于 alist 的影视刮削工具）

## 书籍

### TaleBook

TaleBook是一款书籍管理工具，它基于calibre项目构建，具备书籍管理、在线阅读与推送、用户管理、SSO登录、从百度/豆瓣拉取书籍信息等功能。

### Audibookshelf

Audiobookshelf是一款自托管的有声读物和播客服务器，包含web端和app端应用。
双端的浏览记录完全同步，是很不错的一款有声书系统。

### 其他

calibre-web（图书管理）

## 工具类

宝塔（运维面板）

1panel（运维面板）

Excalidraw（在线绘图工具）

WordPress（博客框架）

Gitlab（私人代码库）

Exsi（虚拟机管理）

Uptime Kuma（服务状态监控）

Frps/Frpc（内网穿透）

Vaultwarden（密码管理）

## AI 与生产力工具

2024 年 AI 技术大爆发，云端 AI 服务也能本地部署，AI 与生产力紧密融合！?

Open-WebUI（类 ChatGPT 工具）

Dify（生成式 AI 检索框架）

quivr（基于 AI 的第二大脑）

gpt_academic（学术大模型）

transformer-explainer（大模型可视化）

one-api（大模型 API 中间件）



## 其他

joplin记日记；kanboard管理任务；chromium浏览器；homebox管理物品；openspeedtest测速
