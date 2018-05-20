---
layout: post
title: python学习笔记（五）
date: 2018-05-21 
comments: true
tags: python  
---
## 4 字典
### 4.1 创建和使用
创建：
```python
phonebook = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}
```
**注意：字典的格式为key:value，key不能重复**
### 4.2 基本操作
![][dictop]

### 4.3 字典方法
01. clear()
*Q:*为什么需要这个方法？我直接赋值空字典不行吗？
*A:*在大部分情况下是可以的，但当两个变量指向同一个字典的时候，如果我希望把这个地址的字典数据清空，也就是清除所有指向这个字典的变量，而不是清空单独一个变量，这时候就要用到clear方法。
例子如下：
```python
x = {}
y = x
x['a'] = 'b'
print(y)
x = {}
print(y)
{'a': 'b'}
{'a': 'b'}
```
```python
x = {}
y = x
x['a'] = 'b'
print(y)
x.clear()
print(y)
{'a': 'b'}
{}
```
02. deepcopy
不介绍copy的意图是我也不知道直接赋值和copy有什么区别😅
然而这个deepcopy函数不能不知道，因为这是完全复制。
![][deepcopy]




[dictop]: /assets/dictop.png
[deepcopy]: /assets/deepcopy.png