---
title: VPS实用系之二，安装docker容器
date: 2019-03-15 16:55:54
tags:
- 工具 
- VPS
---
我们入手了一台虚拟服务器（VPS）后，可以用来部署各种应用，不管是我们日常使用的，还是试水体验的。其中有些应用的部署配置比较繁琐，需要各种复杂配置。为了节约时间，我们可以用拿打包好的docker镜像直接部署。省去了繁琐的配置，同时也使得应用的更新极为方便。

于是一些应用尽量在docker中部署，专注于使用就可以了。

## 初始步骤
### 1.安装vps虚拟机
服务器我都选ubuntu。都作为linux发行版，其实没啥好选。老司机的说法是部署unbuntu能躲不少坑。有几点提醒：
- Vultr 提供免费的快照功能来备份服务器，我们可以用来快速恢复，避免重装。
- 推荐日本服务器，其次美国。对于内陆用户，日本的线路速度线路速度快，延迟低。
- 除非删机器，IP不能换。
- 安装新服，ssh记得勾选。
- 重装，ssh需要更换。[http://www.witheringtree.com/2011/01/how-to-fix-rsa-host-key-change-error-on-mac/]

### 2.BBR加速。
内核高于4.9才支持bbr加速。现在不管哪个linux系统的最新版，内核都在4.9之上了。
了解系统信息：

`cat /etc/issue`

了解内核信息

`uname -a`

4.9内核以上的linux系统都支持。下面我们开启它们。，加入以下设置：

`vi /etc/sysctl.conf`

> net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr

使内核马上生效：
`sysctl -p`

### 3.安装docker

`curl -sSL https://get.docker.com/ | sh`

部署docker管理界面：
```
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock \
--name ui --restart=always lihaixin/portainer:me
```
因为你是root进入的，所以你已经具备了管理员权限。所以不要输入提示的增加用户命令。不信，你输入`who`。你可以看到你的root身份。

`docker ps `//就可以看到他已经运行在9000端口了
进入管理界面：
> `https://<host>:9000`

由于没有tcl证书，故需允许未认证的网站登录。
记住登录密码。帐号admin，选local。
以后就可以图形界面管理所有docker了。

## 后期docker应用镜像
每个人的应用都不一样。我们根据需要，去找docker镜像仓库（找官方的或个人发布的）安装即可。一般都会配有获取说明。

## 总之
Docker 是一快速部署应用的工具，我们可以轻松部署，迁移，维护和拓展应用。我们要善用他的特性。

***
参考：
[Docker 学习.1 从零开始的 docker 部署](https://blog.starryvoid.com/archives/322.html)