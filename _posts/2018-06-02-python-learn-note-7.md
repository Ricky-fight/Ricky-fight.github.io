---
layout: post
title: python学习笔记（七）
date: 2018-06-02 
comments: true
tags: python  
---
## 6 抽象
### 6.3 为函数编写文档
```python
def square(x):
    'Caculates the square of the number x.'
    return x * x
```
这样就可以用help查看该函数的解释了
### 6.4 函数的参数
python 中函数参数的使用基本和我们熟悉的c语言相同，参数变量传到函数中时，修改参数只生效于这个函数的局部作用域：
```python
>>> def try_ to_ change( n):
...            n = 'Mr. Gumby'
... 
>>> name = 'Mrs. Entity' 
>>> try_ to_ change( name)
>>> name 
'Mrs. Entity'
```
可以看到，在函数内部对参数赋值对外部没有任何影响。
如果一定要修改外部参数， 

1. 可return返回修改后的参数。
2. 可先将参数装在列表里，再传进去。

但**列表**呢？
```python
>>> def change(n):
... n[0] = 'Mr. Gumby'
... 
>>> names = ['Mrs. Entity', 'Mrs. Thing'] 
>>> change(names)
>>> names
['Mr. Gumby', 'Mrs. Thing']
```
可以发现，在*函数内部*对列表进行修改居然对外部产生了同样的影响！这是为什么？
答案是python其实是将列表的内存地址传给了函数，也就是说，函数获得的列表和外部传进的列表是同一个列表！
这告诉我们，当参数为**可变的数据结构**，内部的修改会对外部产生影响！
但这个其实是个feature，使用python进行数据处理时通常需要对数据进行就地处理，有了这个特性，开发者就能非常开心地编写他们想要的函数了！
如果不想发生这个现象，可以给函数传进一个列表的副本，比如`change(vector[:])`。
### 6.4 关键字参数
这个不用讲了，在传入参数时写明参数关键字是非常有必要的。
```python
>>> store( patient=' Mr. Brainsample', hour= 10, minute= 20, day= 13, month= 5)
```
#### 6.4.4 收集参数
还记得*rest吗？
在第五章我们可以用`*`来收集剩下的任意个列表元素，在这里我们也可以用星号来收集任意个参数：
```python
def print_ params(title, *params): 
      print(title)
      print(params)
```
当你需要把星号放在除了最后以外的位置时，我们需要**使用名称**来指定后续参数：
```python
>>> def in_ the_ middle( x, *y, z):
...           print( x, y, z) 
... 
>>> in_ the_ middle( 1, 2, 3, 4, 5, z=7) 
1 (2, 3, 4, 5) 7 
>>> in_ the_ middle( 1, 2, 3, 4, 5, 7) 
Traceback (most recent call last): File "<stdin>", line 1, in < module> TypeError: in_ the_ middle() missing 1 required keyword- only argument: 'z'
```

#### 6.4.5 分配参数
`*`号还可以用来分配参数，我们可以用`*`将元组或列表的元素分配到函数的参数上。
```python
def add( x, y): 
    return x + y
params = (1, 2)
add(*params)
```
此代码块的输出结果：3

