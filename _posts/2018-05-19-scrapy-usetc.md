---
layout: post
title: 使用 Scrapy 爬虫监控教务处公告栏
date: 2018-05-19
comments: true 
tags: python   
---

### Introduction
自从某次因为没看到教务处公告错失良机，我就一直想写一个程序监控教务处发布的公告。正好学习 Scrapy 和 mysql 的使用，和最近比较火的 GitHub 上 Python 开源项目 Zmail，可以让我用发邮件的形式提醒我有新公告。

### 分析
在使用 scrapy 前要分析一下教务处网页的结构，右键任一公告链接，点击审查元素（Mac:Chrome上是检查）![][pagestruct]查看网页结构：
![][pagedev] 发现公告在某个类型为`textAreo clearfix`的`div`标签内的`a`标签，属性都帮我们写好了，一个newsid是唯一识别编号，一个title可以让我方便地填充邮件标题，公告是否是新发布的也非常好判断，只要遍历之前的公告newsid是否有未存入数据库的就可以了。接下来就开始动手做监控脚本。
### 大致步骤
0. 初始化，用爬虫的follow功能先爬取公告栏59页所有的newsid和title存入数据库作为初始数据
1. Scrapy爬取教务处学生公告第一页，获取到title和newsid
2. 存入 mysql 数据库
3. 每2小时Scrapy爬取教务处公告栏页面
4. 逐个遍历数据库中newsid是否已经存在
5. 不存在则存入数据库并发邮件提醒，附上链接（因为很大一部分通知带有附件，我还没研究过如何把附件自动下载下来，就先不自动爬取内容了吧，减少工作量）
6. 定时启动脚本，检查是否有新公告

### 技术细节
天坑啊！要说清楚其实挺麻烦的，慢慢写吧...就把它当成未来技术面试能力的积累...  
主要用到的功能模块主要有3个：

1. scrapy爬取教务处页面
2. pymysql+mysql负责存储和查询公告数据
3. 用到了github上国人开发的开源邮件项目zmail，负责发邮件
4. crontab在我电脑上设置定时运行脚本


[pagestruct]: ../assets/pagestruct.png
[pagedev]: ../assets/pagedev.png