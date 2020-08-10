---
title: MediaWiki
tags: 
categories: ECS七天训练营
date: 2020-08-07 19:59:20
---

## 安装运行环境

这里建议[onestack](https://oneinstack.com)一键脚本搭建LNMP环境

## 下载MediaWiki

```bash
wget https://releases.wikimedia.org/mediawiki/1.29/mediawiki-1.29.1.tar.gz
tar -zxvf mediawiki-1.29.1.tar.gz
```

## 配置nginx

修改一下目录的权限

```bash
chown -R www /data/wwwroot/mediawiki
```

再改一下nginx.conf的默认目录到mediawiki

## 安装MediaWiki

输入公网ip就可以啦，按照指示很快就能弄好

![](https://pic.oldzhg.com/uPic/BPWBCn.png)

安装成功的样子

![Xnip2020-08-07_20-09-05](https://oss.oldzhg.com/uPic/Xnip2020-08-07_20-09-05.jpg)

