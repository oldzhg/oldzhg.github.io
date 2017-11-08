---
title: VPS上安装rclone实现挂载谷歌云盘
date: 2020-03-15 18:34:14
tags: Tech
---

## rclone安装

```bash
curl https://rclone.org/install.sh | bash

rclone config

#新建本地文件夹，路径自己定，即下面的LocalFolder
mkdir /root/GoogleDrive
#挂载为磁盘，下面的DriveName、Folder、LocalFolder参数根据说明自行替换
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 &

rclone mount Kid:/ /data/wwwroot/directory/GoogleDrive --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 &

#卸载
fusermount -qzu LocalFolder
```

## 开启自启动

先新建`systemd`配置文件，适用`CentOS 7`、`Debian 8+`、`Ubuntu 16+`。

再使用命令：

```
#将后面修改成你上面手动运行命令中，除了rclone的全部参数
command="mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000"
#以下是一整条命令，一起复制到SSH客户端运行
cat > /etc/systemd/system/rclone.service <<EOF
[Unit]
Description=Rclone
After=network-online.target

[Service]
Type=simple
ExecStart=$(command -v rclone) ${command}
Restart=on-abort
User=root

[Install]
WantedBy=default.target
EOF
```

开始启动：

```
systemctl start rclone
```

设置开机自启：

```
systemctl enable rclone
```

其他命令：

```
重启：systemctl restart rclone
停止：systemctl stop rclone
状态：systemctl status rclone
```

如果你想挂载多个网盘，那么将`systemd`配置文件的`rclone.service`改成`rclone1.service`即可，重启动什么的同样换成`rclone1`。

## 备注

- 别把挂载盘当下载目录，可以下载到其它目录后，移动进挂载盘



