---
title: VuePress
tags: 
categories: ECS七天训练营
date: 2020-08-06 20:24:25
---

## VuePress

轻量级文档服务器，可以用做博客和技术文档，可以部署在Github

### VuePress优点

1.  界面简洁优雅
2.  支持Markdown语法
3.  渲染静态HTML，性能优异

### 远程安装VuePress

1.  设置安全组

*   登录到阿里云服务器
*   进入控制台，选择ecs
*   选择网络与安全
*   选择安全组，选择实例，进去
*   在入方向选择手动创建
*   协议选择自定义TCP，端口范围为22/22和8080/8080，授权对象0.0.0.0/0，保存
*   退出到实例与镜像里，选择实例，查看公网ip
*   可以使用XSHELL，远程连接公网ip，输入账号密码测试连接

2.  安装node.js（官方那个太麻烦了，顶不住，顶不住）

*   在官方网站查询你需要的node的版本资源`https://github.com/nodesource/distributions`
*   给系统添加你需要安装的node资源`curl -sL https://rpm.nodesource.com/setup_10.x | bash -`
*   安装ndoe.js`yum install -y nodejs`
*   输入`node -v`和`npm -v`查看是否已经安装上了，以及版本是否正确

3.  安装VuePress

*   配置node.js的镜像（国内访问国外太慢了，依旧顶不住）`npm config set registry " https://registry.npm.taobao.org "`
*   输入`npm config get registry`查看是否配置成功
*   安装yarn包管理器`npm install -g yarn`（官方有俩种安装VuePress的方法，yarn的方法比npm直接安装的速度快）
*   通过yarn安装VuePress`npm install yarn -g`
*   这个时候出现错误，可能是node版本太低了，升级一下node版本`n stable`（升级好慢啊）
*   重新安装
*   输入`VuePress -v`查看是否安装成功

**VuePress目录结构**

![](https://pic.oldzhg.com/uPic/ibSXcC.png)

### 配置VuePress

1.  创建一个文件夹，名字为VuePress`mkdir vuepress`，进入到文件夹内部`cd vuepress`
2.  然后创建package.json文件夹`npm init -y`，这个时候会创建出package.json文件，可以ls查看一下
3.  修改package.json文件`vi package.json`
4.  修改成这样

保存，退出

5.  接着创建文件夹 docs `mkdir docs`
6.  进入到内部`cd docs`
7.  创建文件`echo '# Hello VuePress - first blog!' >README.md`文件夹`mkdir .vuepress`进入到文件夹里
8.  创建文件夹和配置文件`mkdir public`和`echo > config.js`
9.  回到最初的目录`cd ../../`
10.  然后启动vuepress`vuepress dev docs`
11.  打开浏览器输入服务器公网ip及端口号8080，即可看见刚刚写入的hello  VuePress

### 个性化定制

修改README.md⽂件，将原来的内容删除后，将以下内容拷贝进去

```
---
home: true
heroText: Vue技术博客初试
tagline: 项⽬目结构，关注讨论，每⽇日分享
actionText: 每⽇日更更新 →
actionLink: /testlink/
features:
- title: 项⽬目结构
details: 以 Markdown 为中⼼心的项⽬目结构，以最少的配置帮助你专注于写作。
- title: 关注讨论
details: 享受 Vue + webpack 的开发体验，在 Markdown 中使⽤用 Vue 组件，同
时可以使⽤用 Vue 来开发⾃自定义主题。
- title: 每⽇日分享
details: VuePress 为每个⻚页⾯面预渲染⽣生成静态的 HTML，同时在⻚页⾯面被加载的
时候，将作为 SPA 运⾏行行。
footer: LearnVueonECS Licensed | Copyright © 2020-present
---
```

演示效果如下

![image-20200806201815666](https://oss.oldzhg.com/uPic/image-20200806201815666.png)

你也可以自己继续换主题，定制插件让样式更多样，在官网上就有介绍[VuePress官网](https://www.vuepress.cn/)

