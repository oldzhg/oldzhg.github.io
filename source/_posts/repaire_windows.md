---
title: windows系统引导的修复
date: 2018-01-06
tags: [Tech, Windows]
---

　　最近由于安装黑苹果，经常不经意间就破坏了win10的引导，所以在多次查询相关资料后记录下每次恢复的过程与具体步骤。

### UEFI引导基本原理
#### 1、esp引导分区
 　　esp磁盘分区是gpt格式硬盘放efi引导文件的磁盘，在mbr格式硬盘中也可以由任一fat格式磁盘分区代替
#### 2、efi文件结构
    　　efi\\boot\\bootx64.efi
    　　efi\\microsoft\\boot\\bcd
#### 3、efi启动过程
　　uefi bios启动时，自动查找硬盘下esp分区的bootx64.efi，然后由bootx64.efi引导efi下的bcd文件，由bcd引导指定系统文件（一般为c:\\windows\\system32\\winload.efi）

### 修复方法
#### 用bootice手动修复
　　从efi引导启动过程来看，虽然它的文件很多，但主要用到的就是两文件，我们完全可以在各Win　pe下挂载esp分区，从硬盘系统中复制bootx64.efi文件，然后用用bootice制作好bcd，就完成efi引导修复。
1、启动任一Win　pe，用esp分区挂载器或diskgenuis挂载esp分区
2、查看esp分区是否可正常读写，如不正常可重新格式化为fat16分区格式。
3.在esp分区中建立如下空文件夹结构
　　\\efi\\boot\\   （bootx64.efi等复制）
　　\\efi\\microsoft\\boot\\（bcd等建立）
4、复制硬盘系统中的bootmgfw.efi（一般在c:\\windows\\boot\\efi下）到esp分区的\\efi\\boot\\下，并重命名为bootx64.efi
5、打开bootice软件，有esp分区的\\efi\\microsoft\\boot\\下新建立一bcd文件，
　打开并编辑bcd文件，添加“windows vista\\7\\8启动项，指定磁盘为硬盘系统盘在的盘，指定启动分区为硬盘系统分区（一般为c:）
　最后保存当前系统设置并退出。

### 注意
1. 指定启动分区不是esp分区所在分区，就是硬盘64位Win7、Win8 系统所在分区
2. 指定启动文件为\\Windows\\system32\\winload.efi 是.efi，不是.exe，要手工改过来


