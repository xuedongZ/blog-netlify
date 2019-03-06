---
title: VPS实用系之一，安全防护
date: 2019-03-06 22:02:18
tags:
---


VPS用有一年多了，最近发现梯子有时有失效，于是重装。开始意识到之前VPS一点安全措施都没做。
> 由于一台vps就相当于一台拥有独立IP的，直接暴露于互联网之上的电脑，这在为你带来便利的同时也直接让你的vps与危险画上了等号，密码穷举、DDOS攻击、各种各样你想到的想不到的攻击方法都在等待着你，不要相信你那惊悚的创意能想出多么强大的密码，统计学告诉我们——还是相信RSA吧！

不信，我们在机器（包括平时使用的linxu的机器）上跑几个命令看看吧。
- 1.查看尝试暴力破解机器密码的人
```
sudo grep "Failed password for root" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | more
```

或者 `cat /var/log/auth.log `
​

- 2.查看暴力猜用户名的人
```
sudo grep "Failed password for invalid user" /var/log/auth.log | awk '{print $13}' | sort | uniq -c | sort -nr | more
```

看完倒吸一口凉气。

想破解我的IP有744个，排名前三的是江苏、北京、四川的IP…看来必须行动起来了。

**采取行动**

可以做的有：更新系统软件；更改默认root密码，新建普通用户，禁用帐号密码ssh登录；更改SSH默认端口，开启SSH免密登录；开启vultr防护墙（自带的免费功能）…

在开始之前，提一下**本例环境：**
- 本地主机：macOS主机；
- 终端：使用iterm。mac的iterm已经集成了Xshell，可以远程连接服务器。window系统就需要安装Putty或Xshell这样的管理工具登陆服务器；
- vps服务器：vultr-unbuntu系统。

## 基本设置

### 系统软件更新

`root@vultr:~#` apt-get update
`root@vultr:~#` apt-get upgrade
没用配置国内的源，速度嫌慢，就省略这步，否则需要等20mins。
如果你的服务器只是用来安装一个SS或SSR应用，并没有其他非常重要的服务端服务，也许并不需要这种操作，因为这一过程实在太消耗时间和资源了。但是如果是有其他重要的数据服务，保证软件的安全补丁的及时更新还是十分必要的。

### 用户设置

1.更改 root 密码 ：
passwd
小提示：好的密码应该同时包含数字、小写英文字母、大写英文字母、和标点符号，最少 15 个字符，这样的密码强度大概有 90 bit，勉强可以应付密码被“暴力破解”，当然最好使用随机产生，不要自己键入，使用复制黏贴（shife+Insert）。

2.新建用户并赋予root 权限

```
useradd superman
passwd superman
//输入密码
//再次确认密码
```

给 superman 这个用户添加 sudo 权限
```
echo -e "\n superman ALL=(ALL) ALL\n" >> /etc/sudoers
```
// 输出文件最后3行
```
tail -3 /etc/sudoers
```
输出以下内容即表示成功添加：

> superman ALL=(ALL) ALL

建议：完成后不要登出系统，使用另一个视窗尝试登入 root 和普通帐号，测试无误便可进行下一步。

### 启用公钥验证登入 ssh

1.准备工作，**本地Mac**生成公钥：（可省略）
在我的mac终端iterm中输入以下命令：
`ssh-keygen -t rsa -b 4096`
一路回车即可。成功后会在当前用户的 .ssh/ 文件夹下找到。
一般程序员都有git帐号，肯定本地已经有SSH key了，不需要重复生成了。

关于SSHkey的知识可参考
ssh官网：[https://www.ssh.com/ssh/keygen/]
vultr官网：[https://www.vultr.com/docs/how-do-i-generate-ssh-keys/]

2.将本地的公钥扔到服务器上去
mac的终端item中：

```
scp .ssh/id_rsa.pub root@你的vps域名或者ip:~/
接下来提示你输入密码。
```

然后我们回到vps的服务器上，把把这个 id_rsa.pub 文件复制到ssh的相应目录里，然后改名为 authorized_keys。

要注意的是，你想要这对密钥登录哪个账户，就放到哪个账户下，比如我要自动登录root，那么我就会这样做：
> mkdir ~/.ssh //如果当前用户目录下没有 .ssh 目录，就先创建目录
chmod 700 ~/.ssh
mv id_rsa.pub ~/.ssh
cd .ssh
mv id_rsa.pub authorized_keys   //因为是新系统，就直接改名。如果本身就有authorized_keys文件，就需要cat id_rsa.pub >> authorized_keys。将文件内容追加入authorized_keys。
chmod 600 authorized_keys

### 关闭所有帐户ssh密码登录

> /etc/ssh/sshd_config

在如上目录找到ssh服务的配置文件，将如下字段进行修改：
> #PasswordAuthentication yes
//修改为如下内容：
PasswordAuthentication no     //所有用户都不能以密码的方式登录了。
RSAAuthentication yes
PubkeyAuthentication yes
//使用命令重启ssh服务：
/etc/init.d/ssh restart
另外，为了避免Write failed: Broken pipe错误，你也可以顺便在这个文档末尾加上下面一条命令，然后再重启 ssh 服务。
> ClientAliveInterval 60

这样，你的vps很大程度上就比较安全了，不过，如果你的vps用来做网站了，开启了httpd之类的服务，那可能你还需要

### 完成公钥登录

这样，我们就已经可以使用RSA密钥文件来登录ssh了。

```
ssh root@<vps的域名或IP>        //不需要输入密码，直接登录了。黑客再也不可能通过帐号来登入你服务器。除非你把私钥泄漏。
```



## 进阶设置

### 更换默认ssh端口
>  /etc/ssh/sshd_config

进入以上文件，注销，并新增。
>
>  #port 22
>  port xxxxx   	  //你定义的端口，任何 1024 – 65535 之间的任何数字。知名端口，常用于系统服务等，例如http服务的端口号是80

以后SSH登录VPS，就要带上端口号了（如有防火墙需，要放行此端口），像这样：
`ssh root@<vps的域名或IP> -P xxxxx`

### 安装防火墙
我们利用vultr官网自带的防火墙，开放常用端口，以及SSH的自定义端口。
Vultr后台新增的Firewall（防火墙）功能，类似阿里云的安全组功能，备份之外又一个实用功能。它可以代替Linux内置的防火墙功能，并且好用多了。

> 常用的端口
 HTTP：TCP 80		//需要
HTTPS：TCP 443     	//需要
SSH：TCP 22	//默认，无需设置，可改，
FTP：TCP 21 20
MySQL：TCP 3306

可以通过源过滤数据包，0.0.0.0/0 表示来自任何地方的数据包。如果您希望允许来自特定IP的SSH连接并拒绝其他来源，请选择“自定义”，并输入IP地址和子网掩码。

拿着"开放需要用的，能不开就不开"的原则，我们设置基本端口即可。放心填，可删改。样图如下：
![vultr防火墙设置](https://ws2.sinaimg.cn/large/006tKfTcgy1g0to2b7pm3j31800hitav.jpg)

### 关闭IPV6
IPV6比IPV4有不少优势。但目前一般小区用户都用不到。比如一些大学的教育网会用到。
但是，你不用hackers会用到，如果把这协议开着，会有潜在的危险。

>  /etc/sysctl.conf

vi 或 nano 进入以上文件，加入以下内容
```
#net.ipv6.conf.all.accept_ra = 2
＃在系统范围内的所有接口上禁用IPv6
net.ipv6.conf.all.disable_ipv6 = 1
```
保存退出，以后重启生效。

### Ddos攻击保护（vultr收费功能）
别闹了，你就拿来装梯子的，没什么宝贵的商业机密。

## 总结
虚拟机是开发者的基本配置，他或多或少都承载着一些应用，充当着重要的网络节点。而一个没有防护的节点，就像带着流血的伤口，在鲨鱼出没的水域里游泳。俗称：肉鸡。
网络虽然是自由的信息场，但也绝不是纯净的伊甸园。恩，得学会自我保护。

经过这一惊吓，好像我应该改一改一些网络平台的帐号了(捂脸

***

参考链接
* [购买了VPS之后你应该做足的安全措施](https://www.logcg.com/archives/884.html)
* [20 Ways to Secure Your Linux VPS so You Don’t Get Hacked](https://www.eurovps.com/blog/20-ways-to-secure-linux-vps/)
* [linux下ssh无密码 登陆root账户/非root账户](https://blog.csdn.net/Wflowerd/article/details/79146209)


