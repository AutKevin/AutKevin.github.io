---
title: JellyFin搭建
date: 2025-04-11 11:41:53
categories: NAS
tags: [NAS]
---

## JellyFin影音系统搭建

NASTOOL负责优化刮削和做文件整理，Jackeet负责索引，Qbittorrent负责下载，Jellyfin负责播放。

![img](./JellyFin搭建/v2-ed25d55e783901509e177bcabba1479f_1440w.jpg)

首先新建video文件夹，下面新建movie、tv、link（link里面再建立movie和tv）

![image-20250411135358192](./JellyFin搭建/image-20250411135358192.png)

各个服务的挂载目录指向说明

![image-20250411135513100](./JellyFin搭建/image-20250411135513100.png)

### 镜像下载

qBittorrent、JellyFin、ChineseSubFinder、NASTool

```bash
# 拉取qbittorrent镜像
docker pull linuxserver/qbittorrent:latest
# 拉取jellyfin
docker pull nyanmisaka/jellyfin:latest
# 拉取字幕镜像
docker pull allanpk716/chinesesubfinder:latest
# 种子索引器
docker pull linuxserver/jackett:latest
#拉取NASTool,一定要是2.9.1版本，这个版本最全
docker pull nastool/nas-tools:2.9.1
```

#### dockerfile

在 QNAP 的 File Station 中，创建以下目录结构

/share/Container/
├── qbittorrent/
│   └── config/
├── jellyfin/
│   └── config/
├── chinesesubfinder/
│   └── config/
├── jackett/
│   └── config/
├── nas-tools/
│   └── config/

创建统一的媒体目录

/share/Multimedia/
├── Movies/
├── TV/
├── Anime/

```bash
#qbittorrent
docker run -d \
  --name=qbittorrent \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -e WEBUI_PORT=8080 \
  -p 8080:8080 \
  -p 6881:6881 \
  -p 6881:6881/udp \
  -v /share/Container/qbittorrent/config:/config \
  -v /share/Downloads:/downloads \
  linuxserver/qbittorrent:latest

#Jellyfin
docker run -d \
  --name=jellyfin \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 8096:8096 \
  -v /share/Container/jellyfin/config:/config \
  -v /share/Multimedia:/media \
  nyanmisaka/jellyfin:latest
  
#ChineseSubFinder
docker run -d \
  --name=chinesesubfinder \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 19035:19035 \
  -v /share/Container/chinesesubfinder/config:/config \
  -v /share/Multimedia:/media \
  allanpk716/chinesesubfinder:latest
  
#Jackett
docker run -d \
  --name=jackett \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 9117:9117 \
  -v /share/Container/jackett/config:/config \
  -v /share/Downloads:/downloads \
  linuxserver/jackett:latest

#NASTool（版本 2.9.1）
docker run -d \
  --name=nas-tools \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 3000:3000 \
  -v /share/Container/nas-tools/config:/config \
  -v /share/Multimedia:/media \
  -v /share/Downloads:/downloads \
  nastool/nas-tools:2.9.1
```

#### docker compose

```yml
version: "3" 
services:  
     jellyfin:    
        image: nyanmisaka/jellyfin:latest
        container_name: jellyfin    
        environment:   
         - PUID=0     
         - PGID=0     
         - TZ=Asia/Shanghai    
        volumes:      
         - /share/Container/jellyfin/config:/config      
         - /share/media/video:/video
        ports:     
         - 28096:8096    
         - 28920:8920   
        devices:   
         - /dev/dri:/dev/dri
     nastool:
        image: yohe/nastool:2.9.1
        container_name: nastool
        environment:   
         - PUID=0     
         - PGID=0     
         - TZ=Asia/Shanghai
         - ALPINE_MIRROR=mirrors.ustc.edu.cn
         - LANG=C.UTF-8
         - NASTOOL_AUTO_UPDATE=false
         - NASTOOL_CN_UPDATE=true
         - NASTOOL_CONFIG=/config/config.yaml
         - NASTOOL_VERSION=master
         - PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
         - PYPI_MIRROR=https://pypi.tuna.tsinghua.edu.cn/simple
         - REPO_URL=https://github.com/jxxghp/nas-tools.git
         - UMASK=000
         - WORKDIR=/nas-tools
        volumes:      
         - /share/Container/nastool/config:/config      
         - /share/media/video:/video
        ports:     
         - 23000:3000
     jackett:
        image: linuxserver/jackett:latest
        container_name: jackett
        volumes:
         - /share/Container/jackett/config:/config
         - /share/Container/jackett/downloads:/downloads
        environment:   
         - HOME=/root
         - LSIO_FIRST_PARTY=true   
         - PATH=/lsiopy/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
         - S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0
         - S6_STAGE2_HOOK=/docker-mods
         - S6_VERBOSITY=1
         - TERM=xterm
         - VIRTUAL_ENV=/lsiopy
         - XDG_CONFIG_HOME=/config
         - XDG_DATA_HOME=/config
        ports:     
         - 9117:9117
     qBittorrent:
        image: linuxserver/qbittorrent:4.4.3
        container_name: qBittorrent
        volumes:      
         - /share/Container/qBittorrent/config:/config      
         - /share/media/video:/downloads
        ports:     
         - 28080:8080
         - 26881:6881
         - 26881:6881/udp
```



### qBittorrent

![image-20250411141337075](./JellyFin搭建/image-20250411141337075.png)

访问url显示unauthorized

```bash
#切换管理员模式
sudo -i
#查看docer容器ID
docker ps
#进入容器
docker exec -it 39718f35477f /bin/bash

#修改配置文件
vi /config/qBittorrent/qBittorrent.conf
#添加如下内容
WebUI\HostHeaderValidation=false 
WebUI\CSRFProtection=false

#退出docker环境
exit
#重启容器
docker restart 39718f35477f
```

添加环境变量WebUIHostHeaderValidation=false

使用多种方法都未解决unauthorized问题

#### AppCenter直接安装

![image-20250411150538702](./JellyFin搭建/image-20250411150538702.png)

启动后，系统日志提示密码，使用admin和临时密码进入后重置密码

![image-20250411150457278](./JellyFin搭建/image-20250411150457278.png)

#### 设置下载路径为video

![image-20250411164214203](./JellyFin搭建/image-20250411164214203.png)

### JellyFin

```bash
docker run -d \
  --name=jellyfin \
  --restart unless-stopped \
  -e PUID=1000 \
  -e PGID=1000 \
  -e HTTP_PROXY=http://10.10.10.195:17890 \
  -e HTTPS_PROXY=http://0.10.10.195:17890 \
  -e TZ=Asia/Shanghai \
  -p 28096:8096 \
  -v /share/Container/jellyfin/config:/config \
  -v /share/Container/jellyfin/cache:/cache \
  -v /share/video:/media \
  nyanmisaka/jellyfin:latest
```



#### 创建容器

创建时在存储中可以挂载video，在环境中设置代理http_proxy和https_proxy

![image-20250411154435718](./JellyFin搭建/image-20250411154435718.png)

#### 媒体库

如果使用nastool刮削，这里文件夹需要设置成/video/link/movie

![image-20250411155144601](./JellyFin搭建/image-20250411155144601.png)

#### API 密钥

新建一个 API 密钥，方便后面配置 nas-tools

![image-20250411154841977](./JellyFin搭建/image-20250411154841977.png)

### ChineseSubFinder

主要从subhd、zimuku、shooter、xunlei四个字幕站拉取字幕

#### 创建容器时

挂载主机文件夹为media，否则无法保存

![image-20250411133905549](./JellyFin搭建/image-20250411133905549.png)

设置电影和连续剧目录，如果使用nastool，这里需要设置/media/link/movie和/media/link/tv

![image-20250411133944666](./JellyFin搭建/image-20250411133944666.png)

####  API Key

先在纵览中停止守护进程，然后在配置中心生成API Key。

![image-20250411155655632](./JellyFin搭建/image-20250411155655632.png)

#### 字幕源设置

![image-20250411160427749](./JellyFin搭建/image-20250411160427749.png)

注册完assrt.net后打开https://assrt.net/usercp.php链接即可查看APIKey

### Jackett

种子索引器

创建容器

![image-20250411211748595](./JellyFin搭建/image-20250411211748595.png)



### NASTool

nastool替代了jellyfin的刮削功能，以及整合了其他索引器、下载器、字幕下载工具

#### 创建容器

![image-20250411161009955](./JellyFin搭建/image-20250411161009955.png)

创建完成后，默认admin/password登录

#### 配置TMDB API Key

同时记得勾选刮削元数据及图片

![image-20250411161212078](./JellyFin搭建/image-20250411161212078.png)

#### 设置媒体库Link

全部设置为link文件夹下面的movie、tv、animate

![image-20250411161523808](./JellyFin搭建/image-20250411161523808.png)

#### 同步文件夹

![image-20250411184028944](./JellyFin搭建/image-20250411184028944.png)

配置完成

![image-20250411205532872](./JellyFin搭建/image-20250411205532872.png)

开启目录同步

![image-20250411210614558](./JellyFin搭建/image-20250411210614558.png)

#### 配置下载器

![image-20250411213538437](./JellyFin搭建/image-20250411213538437.png)

配置下载目录

![image-20250411214837310](./JellyFin搭建/image-20250411214837310.png)

#### 配置Jellyfin

![image-20250411215308363](./JellyFin搭建/image-20250411215308363.png)

#### 字幕配置

![image-20250411220328376](./JellyFin搭建/image-20250411220328376.png)
