---
title: 如何在mac上安装hexo
toc: true
tags: "Hexo使用"
---

mac OS: High Sierra

<!-- more -->

## 安装node
node.js V8.9.1
This package will install:
1. Node.js v8.9.1 to /usr/local/bin/node
2. npm v5.5.1 to /usr/local/bin/npm


## 安装hexo
### 设置淘宝镜像，加快安装速度
```
$ sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 安装hexo
```
$ sudo cnpm install -g hexo-cli
```

### 配置hexo
```
$ hexo init DIR
$ cd DIR
$ npm install
```

### 生成静态文件
```
$ hexo generate
```
或
```
$ hexo g
```

### 运行本地服务器
```
$ hexo start
```
或
```
$ hexo s
```

### 打开浏览器查看效果
http://localhost:4000




