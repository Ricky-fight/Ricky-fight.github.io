---
layout: post
title: macOS技巧
date: 2018-05-12 
comments: true
tags: macOS  
---
### 如何解除软件来源打开限制
系统是OS Sierra(10.12)以上，需要用终端打开『允许任何来源』。
打开launchpad——其他——终端。在命令行里输入
```bash
sudo spctl --master-disable
```