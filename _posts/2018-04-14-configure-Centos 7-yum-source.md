---
layout: post
comments: true
title: 超简单 CentOS 7 配置阿里云yum源
date: 2018-04-14 21:58:00 
tags: 运维
---


### 步骤
1. 打开centos的yum文件夹

输入命令
`cd /etc/yum.repos.d/`

2. 用wget下载repo文件

输入命令`wget http://mirrors.aliyun.com/repo/Centos-7.repo`

如果wget命令不生效，说明还没有安装wget工具，输入`yum -y install wget` 回车进行安装。

当前目录是`/etc/yum.repos.d/`，刚刚下载的Centos-7.repo也在这个目录上

3. 备份系统原来的repo文件

`mv CentOs-Base.repo CentOs-Base.repo.bak`

即是重命名 CentOs-Base.repo -&gt; CentOs-Base.repo.bak

4. 替换系统原理的repo文件

`mv Centos-7.repo CentOs-Base.repo`

即是重命名 Centos-7.repo -&gt; CentOs-Base.repo

5. 执行yum源更新命令

```bash
yum clean all
yum makecache
yum update
```
依次执行上述三条命令即配置完毕