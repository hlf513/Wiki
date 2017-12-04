---
layout: post
title: Http 协议
date: '2017-11-11 15:55'
toc: true
tags:
top: 9997
---

# Http 简介
HTTP 是基于 TCP/IP 协议的应用层协议。主要规定了客户端和服务器之间的通信格式，默认使用80端口。

## Http 演变历史
### Http0.9
1991年发布，只有 GET，只能回应 html 格式的字符串。

### Http1.0
1996年5月发布，记载于 [RFC1945](https://www.ietf.org/rfc/rfc1945.txt);可以传输任何格式的内容（文本、图像、视频、二进制文件等）；除了 GET，引入了 POST、HEAD；

### Http1.1

## Http 方法

## Http 状态码

| 状态码 | 简义       | 说明                                         |
| ------ | ---------- | -------------------------------------------- |
| 1XX    | 信息       | 101更换协议（websocket）                     |
| 2XX    | 成功       | 200、204不含内容、206范围请求                |
| 3XX    | 重定向     | 301永久、302临时、304资源未更改              |
| 4XX    | 客户端错误 | 400语法错误、401验证、403拒绝、404未找到资源 |
| 5XX    | 服务器错误 | 500程序、502内部、503、504服务不可用         |

## Http 报文

## URL / URI / URN
### 定义
uniform resource identifier（统一资源标识符） URI
uniform resource locator （统一资源定位符）URL
uniform resource name（统一资源名称）URN

### 区别
URL、URN 是 URI 的子集
> URI可被视为定位符（URL），名称（URN）或两者兼备。统一资源名（URN）如同一个人的名称，而统一资源定位符（URL）代表一个人的住址。换言之，**URN定义某事物的身份，而URL提供查找该事物的方法。**

![URI](/images/2017/11/11/uri.png)


# HTTPS

# HTTP2.0

# 常见攻击方式

# 参考资料

* 《图解 HTTP》

<!--以下是脚注-->
