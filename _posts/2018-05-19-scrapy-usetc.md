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
在使用 scrapy 前要分析一下教务处网页的结构，右键任一公告链接，点击审查元素（Mac:Chrome上是检查）![][pagestruct]查看网页结构：
![][pagedev] 发现公告在某个类型为`textAreo clearfix`的`div`标签内的`a`标签，属性都帮我们写好了，一个newsid是唯一识别编号，一个title可以让我方便地填充邮件标题，公告是否是新发布的也非常好判断，只要遍历之前的公告newsid是否有未存入数据库的就可以了。接下来就开始动手做监控脚本。
### 大致步骤
0. 初始化，用爬虫的follow功能先爬取公告栏59页所有的newsid和title存入数据库作为初始数据
1. Scrapy爬取教务处学生公告第一页，获取到title和newsid
2. 存入 mysql 数据库
3. 每2小时Scrapy爬取教务处公告栏页面
4. 逐个遍历数据库中newsid是否已经存在
5. 不存在则存入数据库并发邮件提醒，附上链接（因为很大一部分通知带有附件，我还没研究过如何把附件自动下载下来，就先不自动爬取内容了吧，减少工作量）
6. 定时启动脚本，检查是否有新公告

### 技术细节
天坑啊！要说清楚其实挺麻烦的，慢慢写吧...就把它当成未来技术面试能力的积累...  
功能模块主要有4个：

1. scrapy爬取教务处页面
    依照上述的标签和属性来定位公告的id、date和title，我使用了爬虫解析html元素最常用的xpath语言，得到我想要的所有数据：
    ```python
    id = response.xpath('//div[@class="textAreo clearfix"]/a/@newsid').extract()
    title = response.xpath('//div[@class="textAreo clearfix"]/a/@title').extract()
    date = response.xpath('//div[@class="textAreo clearfix"]/i/text()').extract()
    ```
    
    
2. pymysql+mysql负责存储和查询公告数据
    使用方法在[pymysql][1]官方文档中有示例，如何增删改查直接照搬即可。在此放出部分代码以供以后复用。
    ```python
    import pymysql
           # 连接到MySQL数据库
        connection = pymysql.connect(host='localhost',
                                     user='xingwei',
                                     password='***********',
                                     db='uestcjwc',
                                     charset='utf8mb4',
                                     cursorclass=pymysql.cursors.DictCursor)
            try:
            with connection.cursor() as cursor:
                # 检查公告是否已经在之前获取到过
                for i in range(len(id)):
                    selectsql = "select * from news where newsid = %s"
                    cursor.execute(selectsql, (id[i]))
                    result = cursor.fetchone()
                    if result is None:
                        # print('---------!!New record!!--------')
                        insertsql = "INSERT INTO `news` VALUES (%s, %s, %s)"
                        cursor.execute(insertsql, (id[i], title[i], date[i]))
                        # 必须手动commit你的insert操作
                        connection.commit()
                        #发email到我的邮箱，提醒我有新公告
                        url = 'http://www.jwc.uestc.edu.cn/web/News!view.action?id=%s' % (id[i])
                        mail(url,title[i])
                    else:
                        # print('---------!!Old record!!--------')
                    pass
        finally:
            connection.close()
            # 关闭连接
    ```
3. 用到了github上国人开发的开源邮件项目[zmail][2]，负责发邮件
这个package非常好用，对于只需要email基本功能，且对性能不作要求的人来说足够用了。  
代码精简到极致：
```python
import zmail
def mail(url, title):
    server = zmail.server('18918735979@163.com', 'xw64153869')

    mail = {
                'subject': '教务处有新公告:%s' %(title),
                'content_html': url,
            }
    server.send_mail('18918735979@163.com', mail)
pass
``` 
4. 用crontab在我电脑上设置定时运行脚本
    这个不说了非常简单。在我的博客有提到：
    [Linux命令 - crontab][3]

### 思考与反省
大佬说过，做过的每个项目要反省不足。
胡乱分析了一遍代码，我认为有以下不足：

1. 各个模块可复用性低
    解决办法：封装sendmail方法，insertdb方法。    
2. 电脑关闭了就没法收公告了，考虑以后放云服务器上，不过现在用crontab也够用了，除非我好几天不开电脑2333
3. 虽然是小脚本，但做出整个这个服务的过程还是有点艰辛，碰到了一些坑，有版本问题，也有我从网上角落处搜集的解决方案的移植问题，遇到这些问题需要去解决的时候，还没有形成系统的思路。

### Github
代码发到github上了，隐去了个人信息（邮箱密码，mysql密码）要用的话直接填写即可，使用说明文档有空会填坑。
[https://github.com/Ricky-fight/scrapy_uestcjwc](https://github.com/Ricky-fight/scrapy_uestcjwc)


[1]: https://pypi.org/project/PyMySQL/
[2]: https://github.com/ZYunH/zmail
[3]: https://ricky-fight.github.io/2018/05/linux-cmd/
[pagestruct]: /assets/pagestruct.png
[pagedev]: /assets/pagedev.png