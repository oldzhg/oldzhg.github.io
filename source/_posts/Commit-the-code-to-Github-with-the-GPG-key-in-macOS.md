---
title: 在 macOS 中使用 GPG 密钥提交代码至 Github
tags: Tech
categories: 工具技巧
date: 2020-03-20 14:36:11
---

# 

普通的 HTTPS 和 RSA 密钥方式提交代码，并不能判断是谁提交的代码，多人可以使用同一套验证进行提交，而 GPG 密钥就可以避免此问题，可以对提交者进行验证。

创建密钥
----

### 安装工具

macOS 上创建密钥需要安装 GPG 命令行工具或者 GUI 工具，下载地址：[点击跳转](https://www.vvave.net/go/aHR0cHM6Ly9ncGd0b29scy5vcmcv)。安装完成后即可使用终端生成 GPG 密钥

### 生成密钥对

创建一个 GPG 密钥对，因为 GPG 有多个版本，因此可能需要查阅官方的支持手册来获得适用的命令，GitHub 适用的密钥必须适用 RSA 密钥且具有 4096 位长度。

*   如果使用的是 2.1.17 及以后版本，可以使用下述命令进行生成。

```
$ gpg --full-generate-key
```

*   如果使用的不是 2.1.17 及以后的版本，可使用以下命令。

```
$ gpg --default-new-key-algo rsa4096 --gen-key
```

1.  然后根据屏幕提示确认使用的加密方式，默认为 RSA 。接下来确认密钥对强度，选择为 4096。
2.  输入用户的信息并进行确认。
3.  输入个安全的密码。

### 查看密钥对

```
$ gpg --list-secret-keys --keyid-format LONG
```

从列表中即可看到本机存在的全部 GPG 密钥对，假定以 ID 为 3AA5C34371567BD2 的密钥对为例。

```
$ gpg --list-secret-keys --keyid-format LONG
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

导出密钥对信息

```
$ gpg --armor --export 3AA5C34371567BD2
```

> 小贴士：密钥对信息会以 `-----BEGIN PGP PUBLIC KEY BLOCK-----` 字符串开头，以 `-----END PGP PUBLIC KEY BLOCK-----` 字符串结束。

添加密钥
----

将复制出来的密钥串粘贴至 GitHub 的密钥页面中，[点击跳转](https://www.vvave.net/go/aHR0cHM6Ly9naXRodWIuY29tL3NldHRpbmdzL2tleXM=)。

提交代码
----

查看本地密钥对

```
$ gpg --list-secret-keys --keyid-format LONG
```

为本地仓库配置 GPG 密钥

```
$ git config --global user.signingkey 3AA5C34371567BD2
```

> 小贴士：本文以 `3AA5C34371567BD2` 为例，请输入真实的密钥对 ID。

为当前的项目配置密钥认证

```
$ git config commit.gpgsign true
```

为全部项目配置密钥认证

```
$ git config --global commit.gpgsign true
```

> 小贴士：配置后使用提交命令即可在 GitHub 网页中看到 出现 **Verified** 绿色标记。


--

## 参考链接

*   [Generating a new GPG key - GitHub Help](https://www.vvave.net/go/aHR0cHM6Ly9oZWxwLmdpdGh1Yi5jb20vZW4vYXJ0aWNsZXMvZ2VuZXJhdGluZy1hLW5ldy1ncGcta2V5)
*   [使用 GPG 密钥验证 Github 提交 - CSDN](https://www.vvave.net/go/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTEwNTQzMzMvYXJ0aWNsZS9kZXRhaWxzLzgzOTM0MzA5)

