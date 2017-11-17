---
title: Web Servive
date: '2015-12-25 00:05'
tag:
  - web-service
  - soa
  - rpc
toc: true
---

# What
Web服务是一种服务导向架构的技术，通过标准的Web协议提供服务，目的是保证不同平台的应用服务可以互操作。  

# Why
以下观点来自维基百科：  

1. 重复使用的应用程序组件
2. 连接现有的软件，在不同的应用程序和平台之间交换数据

# How
有三种普遍实现方式：  

## 远程过程调用（RPC）
>面向动作；目前有很多开源的 RPC 框架：YAR、GRPC、Thrift 等

RPC式WEB服务实质上是利用一个简单的映射，以把用户请求直接转化成为一个特定语言编写的函数或方法.  

## 服务导向架构（SOA[P]）
>面向消息；

遵从服务导向架构（Service-oriented architecture，SOA）概念来构筑WEB服务，在服务导向架构中，通讯由消息驱动，而不再是某个动作（方法调用）。 作为与RPC方式的最大差别，SOA方式更加关注如何去连接服务而不是去特定某个实现的细节。  

通常使用 SOAP 风格；由 UDDI、 SOAP 和 WSDL 构成；他们之间的关系如下图：  
      ![web-service](/images/2017/11/web-service.png)

- **SOAP**  
  一个基于XML的可扩展消息信封格式，需同时绑定一个网络传输协议。这个协议通常是HTTP或HTTPS，但也可能是SMTP或XMPP。  
- **WSDL**  
  一个XML格式文档，用以描述服务端口访问方式和使用协议的细节。通常用来辅助生成服务器和客户端代码及配置信息。    
- **UDDI**  
  一个用来发布和搜索WEB服务的协议，应用程序可借由此协议在设计或运行时找到目标WEB服务。  

## 表述性状态转移（REST）
>面向资源；

表述性状态转移式（Representational state transfer，REST），是一种架构风格，把接口限定在一组广为人知的标准动作中（比如HTTP的GET、POST、PUT、DELETE）以供调用。  

**REST 的 基本特征：**

1. 客户端和服务器结构
2. 连接协议具有无状态性
3. 能够利用Cache机制增进性能
4. 层次化的系统

**REST 的三要素：**

1. 唯一的资源标识
2. 简单的方法
3. 一定的表达方式  

三要素关系图：    
![REST](/images/2017/11/rest.png)

REST 是以 **资源** 为中心, **名词** 即资源的地址, **动词** 即施加于名词上的一些有限操作, **表达** 是对各种资源形态的抽象.  
以HTTP为例, 名词即为URI(统一资源标识), 动词包括POST, GET, PUT, DELETE等(还有其它不常用的2个,所以 整个动词集合是有限的), 资源的形态(如text, html, image, pdf等)


# 参考资料
1. [Web服务-维基百科](https://zh.wikipedia.org/zh-cn/Web服务)
2. [Web services-维基百科](https://zh.wikipedia.org/wiki/Web_services#.E4.BB.80.E4.B9.88.E6.98.AF_SOAP)
3. [面向服务的体系结构](https://zh.wikipedia.org/wiki/面向服务的架构)
4. [REST](https://zh.wikipedia.org/wiki/REST)
5. [远程过程调用](https://zh.wikipedia.org/wiki/遠程過程調用)
6. [Webservice学习笔记五，Web Service实践之REST vs RPC](http://blog.csdn.net/x_yp/article/details/6231766)
7. [Webservice学习笔记六，SOAP, REST and XML-RPC报文格式收集](http://blog.csdn.net/x_yp/article/details/6231918)
8. [rest-vs-soap](http://www.slideshare.net/ozten/rest-vs-soap-yawn)
9. [Restful-User-Experience](http://www.slideshare.net/trilancer/restful-user-experience-1421793)
