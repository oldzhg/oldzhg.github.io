---
title: Sublime Text3 使用技巧教程
date: 2018-05-07 11:59:27
tags: 转载
---

## 1、快速查找&替换

多数情况下，我们需要查找文中某个关键字出现的其它位置，这时并不需要重新将该关键字重新输入一遍然后搜索，我们只需要使用 `Ctrl + D` 选中关键字，再次 `Ctrl + D` 或者使用 `F3` 选择该词出现的下一个位置，`Shift + F3` 跳到其上一个出现位置，此外还可以用 `Alt + F3` 选中其出现的所有位置（之后可以进行多重编辑，也就是快速替换）。


## 2、快速打开文件

`Ctrl+P` 快捷键可以快速打开文件，输入文件名称即可切换打开了，因为可能我们一次打开的窗口比较多，`Ctrl+P` 快捷键可以快速的切换到相应的文件。



## 3、打开整个工程

文件——》打开文件夹——》选择自己的工程，可以打开整个工程，这里的话就可以在整个工程里边查看所有文件了，修改方便。你也可以直接将文件夹拖到侧边栏中。



## 4、快捷键列表（Shortcuts Cheatsheet）

#### 通用（General）

- ↑↓←→：上下左右移动光标，vim编辑模式下可以使用KJHL。
- Ctrl + /：注释
- Alt：调出菜单
- Ctrl + Shift + P：调出命令板（Command Palette）
- Ctrl + `：调出控制台
- Ctrl + k + b   、Ctrl + k,Ctrl + b：打开、关闭侧边栏

#### 编辑（Editing）

- Ctrl + Enter：在当前行下面新增一行然后跳至该行
- Ctrl + Shift + Enter：在当前行上面增加一行并跳至该行
- Ctrl + ←/→：进行逐词移动
- Ctrl + Shift + ←/→进行逐词选择
- Ctrl + ↑/↓移动当前显示区域
- Ctrl + Shift + ↑/↓移动当前行



#### 选择（Selecting）

- Ctrl + D：选择当前光标所在的词并高亮该词所有出现的位置，再次Ctrl + D选择该词出现的下一个位置，在多重选词的过程中，使用Ctrl + K进行跳过，使用Ctrl + U进行回退，使用Esc退出多重编辑
- Ctrl + Shift + L：将当前选中区域打散
- Ctrl + J：把当前选中区域合并为一行
- Ctrl + M：在起始括号和结尾括号间切换
- Ctrl + Shift + M：快速选择括号间的内容
- Ctrl + Shift + J：快速选择同缩进的内容
- Ctrl + Shift + Space：快速选择当前作用域（Scope）的内容



#### 查找&替换（Finding&Replacing）

- Ctrl + F/H：进行标准查找/替换，之后：
  - F3：跳至当前关键字下一个位置
  - Shift + F3：跳到当前关键字上一个位置
  - Alt + F3：选中当前关键字出现的所有位置
  - Alt + C：切换大小写敏感（Case-sensitive）模式
  - Alt + W：切换整字匹配（Whole matching）模式
  - Alt + R：切换正则匹配（Regex matching）模式
  - Ctrl + Shift + H：替换当前关键字
  - Ctrl + Alt + Enter：替换所有关键字匹配
- Ctrl + Shift + F：多文件搜索&替换

在搜索框输入关键字后 Enter 跳至关键字当前光标的下一个位置，Shift + Enter 跳至上一个位置，Alt + Enter 选中其出现的所有位置（同样的，接下来可以进行快速替换）。

使用 Ctrl + H 进行标准替换，输入替换内容后，使用 Ctrl + Shift + H 替换当前关键字，Ctrl + Alt + Enter 替换所有匹配关键字。


#### 跳转（Jumping）

- Ctrl + P：跳转到指定文件，输入文件名后可以：
  - @ 符号跳转：输入@symbol跳转到symbol符号所在的位置
  - \# 关键字跳转：输入#keyword跳转到keyword所在的位置
  - : 行号跳转：输入:12跳转到文件的第12行。
- Ctrl + R：跳转到指定符号
- Ctrl + G：跳转到指定行号



#### 窗口（Window）

- Ctrl + Shift + N：创建一个新窗口
- Ctrl + N：在当前窗口创建一个新标签
- Ctrl + W：关闭当前标签，当窗口内没有标签时会关闭该窗口
- Ctrl + Shift + T：恢复刚刚关闭的标签



#### 屏幕（Screen）

- F11：切换普通全屏
- Shift + F11：切换无干扰全屏
- Alt + Shift + 2：进行左右分屏
- Alt + Shift + 8：进行上下分屏
- Alt + Shift + 5：进行上下左右分屏
- 分屏之后，使用Ctrl + 数字键跳转到指定屏，使用Ctrl + Shift + 数字键将当前屏移动到指定屏

作者：常大鹏

链接：https://www.jianshu.com/p/aab80e642960

來源：简书
