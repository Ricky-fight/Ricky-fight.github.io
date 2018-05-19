---
layout: post
title: Python学习笔记（一）
date: 2018-05-12
comments: true 
tags: python   
---
### 数学函数
- 幂运算： `a**b` &lt;==&gt; a^b
- 取整函数：`round` `math.floor` `math.ceil`
- 复数运算： `cmath` 模块
- `sqrt(-1)` -&gt; 1j
- python 中 `j` 表示 虚数单位也就是高中我们学过的“i”
- (1 + 3j) * (9 + 4j)[1]() -&gt; (-3+31j)(复数运算不需要也可以)

### 海龟绘图法 
> 简单又强大的绘图模块，相当于logo语言内置在python里了，看到这个瞬间回想起小学学小海龟的快乐时光：）

#### 主要的几个命令
- `penup()` #铅笔抬起
- `pendown()` #铅笔落下
- `left(degree)` #角度制
- `right(degree)` #角度制
- `forward(distance)` or `fd(distance)` #前进
- `back(distance)` or  `bk(distance)` #后退
- 要了解其他的命令，请参阅“Python库参考手册”的相关部分（ [module turtle](https://docs.python.org/3/library/turtle.html)）。

#### e.g. 尝试画一个正八边形
```python
from turtle import *
for i in range(8):
	fd(100)
	lt(45)
```
效果如下：
![](/assets/turtle.png)
### 字符串
- 表示字符串有三种方法，以下三种都可以转义：单引号''，双引号""，三单引号'''（这种写法可以支持多行字符串）

[1]: Magnus Lie Hetland. Python基础教程（第3版）（图灵图书） (Kindle位置652). 人民邮电出版社. Kindle 版本.