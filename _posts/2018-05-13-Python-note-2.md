---
layout: post
title: Python学习笔记（二）
date: 2018-05-13 
tags: python   
---

### 编码
实践告诉我们，编码是Python语言中很令人头疼的问题，我们在处理字符串（尤其是应用爬虫等程序）时更应注意。

比如输出非ascii字符集时，要对字符串编码处理：

	>>> ”hello,world!“.encode("utf-8")
	b'hello,world!'
	”hello,world!“.encode("utf-16")
	b'\xff\xfeh\x00e\x00l\x00l\x00o\x00,\x00w\x00o\x00r\x00l\x00d\x00!\x00'
	”hello,world!“.encode("utf-32")
	b'\xff\xfe\x00\x00h\x00\x00\x00e\x00\x00\x00l\x00\x00\x00l\x00\x00\x00o\x00\x00\x00,\x00\x00\x00w\x00\x00\x00o\x00\x00\x00r\x00\x00\x00l\x00\x00\x00d\x00\x00\x00!\x00\x00\x00'
32位和16位编码比较浪费空间，建议使用变长编码utf-8.
如果不想输出非Ascii编码，但是从外部接收到的字符可能含有非ascii编码时，如此写很有可能会报错：

	>>> ”hello,world!“.encode("ascii")
	Traceback (most recent call last):
 	File "<stdin>", line 1, in <module>
 	UnicodeEncodeError: 'ascii' codec can't encode character '\xe6' in position 1: ordinal not in range(128)
正确的处理方式：

	>>>”hello,world!“.encode("ascii"，“replace”)
	b'H?ll?, w?rld!'
几乎在所有情况下，都最好使用UTF-8。事实上它也是默认使用的编码。[1]()
在编写源代码时，必要的一步是在第一行加上：

	# -*- coding: encoding name -*-
请将其中的 encoding name 替换为你要使用的编码(大小写都行)，如utf-8或latin-1。
	

### bytearray
python 提供可变字符串：
	
	>>> x = bytearray(b"Hello!")
	>>> x[1]= ord(b"u")
	>>> x
	bytearray(b'Hullo!')
修改可变字符串需要使用ord获取序数值（ordinal value）。  
—— 书上是这么写的，然而我试过`ord("u")`也可以？

### 列表的主要操作
#### 索引

索引从0开始：

	>>> greet = "Hello"
	>>> greet[0]
	'H'	
索引可以为负：

	>>> greet[-1]
	'o'
不但如此，还可以直接对字符串字面量索引：

	>>> "Hello"[-1]
	'o'
	>>> fourth = input('Year: ')[3]
	Year: 2018
	>>> fourth
	'5'
很方便，是吧？

		
#### 切片
	
顾名思义，切片操作可以访问特定范围内的元素。

	>>> "hello"[0:3]
	'hel'
切片[a:b]返回列表的[a,b）元素。
这个不需要现在强记，因为以后很多操作都遵循左闭右开的规则。
** 很有用的一个技巧是，当你需要返回前N个或后N个：
	
	>>> N=3
	>>> "hello"[:N-1]
	'he'
	>>> "hello"[-N:]
	'llo'

切片操作可以指定步长：

	>>> "Hello,world!"[::2]
	'Hlowrd'
*技巧：步长可以为负：

	>>> "Hello,world!"[::-1]
	'!dlrow,olleH'
	>>> "Hello,world!"[::-2]
	'!lo,le'
利用这个技巧可以倒序输出。
	
#### 相加

	>>> [1,2,3]+[4,5,6]
	[1, 2, 3, 4, 5, 6]
	
#### 相乘
	
	>>> 6*[1,2,3]
	[1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]
	>>> "python"*2
	'pythonpython'
	>>> [None]*10
	[None, None, None, None, None, None, None, None, None, None]
None表示什么都没有
#### 成员资格：in

可以使用`in`检查某元素是否在列表里：

	>>> 'r' in permissions
	True
	>>> 'x' in permissions
	False
	
	>>> users = ['Ricky','Michell']
	>>> input('Enter you user name: ') in users
	Enter you user name: Ricky
	True
	>>> input('Enter you user name: ') in users
	Enter you user name: ricky
	False

第二个例子表明：
1. `in`操作同样可以用于字面量
2. 列表的字符串元素区分大小写（废话）
基于此，我们可以做出一个简易登录系统。

	import time
	greet = "Welcome to login system!"
	database = {"Ricky":"123456","Michaell": "000000"}
	print(greet)
	time.sleep(1.5)
	username = input("Your username: ")
	if username in database:
	    password = input("Your password: ")
	    print("checking...")
	    time.sleep(1.5)
	    if password == database[username]:
	        print("login successful!")
	    else:
	        print("incorrect password!")
	else:
	    print("you are unregistered.")
		
测试效果如下：
	  
  
	xingweideMBP:learn xingwei$ python3 main.py 
	Welcome to login system!
	Your username: Ricky
	Your password: 123456
	checking...
	login successful!
	xingweideMBP:learn xingwei$ python3 main.py 
	Welcome to login system!
	Your username: Ricky
	Your password: 888
	checking...
	incorrect password!
	xingweideMBP:learn xingwei$ python3 main.py 
	Welcome to login system!
	Your username: hacke
		you are unregistered.
			
[1] Magnus Lie Hetland. Python基础教程（第3版）（图灵图书） (Kindle位置952). 人民邮电出版社. Kindle 版本. 
