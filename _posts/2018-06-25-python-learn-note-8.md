---
layout: post
title: python学习笔记（八）
date: 2018-06-25 
comments: true
tags: python  
---
## 6 抽象
### 6.4 参数魔法
#### 6.4.6 练习使用参数
书上提供了一个综合性的例子以供更好理解参数分配：
```python
def story(** kwds):
    return 'Once upon a time, there was a ' \
        '{job} called {name}. '.format_map(kwds)


def power(x, y, *others):
    if others:
        print(' Received redundant parameters:', others)
    return pow(x, y)


def interval(start, stop=None, step=1):
    'Imitates range() for step > 0'
    if stop is None:  # 如果没有给参数stop指定值,start,stop=0,start
        start, stop = 0, start
    result = []

    i = start
    while i < stop:
        result.append(i)
        i += step
    return result
```
