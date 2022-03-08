---
title: 如何在Mac上禁止ip
date: 2020-03-20 18:34:14
tags: Tech
categories: 工具技巧
---

 最近需要在Mac上禁止几个IP的通过，在网上搜到大部分都是通过编辑hosts文件禁用域名的访问。

经过一发查找后发现可以通过Mac上的pf服务达到禁用的目的。

具体pf的命令通过手册查看：

```bash
man pfctl
```

首先用root 权限修改 `/etc/sysctl.conf` 文件：

```bash
sudo vim /etc/pf.conf
```

不用看里面的内容，在最下面添加以下文本，记得替换成你想屏蔽的ip：

```bash
# 屏蔽ip
block drop from any to 17.253.114.253
block drop from any to 131.6.76.53
```

使用-f命令为pf服务启动指定的conf文件

```bash
sudo pfctl -f file
#Load the rules contained in file.  This file may contain macros, tables, options, and normalization, queueing, translation, and filtering rules.
```

使用 `-e` 命令启用 pf 服务。使用 `-E` 命令强制重启 pf 服务，使用 `-d` 命令关闭 pf。

现在我们启动pf服务

```
sudo pfctl -ef /etc/pf.conf
```

此时发现再去ping这几个ip已经ping不通了

![](//pic.oldzhg.com/uPic/syEbA6.png)

从 Mavericks 起 pf 服务不再默认开机自启。

如果我们想实现开机自动启动该服务，首先需要关闭系统的SIP，网上有很多方法，这里不再赘述。

然后修改 `/System/Library/LaunchDaemons/com.apple.pfctl.plist` 

当尝试编辑该文件时发现系统是不可读状态，原因是从macOS 10.15开始系统将默认被挂载为只读。

![](//pic.oldzhg.com/uPic/4GbGTH.png)

文件详细里面多了一条restricted，被限制写的操作。

还好还有另外的方法，以读写模式重新挂载系统即可。

```bash
sudo mount -uw /  
killall Finder  
```

随后重新打开向 plist 文件中添加 `-e` 行，如下所示：

```
<string>pfctl</string>
<string>-e</string>
<string>-f</string>
<string>/etc/pf.conf</string> 
```

下次系统启动时可以自动启动pf服务。