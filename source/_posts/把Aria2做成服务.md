---
title: 把Aria2做成服务
date: 2018-11-24 16:48:34
tags: Tech

---
添加开机启动：

```
vi /etc/init.d/aria2c    #把Aria2做成服务
```

粘贴以下代码：

```
#!/bin/sh
### BEGIN INIT INFO
# Provides: aria2
# Required-Start: $remote_fs $network
# Required-Stop: $remote_fs $network
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Aria2 Downloader
### END INIT INFO
 
case "$1" in
start)
 
 echo -n "已开启Aria2c"
 sudo -u root aria2c --conf-path=/data/wwwroot/download/aria2.conf -D
 #sudo -u后面的是你正在使用的用户名
;;
stop)
 
 echo -n "已关闭Aria2c"
 killall aria2c
;;
restart)
 
 killall aria2c
 sudo -u root aria2c --conf-path=/data/wwwroot/download/aria2.conf -D
 #同上面的一样，根据自己的用户改-u后面的用户名
;;
esac
exit
```

保存文件把权限给为755：

```
chmod 755 /etc/init.d/aria2c
```

保存文件把权限给为755：

```
service aria2c start
```

测试Aria2服务是否可以正常启动：

```
service aria2c start
```

如果只显示“开启Aria2c”，没有其他错误提示的话就说明成功了。添加Aria2c服务到开机启动：

```
update-rc.d aria2c defaults
```

Aria2c服务命令使用说明：

```
service aria2c start //启动Aria2c
service aria2c restart //重启Aria2c
service aria2c stop /关闭Aria2c
```

