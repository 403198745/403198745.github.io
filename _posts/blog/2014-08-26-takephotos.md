---
layout: post
title: Sqlserver的一些常见知识
categories: Blog
description:Sqlserver的一些常见知识
keywords: 数据库,sqlserver
---

### UUID以及8位数字uuid

```
RIGHT(100000000 + CONVERT(bigint, ABS(CHECKSUM(NEWID()))), 8)  

```
### 2014 年 6 月 武汉

1. 照顾好拍摄对象的心情，她开心和配合了，才容易出好片。
2. 如果她真的不愿意拍，就不拍了吧，否则出来基本也是废的。
3. 蓝天白云绿水真的很容易出好片，但是阴天加浑浊的长江边还是不要轻易挑战了。

### 2014 年 3 月 武汉

1. 气氛要活泼一点，不然模特摆拍容易表情僵硬。
2. 光圈优先在室外光线好的情况下容易过曝。
3. 只挑拍得最好的片发给模特看，如果没有出好片，宁愿不发。
