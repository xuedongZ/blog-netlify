---
title: 为什么 Github 不显示 contribution 记录
date: 2019-04-18 10:55:10
tags:
  - 工具
---

[GitHub](https://zh.wikipedia.org/wiki/GitHub)是程序员必不可少的学习和协作平台。他的[民间用途](http://www.longtask.net/2017/12/05/github-innovation/)很多，有托管代码、文章、博客，写电子书等等功能，几乎需要天天用到。而且我们的仓库变动可以通过主页下方的绿色方块来显示，这可以粗略的看出作者在平台上的活跃程度。

说来惭愧，很长一段时间里，我的 github contribution 本地记录都没正常显示。生活往往是这样，在你不知所以然的时候，也可以有模有样的做下去。

思来想去，可能是早期的骚操作重改帐号名什么的，总之原因很简单：

> **初始设定时，提交的 email 和 github 不匹配。**

如果时光可以倒流，那就倒流一下吧...

## 正常 git 初始安装

### 1.安装 git

Mac 的操作系统 Mavericks (10.9) 以上版本中都内建有 Git，你可以在 Terminal 中通过以下命令查看是否有 Git：

```
git --version
which git
```

也可以通过 Homebrew 安装最新版本的 Git：

```
ruby -e "\$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
brew install git
```

### 2.git 本地配置

就像是首次玩游戏一样，你必须设定游戏玩家角色叫什么。这里我们必须要设定两个参数：

```
git config --global user.name "你的名字"
// 切记"你的名字"要修改成自己的名字
git config --global user.email "你的 email@xxx.xxx"
// 切记 你的 email@xxx.xxx 要修改成自己用来注册 github 的那个 email
```

这样以后进行存档时，就会纪录你是这次进度的存档者。于是，我知道哪儿了错了。

**你也许会说：我已经把 ssh 私钥放在了 github.com 中了啊。**
然而这个过程只是建立一个可以互相访问的通道而已。到底本地哪个用户向对远端 git 仓库 push，是根据你本地的 user.email 邮箱来识别的。
在 git 中 user.email 和用户名是一一对应的：
如果 user.email 在 git 中已经注册，则显示 email 对应的 git 用户名。
如果 user.email 在没有注册，则显示 user.name(为灰色)
![未识别的帐户](https://github.com/xuedongZ/blog-img/blob/img/2019/Snip20190406_1.png)

## 补救办法

大致有三步： 1.修改 github 的 email 配置 2.修改历史项目错误帐号（让以前的方块变绿） 3.建立本地新帐号（让以后的方块变绿）

### 修改 github 的 email 的配置

右上角头像-settings-emails-Add email address
改成你要用的邮箱即可。

### 修改历史项目错误帐号

事先声明：我们需要一个项目一个项目修改，且项目提交操作是在一年之内才能恢复到 contributions。不怕麻烦，觉得有必要的话，我们就继续以下操作：

1.下载项目 repo 到本地
`cd repo.git`
cd 到 repo 仓库

2.利用脚本批量改名
复制粘帖官方脚本至终端（只需要根据你自己的信息修改三个变量：`旧的Email地址`，`正确的用户名`，`正确的邮件地址`）

```
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="旧的 Email 地址"
CORRECT_NAME="正确的用户名"
CORRECT_EMAIL="正确的邮件地址"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_COMMITTER_NAME="$CORRECT_NAME"
export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_AUTHOR_NAME="$CORRECT_NAME"
export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

按`Enter`键执行脚本

3.用`git log`命令看看新 Git 历史有没有错误

4.把正确历史 push 到 Github
`git push --force --tags origin 'refs/heads/*'`

5.删掉刚刚临时创建的 clone
`cd ..`
`rm -rf repo.git`

至此，你的历史项目便救回来了...一个。

### 本地建立新帐号

最后一步，全局设置好你的正确信息，以后就放心的用 Github 进行版本管理吧 ^\_^
`git config --global user.email "你的邮件地址"`
`git config --global user.name "你的Github用户名"`

---

**参考：**

1. 官方教程：[change author info](https://help.github.com/en/articles/changing-author-info)
2. segmentfault：[为什么 Github 没有记录你的 Contributions](https://segmentfault.com/a/1190000004318632)
3. 简书：[github 用户主页不显示 contributions 记录的问题](https://www.jianshu.com/p/82ee1c341456)
