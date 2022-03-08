---
title: Centos7禁用密码登录
date: 2020-02-18 18:34:14
tags: Tech
---

## 步骤

- 编辑`/etc/ssh/sshd_config`
- 将`PasswordAuthentication`参数值修改为no： `PasswordAuthentication no`
- 重启ssh服务：`systemctl restart sshd.service`

