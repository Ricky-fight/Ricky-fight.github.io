---
layout: post
title: macOS + Sublime Text + Latex 环境配置
date: 2018-10-25
comments: true
tags: latex
---

### 转载自
> https://www.jianshu.com/p/50a813c8a6ea
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
