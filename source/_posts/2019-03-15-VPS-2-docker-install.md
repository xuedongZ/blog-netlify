---
title: VPS实用系之二，安装docker容器
date: 2019-03-15 16:55:54
tags:
- 工具 
- VPS
---
我们入手了一台虚拟服务器（VPS）后，可以用来部署各种应用，不管是我们日常使用的，还是试水体验的。其中有些应用的部署配置比较繁琐，需要各种复杂配置。为了节约时间，我们可以用拿打包好的docker镜像直接部署。省去了繁琐的配置，同时也使得应用的更新极为方便。

## docker是什么
Docker 是一个开放源代码软件项目，让应用程序布署在软件容器下的工作可以自动化进行，借此在 Linux 操作系统上，提供一个额外的软件抽象层，以及操作系统层虚拟化的自动管理机制。
Docker 利用 Linux 核心中的资源分脱机制，例如 cgroups，以及 Linux 核心名字空间（name space），来创建独立的软件容器（containers）。这可以在单一 Linux 实体下运作，避免启动一个虚拟机造成的额外负担。
简单概括起来就是，Docker 是个容器，什么都能往里塞，你也可以理解为是一个轻量化的虚拟机。
使用 Docker 的好处就是对当前系统的环境没有破坏性，基本上一款镜像可以跑在任意包含了 Docker 的机器上，可以说是十分方便了。

## 从零开始行动起来
### 1.快速安装vps虚拟机
服务器我都选ubuntu。都作为linux发行版，其实没啥好选。老司机的说法是部署unbuntu能躲不少坑。有几点提醒：
- 优选日本、香港、新加破等相对较近的主机。延迟更小些。
- 除非删机器，IP不能换。
- 拿到分配IP先要检查是否可用。
  1.ICMP能否ping通；丢包是否严重。
  2.TCP端口有没被掐，被掐说明这个IP被别人玩坏了（这两步不满足，后面的行动就没有意义）。
  3.线路是否合理，比如明明想连新加坡非要用经过日本跳转，多加的延迟你能接受也无妨。
- 安装新服，ssh记得勾选。
- 重装（非删除），ssh需要更换。[http://www.witheringtree.com/2011/01/how-to-fix-rsa-host-key-change-error-on-mac/]



### 2.安装docker CE

`curl -sSL https://get.docker.com/ | sh`

部署docker管理界面：
```
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock \
--name ui --restart=always lihaixin/portainer:me
```
因为你是root进入的，所以你已经具备了管理员权限。所以不要输入提示的增加用户命令。不信，输入`who`,可以看到你的root身份。

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