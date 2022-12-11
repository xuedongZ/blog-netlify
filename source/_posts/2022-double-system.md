---
title: 安装物理双系统
date: 2022-12-11 15:37:21
tags: 
  - 电脑
---

电脑是目前人类社会最重要的生产力工具之一，是我们工作会生活的得力帮手。
为电脑准备操作系统，应对不同类型的任务，也会让我们做事的时候得心应手，总之
> Sharpening your axe will not delay your job of cutting wood.
> 说人话就是，磨刀不误砍柴工。

## 主流操作系统
主流健壮的操作系统包括 windows、mac、linux。这里不想评价哪个系统更好，都有各自的应用场景。
选择哪个涉及因素很多，我们所事工作，任务的类型，处理媒介等等因素都脱不开关系。当然对一些学生党来说，价格也是一大重要考虑因素。

但对于已经工作数年的人来说，同时使用2台以上电脑也不稀奇。甚至装了虚拟机后，更是可以部署n个操作系统，来应对不同的任务。

比如：
- 国产软件都扔到虚拟机的windows里，像微信，wps这些。
- 涉及银联的操作，也会单独准备一个虚拟机win系统。(只不过现在移动支付太发达，最近这几年几乎用不上了)。
- 对于一些财务工作者，应对不同的公司需要不同的财务软件，这些软件可能互相冲突。也可以用虚拟机装多个操作系统，来隔离操作环境。使用的时候再按需加载。
当然关于虚拟机使用更是值得开一个专篇来讲，不是本次的重点。

早年画电气施工图，专门买了联想的win笔记本。毕竟一些工程软件只有windows版本。后来转行学编程，开始用mac，才发现原来电脑可以这样的安静，一点杂音都没有，还非常流畅。后来联想笔记本运行的时候翁翁作响，而且由于系统机制的原因运行久了会变卡，使用体验和mac根本不能比。而linux系统哪怕再老旧的电脑都能流畅运行。尤其linux的桌面版丰富后，如今使用体验也是逼近mac。

## 尝试装物理双系统
为了让电脑更高效的运行，后面尝试主系统装linux，第二系统选择win10。目前已经稳定使用3年。

摸索linux的过程，也是熟悉软件知识的过程，过程遇到问题的排查能力和编程能力是通用的。对于不想花时间配置、定制的普通用户，直接选已有配套应用的桌面版linux发行版，也足够日常使用了。
不用担心数据的泄漏，也不用考虑版权问题。系统都开源，软件也基本免费。任何需求都可以找到优秀、开源的软件。

### 双系统的选择
#### 1.linux系统
linux系统有很多发行版。对于我这种偏向日常使用的普通用户，当然也选择桌面版。
如果你的的电脑实在是古董级的，配置实在低。可以选择一些轻量的发现版，如lubuntu。
我看好的主要ubuntu，和opensuse。经过比较，选中了 opensuse 的滚动发行版tumblweeed。
所谓滚动有两层含义，
- 每次版本升级，可以热更新，无需重装。
- 系统被搞挂了，可以随时恢复。（无需人工管理恢复镜像，默认每次系统更新局部软件的时候，会自动制作镜像）

这就非常节约时间。尤其装windows系统，还有一堆后续维护工作，装驱动，装一堆办公或日用大型软件...想想都头大。
更有甚者，有人还要花时间装全家桶和杀毒软件（具体不点名了）。把自己电脑有限内存和计算资源，以及隐私资料拱手相让。

#### 2.win10系统
win7 已经退役，win11对系统资源和电量消耗大，折中选择了win10 ltsb版本，相对轻量也有安全更新。

如果你是重度window用户，并且有其他特殊需求，也可以选择其他window版本。

## 双系统的安装
原本是想给弟、妹重装系统的，但想着若自己有摸索、尝试的意愿更好。我提供思路就行，网上教程满天飞，只要有甄选能力，还是能搞定的。

毕竟电脑只是工具，要干的事情比工具本身更重要。查理芒格压根就不用电脑，他的生产工具就一个计算器和一张破旧的复利表。

实在志不在此，淘宝找人装都行（只要你信得过别人是给你装的纯净的）。

好在我的装法不想完全推倒重来，只是匀出一个新的空间给linux。甚至原本自带的windows系统不用动。

### 0.准备
确保自己有良好的网络；
最好另备一台电脑（以备下载应急资料）；
一个移动硬盘，一个u盘；
### 1.电脑资料全盘备份
一般是需要格式化部分磁盘，电脑重新分区。迫不得已再选择全盘格式花。
- 背后涉及磁盘格式的知识。一共两种磁盘格式: mbr和gpt。一般默认mbr格式，只最多支持4个主分区，而gpt支持无限个主分区。
- 如果想分配超过5个主分区，就要重新全盘格式化为gpt，并且把每个磁盘的大小和数量分配成自己需要的。如果不调整分区大小，把空闲的200G用来装系统，就明显太浪费了。

哪怕用不着格式化，备份资料也可以防止安装系统时出意外。甚至，平时手边就该准备一个专门用于备份的移动硬盘。

### 2.下载系统镜像并刻录u盘启动盘。
windows镜像下载官方原版镜像：https://msdn.itellyou.cn/
linux镜像下载：对应发行版官网下载

u盘启动制作工具：Rufus
备注：
- 1.制作镜像前，比对下载文件的md5值，和官网给的md5。如果一致，说明下载过程没有被调包。
- 2.制作镜像的分区形式和实际分区形式一致。
如何查看自己磁盘的分区形式：[看这篇知乎](https://zhuanlan.zhihu.com/p/102755889)

### 3.分区
  基本思路就是，尽量保持原来的系统盘C盘，沿用原有分区格式(mbr或gpt）。从其余的 d,e,f 匀一个60G-100G的空间给linux。
  - c盘维持原有，win系统100G足够。
  - linux需要的40-60G空间（可以是60G的整个分区，也可以把该分区拆分成linux需要的多个连续分区空间。拆分不用提前，安装的时候再拆分）

关于分区的大小，发行版官网会有建议。像opensuse tumbleweed 官网建议至少 40G空余空间。若你是重度用户，需要用来开发，甚至编译一些大型软件，完全可以再多放大一些。



**磁盘分区的目的**

创建磁盘分区大概有下面几个目的：
- 提升数据的安全性(一个分区的数据损坏不会影响其他分区的数据)
- 支持安装多个操作系统
- 多个小分区对比一个大分区会有性能提升
- 更好的组织数据

关于分区，如今电脑性能和磁盘容量已经不同往日，完全可以不用在意这点优化。作为程序员也许会死磕来致敬艺术。

### 4.改bios启动方式
如果换成了gpt，要改启动类型，从bios改为UEFI。
查看系统启动引导类型： 
  > win+r 然后输入 msinfo32

类型不匹配改动：
- 进入方式：开机引导页面，狂按delete或者F12.（我的联想是F2）
  >  默认UEFI启动；关闭bootsecurity（改为disable）

- 把u盘的启动项，升为第一个。保存重启后，就会从u盘引导加载了。

### 5.从u盘引导安装系统
按提示安装重启即可。装系统就是装大号版的软件，具体查找相应发行版的视频或文字安装教程。
重启之后，只需做简单的配置工作，比如把替换更新源为国内的镜像。其他基本日用软件如音频、视频、浏览器、网卡都是现成的。基本是开箱即用。

## 结语
装系统并不值得钻研，重装率越低才说明安装、使用习惯好。不要学早年我，只顾着磨刀，不去砍柴。
另外电脑不算旧，却经常运行卡顿，可以不用想着换新电脑，花点小钱升级一下内存，立马运转如飞，如获新生。

---
参考：
1.[Window Linux 双系统安装历程](https://www.51cto.com/article/715165.html)
2.[Win10 LTSB版本区别详解 Win10 LTSB是什么版本](https://www.jb51.net/os/win10/780821.html)