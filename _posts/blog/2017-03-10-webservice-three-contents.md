---
layout: post
title: webservice三要素
categories: Blog
description: 第一天写blog。
keywords: soap，服务
---


## webservice三要素

###Simple Object Access Protocol  简单对象访问协议

soap用来描述传递信息的格式，WSDL用来描述如何访问具体接口，uddi用来管理，分发，查询webservice,SOAP使用基于XML的数据结构和HTTP的组合定义一个标准的方法来使用Internet上各种不同操作环境中的分布式对象。同时也是定义访问Web Services的协议，也就是哪些是可以访问的，怎样的格式才能够访问，返回的格式又是什么样的，这些都是SOAP定义的。

###WSDL（WebServicesDescriptionLanguage） 网络服务描述语言

WSDL用来描述如何访问具体接口，是描述SOAP协议的具体语言，用WSDL实现SOAP协议，把它写成文件，直接访问。

###UDDI (通用描述、发现及整合)

是把这些web services 收集和存储起来，这样当别人访问这些信息的时候就从UDDI中查找，看有没有这个信息存在。
