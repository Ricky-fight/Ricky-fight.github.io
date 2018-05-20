---
layout: post
title: python学习笔记（四）
date: 2018-05-21 
comments: true
tags: python  
---
## 第三章 使用字符串
### 字符串基本操作
正好复习一下所有序列操作有哪些：索引、切片、乘法、成员资格检查、长度、最小值和最大值
上述操作都适用于字符串，这也是python学习的魅力，省心，好记。
### 生成字符串
#### format % value 格式化输出
类似于c语言，python也可以以格式化字符串的方法生成字符串：
```python
format = "hello, %s. I'm %d years old."
value = ("Ricky", 19)
print(format % value)
```
输出如下
```
hello, Ricky. I'm 19 years old.
```
优点是能一一对应，结构清晰
#### str.format() 替代
```python
str = "{3} {0} {2} {1} {3} {0}". format("be", "not","or","to")
print(str)
```
输出如下
```
to be or not to be
```
这两种也是最基本，最常见的手法。

### 指定排版格式
- 宽度:   `{:10f}`    控制宽度为10
- 精度:   `{:.2f}`    保留两位小数
- 千位分隔符:    `print("{:,d}". format(10**100))` 
- 符号:   `print("{:+10f}". format(3.14))` 
```python
from math import pi
print("{:-10f}". format(-pi))
```
可以看到成功地在有符号的情况下输出了10位结果
- 对齐
```python
from math import pi
print('{0:<10.2f}\n{0:^10.2f}\n{0:>10.2f}'.format(pi))
3.14      
   3.14   
      3.14
```

左对齐|居中|右对齐
|:-+-:|
<|^|>

- 用0填充:     `print("{:010f}". format(3.14))`
- 也可以用其他符号填充：   `print("{:¥10f}". format(3.14))`

### 字符串方法
懒得抄写一遍了，直接粘贴！反正现在上课记笔记也是拍拍拍（逃
#### 记几个特别有用的模块常量：
![][str1]
![][str2]
#### center
![][center]
#### join
![][join]
```python
a = "join"
b = '+'
print(b.join(a))
```
我想说的是，注意搞清join主客关系
#### split
非常有用！！作用是处理英语句子或者具有类似url格式的字符串时非常方便！
![][split]

[str1]: /assets/string1.png
[str2]: /assets/string2.png
[center]: /assets/center.png
[join]: /assets/join.png
[split]: /assets/split.png