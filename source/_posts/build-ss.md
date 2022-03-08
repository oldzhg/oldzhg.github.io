---
title: virmach 上搭建 ss + bbr
date: 2017-12-22 18:34:14
tags: Tech
---

#### 购买
前段时间一直在寻找一台便宜够用的VPS单单作FQ用，偶然间看到virmach打75折，于是入了一台年付仅9.37（约合人民币61.98元)。优惠码：zhujiceping25
![](//pic.oldzhg.com/uPic/price.png)

#### 安装bbr加速
输入命令：
```bash
	wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
	chmod +x bbr.sh
	./bbr.sh
```
安装完毕并重启, 执行如下命令： 
```bash
	lsmod | grep bbr
```
返回值有tcp-bbr模块即说明bbr已启动。
![](//pic.oldzhg.com/uPic/bbr.png)

#### 使用Superspeed脚本测速
脚本如下：
```bash
	yum -y install wget
	wget -qO- https://raw.githubusercontent.com/wn789/Superspeed/master/superbench.sh | bash
```
如下图所示，速度还是很快的，果然是G口。
![](//pic.oldzhg.com/uPic/test1.png)

#### 安装Shadowsocks
运行脚本：
```bash
	wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
	chmod +x shadowsocks-all.sh
	./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```
依次选择安装版本、ss密码、端口、加密方式后如图，
![](//pic.oldzhg.com/uPic/build_ss.png)
等待安装完成，
![](//pic.oldzhg.com/uPic/complete.png)
测试可用，接下来将配置文件修改为多用户多端口，执行命令：

```bash
	vi /etc/shadowsocks-python/config.json
```
下面是文件示例：
![](//pic.oldzhg.com/uPic/example.png)
修改后重启shadowsocks

```bash
	/etc/init.d/shadowsocks-python restart
```
#### 本地测试
本地20M宽带测试速度表现很好
![](//pic.oldzhg.com/uPic/test2.png)

#### 参考资料
1. Shadowsocks 一键安装脚本（四合一）
	https://shadowsocks.be/11.html
2. VPS网络优化各种方法汇总——锐速/BBR/BBR魔改版一键安装脚本
	https://www.cokemine.com/vpsyh.html
3. Superspeed一键测速脚本
	https://www.wn789.com/9504.html
4. Shadowsocks Python版一键安装脚本
	https://shadowsocks.be/1.html
