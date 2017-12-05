---
title: 解决Sublime3上无法下载Packge
---


## 问题：Sublime3上无法下载Packge

### 提示：There are no packages available for installation

### 解决方法：
<!-- more -->

1. 据说是IPv6的原因，如果我们的Intent服务提供者（ISP）不支持IPv6就会引发上述错误，原文如下：
This error is happened with IPv6 problem. If your Internet Service Provider (ISP) does not support for IPv6 you got this error.

2. （Mac OS）ping sublime.wbond.net

3. 记录IP地址并sudo vi /etc/hosts添加IP地址和域名的解析记录，即可
