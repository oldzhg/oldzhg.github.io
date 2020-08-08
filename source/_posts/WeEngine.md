---
title: 基于ECS构建微信公众号管理系统
tags: Tech
categories: 工具技巧
date: 2020-08-08 21:01:22
---

### 下载解压

```bash
wget https://cdn.w7.cc/download/WeEngine-Laster-Online.zip
unzip -d wechat WeEngine-Laster-Online.zip
```

修改nginx默认目录到wechat,重启nginx

```bash
nginx -s reload
```

### 创建数据库

```bash
mysql -uroot -p
create database wechat;
show databases;
```

<img src="https://oss.oldzhg.com/uPic/image-20200808202241760.png" alt="image-20200808202241760" style="zoom:50%;" />

### 开始部署

打开网站，`http://ip/install.php`

<img src="https://pic.oldzhg.com/uPic/Xnip2020-08-08_20-18-00.jpg" alt="Xnip2020-08-08_20-18-00" style="zoom:50%;" />

发现目录权限不足，授予刚刚创建的wechat目录权限即可

```text
chown -R www /data/wwwroot/wechat
```

继续安装

<img src="https://pic.oldzhg.com/uPic/image-20200808202633209.png" alt="image-20200808202633209" style="zoom:50%;" />

到这里就基本完成系统的部署

### 添加公众号

剩下就是微信公众号的添加，不再赘述

完成大概这个样子

<img src="https://pic.oldzhg.com/uPic/image-20200808205941459.png" alt="image-20200808205941459" style="zoom:50%;" />


 