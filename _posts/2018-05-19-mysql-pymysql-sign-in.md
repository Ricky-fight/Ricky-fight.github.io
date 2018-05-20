---
layout: post
title: pymysql.err.OperationalError 1045 Access denied最简单解决办法
date: 2018-05-19
comments: true 
tags: mysql pymysql  
---
> 参考自 [William_Dong@CSDN][william]  


使用的是python3.6+pymysql+mysql8.0

在cmd命令行直接输入mysql回车出现：`ERROR 1045 (28000): Access denied for user 'ODBC'@'localhost' (using password: NO)`

这时在cmd命令行输入`mysql -u root -p `回车输入密码，就可以成功连接数据库

但用pymysql登陆报错，使用 Pymysql 官方给出的范例脚本如下：
在此之前创建一个新的DB：

```mysql
CREATE TABLE `users` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `email` varchar(255) COLLATE utf8_bin NOT NULL,
    `password` varchar(255) COLLATE utf8_bin NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin
AUTO_INCREMENT=1 ;
```
```python
import pymysql

# Connect to the database
connection = pymysql.connect(host='localhost',
                             user='root',
                             password='pass',
                             db='test',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)

try:
    with connection.cursor() as cursor:
        # Create a new record
        sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
        cursor.execute(sql, ('webmaster@python.org', 'very-secret'))

    # connection is not autocommit by default. So you must commit to save
    # your changes.
    connection.commit()

    with connection.cursor() as cursor:
        # Read a single record
        sql = "SELECT `id`, `password` FROM `users` WHERE `email`=%s"
        cursor.execute(sql, ('webmaster@python.org',))
        result = cursor.fetchone()
        print(result)
finally:
    connection.close()
```
预期输出是：

```shell
{'password': 'very-secret', 'id': 1}
```
然而脚本回报了traceback：
```shell
Traceback (most recent call last):
  File "testmysql.py", line 9, in <module>
    cursorclass=pymysql.cursors.DictCursor)
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/__init__.py", line90, in Connect
    return Connection(*args, **kwargs)
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/connections.py", line 704, in __init__
    self.connect()
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/connections.py", line 974, in connect
    self._request_authentication()
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/connections.py", line 1203, in _request_authentication
    auth_packet = self._read_packet()
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/connections.py", line 1059, in _read_packet
    packet.check_error()
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/connections.py", line 384, in check_error
    err.raise_mysql_exception(self._data)
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pymysql/err.py", line 109,in raise_mysql_exception
    raise errorclass(errno, errval)
pymysql.err.OperationalError: (1045, "Access denied for user 'root'@'localhost' (using password: NO)")
```
网上给了各种各样的方法，大多是通过各种方式修改密码。

最简单的方法是更换了root密码的认证方式解决的，新版mysql使用的caching_sha2_password，换成mysql_native_password我就可以连上了。

步骤是在cmd命令行连接mysql
`mysql -u root -p`

然后输入`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'dong1990';`

DONE! 

这时再跑下python脚本就可以连接了。

[william]: https://blog.csdn.net/dongweionly/article/details/80273095