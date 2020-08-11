---
title: 搭建Java Web开发环境
tags: 
categories: ECS七天训练营
date: 2020-08-11 13:47:05
---
-----------

## 安装JDK

这里安装JDK1.8

```
yum -y install java-1.8.0-openjdk*
```

## 安装MySQL

1.  下载并安装MySQL
    
    ```
    wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
    yum -y install mysql57-community-release-el7-10.noarch.rpm
    yum -y install mysql-community-server
    ```
    
2.  启动MySQL
    
    ```
    systemctl start mysqld.service
    ```
    

## 安装Tomcat

### Tomcat是什么？

Tomcat是一个应用服务器。他的作用就是让你写好的Java网站在服务器上运行。

### Tomcat安装步骤

1.  下载并解压Tomcat压缩包
    
    ```
    wget https://mirror.bit.edu.cn/apache/tomcat/tomcat-8/v8.5.57/bin/apache-tomcat-8.5.57.tar.gz
    tar -zxvf apache-tomcat-8.5.57.tar.gz 
    ```
    
2.  修改Tomcat名字
    
    ```
    mv apache-tomcat-8.5.57 /usr/local/Tomcat8.5
    ```
    
3.  为Tomcat授权
    
    ```
    chmod +x /usr/local/Tomcat8.5/bin/*.sh
    ```
    
4.  修改Tomcat默认端口号（默认为8080）
    
    ```
    sed -i 's/Connector port="8080"/Connector port="80"/' /usr/local/Tomcat8.5/conf/server.xml
    ```
    
5.  启动Tomcat
    
    ```
    /usr/local/Tomcat8.5/bin/./startup.sh
    ```
    
6.  访问Tomcat
    
    访问服务器公网地址