---
title: iTerm2连接VPS上查看中文乱码
date: 2020-03-12 18:34:14
tags: Tech
---

mac 上用是iterm2终端, Shell 环境是zsh。ssh 到Linux 服务器上查看一些文件时，中文乱码。  这种情况一般是终端和服务器的字符集不匹配，MacOSX下默认的是utf8字符集。

解决方案如下：
输入locale可以查看字符编码设置情况，而我的对应值是空的。  而默认的.zshrc没有设置为utf-8编码，所以本地和服务器端都要在.zshrc设置，步骤如下，(bash对应.bash_profile或.bashrc文件)。

1.在终端下输入  

```
vim ~/.zshrc
```

2.在文件内容末端添加：  

```bash
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

接着重启一下终端，或者输入source ~/.zshrc使设置生效。  

连接服务器，中文显示都正常了。

