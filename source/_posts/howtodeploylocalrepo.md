---
title: 如何创建CentOS7本地镜像源
date: 2017-12-12 12:42:41
tags:
---

摘要：创建CentOS 7的本地镜像源的过程
<!-- more -->

## 安装环境
操作系统：CentOS 7.2.1511
安装路径：/home/localrepo （保证有150G的剩余空间即可）
计划提供CentOS 7镜像同步功能

## 安装软件

### nginx

```
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum install -y nginx
```

修改配置文件
```
vi /etc/nginx/conf.d/default.conf
```

注释server_name
增加
```
location / {
    root /home/localrepo;
    index index.html index.htm;
    autoindex on;
    autoindex_exact_size off;
    autoindex_localtime  on;
}
```

配置完成后启动nginx
```
nginx
```

重新读取配置文件
```
nginx -s reload
```

关闭防火墙
```
systemctl disable firewalld
systemctl stop firewalld
```

### 安装createrepo
```
yum install -y createrepo
```

## 编写同步脚本

### 编写rsync.sh

```
vi /home/rsync.sh

#填入内容
#epel
cd /home/localrepo/ && rsync -avzR --exclude-from=/home/exclude.list rsync://mirrors.tuna.tsinghua.edu.cn/epel/7/ /home/localrepo/epel
createrepo /home/localrepo/epel/7/x86_64/

#centos7-base
cd /home/localrepo/ && rsync -avzR --exclude-from=/home/exclude.list rsync://mirrors.tuna.tsinghua.edu.cn/centos/7/os/x86_64/ /home/localrepo/centos
createrepo /home/localrepo/centos/7/os/x86_64/

#centos7-updates
cd /home/localrepo/ && rsync -avzR --exclude-from=/home/exclude.list rsync://mirrors.tuna.tsinghua.edu.cn/centos/7/updates/x86_64/ /home/localrepo/centos
createrepo /home/localrepo/centos/7/updates/x86_64/

#centos7-extras
cd /home/localrepo/ && rsync -avzR --exclude-from=/home/exclude.list rsync://mirrors.tuna.tsinghua.edu.cn/centos/7/extras/x86_64/ /home/localrepo/centos
createrepo /home/localrepo/centos/7/extras/x86_64/

#centos7-centosplus
cd /home/localrepo && rsync -avzR --exclude-from=/home/exclude.list rsync://mirrors.tuna.tsinghua.edu.cn/centos/7/centosplus/x86_64/ /home/localrepo/centos
createrepo /home/localrepo/centos/7/centosplus/x86_64/

chmod +x /home/localrepo/rsync.sh

```

### 创建例外清单
这些不会被同步下载

```
vi /home/exclude.list

#填入内容
SRPMS
aarch64
ppc64
ppc64le
debug
repodata
EFI
LiveOS
images
isolinux
CentOS_BuildTag
EULA
GPL
RPM-GPG-KEY-CentOS-7
RPM-GPG-KEY-CentOS-Testing-7
drpms
```

### 开始执行同步到本地
/home/rsync.sh

![001.png](001.png)

## 配置定时同步

### crontab
每天凌晨2点运行一次rsync.sh

```
vi /homr/rsync.cron

#每天2点执行一次rsync.sh
00 2 * * * /home/rsync.sh >> /home/rsync_$(date +\%Y\%m\%d_\%H\%M\%S).log >&1


crontab /home/rsync.cron > ~/log
```

## 客户端配置

### 备份以前的repo文件

```
cp -r /etc/yum.repos.d/ /root/
rm -rf /etc/yum.repos.d/
```

### 新建repo文件

```
vi /etc/yum.repos.d/home.repo

#填入内容
[epel-home]
name=name=Terence homebase mirror
baseurl=http://172.168.3.246/epel/7/x86_64/
enabled=1
gpgcheck=0

[centos7-base]
name=name=Terence homebase mirror
baseurl=http://172.168.3.246/centos/7/os/x86_64/
enabled=1
gpgcheck=0

[centos7-updates]
name=name=Terence homebase mirror
baseurl=http://172.168.3.246/centos/7/updates/x86_64/
enabled=1
gpgcheck=0

[centos7-extras]
name=name=Terence homebase epel mirror
baseurl=http://172.168.3.246/centos/7/extras/x86_64/
enabled=1
gpgcheck=0

[centos7-centosplus]
name=name=Terence homebase epel mirror
baseurl=http://172.168.3.246/centos/7/centosplus/x86_64/
enabled=1
gpgcheck=0
```

### 清除缓存
```
yum clean all
```

### 重建缓存
```
yum makecache
```

## 查询验证

```
yum info git

可安装的软件包
名称    ：git
架构    ：x86_64
版本    ：1.8.3.1
发布    ：12.el7_4
大小    ：4.4 M
源    ：centos7-updates
简介    ： Fast Version Control System
网址    ：http://git-scm.com/
协议    ： GPLv2
描述    ： Git is a fast, scalable, distributed revision control system with an
         : unusually rich command set that provides both high-level operations
         : and full access to internals.
         :
         : The git rpm installs the core tools with minimal dependencies.  To
         : install all git packages, including tools for integrating with other
         : SCMs, install the git-all meta-package.

```

如果看到源显示为centos7-updates说明架设成功了

## 获取7.2.1511光盘

因为网络上没有7.2.1511的公开源，因此就下载了CentOS-7-x86_64-Everything-1511.iso
并拷贝到本地，再提供yum源

```
cp CentOS-7-x86_64-Everything-1511.iso /home/localrepo/centos/
mkdir /home/localrepo/centos/7.2.1511
mount -o loop /home/localrepo/centos/CentOS-7-x86_64-Everything-1511.iso /mnt
cp -rf /mnt/* /home/localrepo/centos/7.2.1511

vi /etc/yum.repos.d/home.repo

[centos7.2.1511]
name=name=Terence homebase epel mirror
baseurl=http://172.168.3.246/centos/7.2.1511/
enabled=1
gpgcheck=0

```
