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
在使用 scrapy 前要分析一下教务处网页的结构，

### 架构

Scrapy爬取教务处学生公告第一页，获取到title和newsid--> 存入 mysql 数据库  --> 每小时Scrapy爬取 --> 逐个遍历数据库中newsid是否已经存在 --> 不存在则存入数据库并发邮件提醒，附上链接

