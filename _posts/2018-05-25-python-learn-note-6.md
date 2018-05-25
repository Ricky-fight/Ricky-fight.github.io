---
layout: post
title: python学习笔记（六）
date: 2018-05-25
comments: true
tags: python  
---
## 第五章 条件、循环及其他语句
### 重提 print 
除了最常见的`print(xxx)`用法， print函数还有一些对我们非常有利的特性。
#### 打印多个参数
```python
>>> print(' Age:', 42) 
Age:  42
```
可以看到，print打印多个参数时会在参数之间**自动添加空格**
如果你**不想**要自带的空格或者想用别的符号，设置`sep`属性可以自定义分隔符号来替代空格：

```
>>> print(" I", "wish", "to", "register", "a", "complaint", sep="_") 
I_ wish_ to_ register_ a_ complaint

```
#### 不想换行？
每次print都会自动换行，我不想换行怎么办？
简单！看我变魔术（雾
```python
print(' Hello,', end='') 
print(' world!')
```
结果输出Hello, world!

## 解包
python 的解包特性使我们可以同时给多个变量赋值
```python
>>> x,y,z = 1,2,3
>>> print(x,y,z)
1 2 3
```
这个特性引出一个交换变量的技巧：
```python
>>> x, y = y, x
>>> print(x,y)
1 2
```
还有字典的解包：
```python
key,value = somedict.popitem()
```
如果你**不确定解包的item里有多少个元素**
可以使用星号运算符（*）来收集多余的值。
```python
>>> a, b, *rest = [1, 2, 3, 4] 
>>> rest
[3, 4]
```
### 链式赋值
```python
x = y = somefunction()
```
*注意，x，y此时会指向同一对象，x只是y的别名，尽量别这样做。因为多数时间你想要的并不是给y取个别名，而是两个不同的对象。更好的做法可能是这样的：*
```python
x = somefunction() 
y = somefunction()
```
### 增强赋值
如+=、-=、*=、/=。。。。
此操作与c语言的行为可能相同。

### 缩进
python 的缩进可以说是最优美的也是最坑的地方。
在其他语言中，很多语言都使用很明显的特殊关键词或字符来表示一个代码块的开始位置，比如 pascal 的begin，C 的 { ，和代码块的结束位置，比如pascal 的 end， C 的 }。
而在python中，使用*：*来表示一个子代码块的开始，接下来的代码块都要*缩进一个tab*。当你不再缩进一个tab，就表示你的这个代码块结束了。

### 条件语句
格式：
```
if {condition 1}:
    # code 1
    # code 2
    ......
elif {condition 2}:
    # code 3
    # code 4
    ......
else:
    # code 5
    # code 6
    ......
```

### 条件运算符
![][cp1]
![][cp2]

与 赋值 一样， Python 也 支持*链式*比较： 可同 时使用多个比较运算符，如 0 < age < 100。

### is 相同运算符
相信接受过高中物理洗礼的同学都可以理解相同（is）和相等（==）的区别。
给出书上的例子就可以很好的理解：

```python
>>> x = y = [1, 2, 3] 
>>> z = [1, 2, 3] 
>>> x == y 
True
>>> x == z 
True
>>> x is y
True 
>>> x is z
False
```

[cp1]: /assets/compare1.png
[cp2]: /assets/compare2.png
 