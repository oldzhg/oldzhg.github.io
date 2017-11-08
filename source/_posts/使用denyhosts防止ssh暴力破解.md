

# 

title: 使用denyhosts防止ssh暴力破解
date: 2020-02-24 16:48:34
tags: Tech

------

配合[DenyHosts防御ssh暴力破解](https://zhuanlan.zhihu.com/p/36404653)阅读搭建更快



*当你的 Linux 服务器暴露在互联网之中,该服务器将会遭到互联网上的扫描软件进行扫描,并试图猜测SSH登录口令。你会发现,每天会有多条SSH登录失败纪录。那些扫描工具将对你的服务器构成威胁,你必须设置复杂登录口令,并将尝试多次登录失败的IP给阻止掉,让其在一段时间内不能访问该服务器。*

## 1. denyhosts介绍

`denyhosts`是`Python`语言写的一个程序，它会分析`sshd`的日志文件（`/var/log/secure`），当发现重 复的攻击时就会记录`IP`到`/etc/hosts.deny`文件，从而达到自动屏`IP`的功能。

## 2. 下载和安装



```
$ wget https://nchc.dl.sourceforge.net/project/denyhosts/denyhosts/2.6/DenyHosts-2.6.tar.gz  #下载
$ tar -zxvf DenyHosts-2.6.tar.gz  #解压
$ cd DenyHosts-2.6 #进入解压目录
$ python setup.py install #编译安装

#拷贝和修改默认配置文件的名称
$ cd /usr/share/denyhosts/ #进入安装目录
$ cp denyhosts.cfg-dist denyhosts.cfg
$ cp daemon-control-dist daemon-control
```

## 3. 配置

此配置为可选项，因为原本就已经有默认的配置了。



```
#3.编辑配置文件denyhosts.cfg
$ vim denyhosts.cfg   #该配置文件结构比较简单，如果不想瞎折腾可以参照如下配置即可：

PURGE_DENY = 1d       #ip被禁止之后，多久可以释放(w表示周，d表示天，h表示小时，m表示分钟)
DENY_THRESHOLD_INVALID = 5 #无效用户名限制登陆次数
DENY_THRESHOLD_VALID = 10 #有效用户名限制登陆次数
DENY_THRESHOLD_ROOT = 5 #root限制登陆次数
AGE_RESET_ROOT = 1d  #root用户登录失败计数归零的时间
```

更多配置说明：



```
############ THESE SETTINGS ARE REQUIRED ############
#sshd的日志文件
SECURE_LOG = /var/log/secure 
#将阻止IP写入到hosts.deny,所以这个工具只支持 支持tcp wrapper的协议     
HOSTS_DENY = /etc/hosts.deny 
#当一个IP被阻止以后，过多长时间被自动解禁。可选如3m（三分钟）、5h（5小时）、2d（两天）、8w（8周）、1y(一年)
PURGE_DENY = 1w 
#阻止服务名   
BLOCK_SERVICE  = sshd
#定义了某一IP最多被解封多少次。即某一IP由于暴力破解SSH密码被阻止/解封达到了PURGE_THRESHOLD次，则会被永久禁止；
PURGE_THRESHOLD = 20
#允许无效用户登录失败的次数     
DENY_THRESHOLD_INVALID = 5
#允许普通有效用户登录失败的次数   
DENY_THRESHOLD_VALID = 10  
#允许root登录失败的次数  
DENY_THRESHOLD_ROOT = 1   
#设定 deny host 写入到该资料夹   
DENY_THRESHOLD_RESTRICTED = 1
#将deny的host或ip记录到work_dir中      
WORK_DIR = /var/lib/denyhosts      
SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS=YES
#是否做域名反解   
HOSTNAME_LOOKUP=YES  
#将DenyHost启动的pid记录到LOCK_FILE中，已确保服务正确启动，防止同时启动多个服务  
LOCK_FILE = /var/lock/subsys/denyhosts    

############ THESE SETTINGS ARE OPTIONAL ############
#设置管理员邮件地址 例如****@163.com
ADMIN_EMAIL = root
SMTP_HOST = localhost
SMTP_PORT = 25
SMTP_FROM = DenyHosts <nobody@localhost>
SMTP_SUBJECT = DenyHosts Report from $[HOSTNAME]

# 有效用户登录失败计数归零的时间
AGE_RESET_VALID=5d  
# ROOT用户登录失败计数归零的时间
AGE_RESET_ROOT=25d  
# 用户的失败登录计数重置为0的时间(/usr/share/denyhosts/restricted-usernames)
AGE_RESET_RESTRICTED=25d  
# 无效用户登录失败计数归零的时间
AGE_RESET_INVALID=10d

######### THESE SETTINGS ARE SPECIFIC TO DAEMON MODE  ##########
#denyhost服务日志文件
DAEMON_LOG = /var/log/denyhosts  

DAEMON_SLEEP = 30s 
#该项与PURGE_DENY 设置成一样，也是清除hosts.deniedssh 用户的时间,区别是以daemon模式运行时跟PURGE_DENY配合使用，自动解禁才能生效
DAEMON_PURGE = 1h
```

## 4.启动denyhosts



```
$ ./daemon-control start
```

可以使用命令`ps -ef | grep denyhosts`查看是否运行成功。如下结果表示运行成功。



```
[root@host denyhosts]# ps -ef | grep denyhosts
root     17588     1  0 11:57 ?        00:00:00 python /usr/bin/denyhosts.py --daemon --config=/usr/share/denyhosts/denyhosts.cfg
root     17611 24100  0 12:07 pts/1    00:00:00 grep --color=auto denyhosts
[root@host denyhosts]#
```

## 5.设置开机自动启动

让`denyhosts`每次重启后自动启动。设置自动启动可以通过两种方法进行。

第一种是将`denyhosts`作为守护进程服务运行，这种方法可以通过`/etc/init.d/denyhosts`命令来控制其状态。方法如下：



```
$ cd /etc/init.d
$ ln -s /usr/share/denyhosts/daemon-control denyhosts
$ service denyhosts start
```

或者用以下命令通过`chkconfig`工具来管理更方便。



```
$ cd /etc/init.d
$ chkconfig --add denyhosts
$ chkconfig --level 2345 denyhosts on
```

第二种是将`denyhosts`直接加入`rc.local`中自动启动（类似于`Windows`中的“启动文件夹”）：



```
$ echo '/usr/share/denyhosts/daemon-control start' >> /etc/rc.local
```

如果想查看已经被阻止的`IP`，查看`/etc/hosts.deny` 文件即可。如下：



```
[root@host ~]# cat /etc/hosts.deny
#
# hosts.deny	This file contains access rules which are used to
#		deny connections to network services that either use
#		the tcp_wrappers library or that have been
#		started through a tcp_wrappers-enabled xinetd.
#
#		The rules in this file can also be set up in
#		/etc/hosts.allow with a 'deny' option instead.
#
#		See 'man 5 hosts_options' and 'man 5 hosts_access'
#		for information on rule syntax.
#		See 'man tcpd' for information on tcp_wrappers
#
# DenyHosts: Sat Jan 12 12:22:17 2019 | sshd: 120.35.33.27
sshd: 120.35.33.27
[root@host ~]#
```

## 6. 解禁ip

编辑`/etc/hosts.deny`。然后删除或者注释掉对应的`sshd`即可。如以下用注释的方式解禁：



```
[root@host ~]# vim /etc/hosts.deny
# hosts.deny	This file contains access rules which are used to
#		deny connections to network services that either use
#		the tcp_wrappers library or that have been
#		started through a tcp_wrappers-enabled xinetd.
#
#		The rules in this file can also be set up in
#		/etc/hosts.allow with a 'deny' option instead.
#
#		See 'man 5 hosts_options' and 'man 5 hosts_access'
#		for information on rule syntax.
#		See 'man tcpd' for information on tcp_wrappers
#
# DenyHosts: Sat Jan 12 12:22:17 2019 | sshd: 120.35.33.27
#sshd: 120.35.33.27
[root@host ~]#
```

如果想删除一个已经禁止的主机IP,只在`/etc/hosts.deny`删除是没用的。需要进入`/usr/share/denyhosts/data/` 目录对以下文件一个个删除你想取消的主机IP才行。

> /usr/share/denyhosts/data/hosts
> /usr/share/denyhosts/data/hosts-restricted
> /usr/share/denyhosts/data/hosts-root
> /usr/share/denyhosts/data/hosts-valid
> /usr/share/denyhosts/data/users-hosts

你可以执行命令`sudo grep 120.35.33.27 /usr/share/denyhosts/data/*`查看具体文件ip为“120.35.33.27”的具体位置

但如果这样一个有个操作确实很麻烦，不过机智的我在网上找了一个脚本(`denyhosts_removeip.sh`)，这个脚本可以快速的解决这个问题。其内容如下：



```
#!/bin/bash
# Author:licess
# Website:https://www.vpser.net & https://lnmp.org
HOST=$1
if [ -z "${HOST}" ]; then
    echo "Usage:$0 IP"
    exit 1
fi
 
echo "Remove IP:${HOST} from denyhosts..."
/etc/init.d/denyhosts stop
echo '
/etc/hosts.deny
/usr/share/denyhosts/data/hosts
/usr/share/denyhosts/data/hosts-restricted
/usr/share/denyhosts/data/hosts-root
/usr/share/denyhosts/data/hosts-valid
/usr/share/denyhosts/data/users-hosts
' | grep -v "^$" | xargs sed -i "/${HOST}/d"
 
#iptables -D INPUT -s ${HOST} -p tcp -m tcp --dport 22 -j DROP
echo " done"
/etc/init.d/denyhosts start
```

执行脚本清除denyhosts某个IP的命令如下：



```
bash denyhosts_removeip.sh 10.10.10.10
```

参考教程：

1. http://coolnull.com/2068.html
2. https://www.spamcage.com/denyhosts.html
