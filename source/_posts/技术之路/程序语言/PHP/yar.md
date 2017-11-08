---
title: YAR
date: '2017-11-07 22:35'
tag:
  - yar
  - php
  - php_ext
toc: true;
---

# What

一个轻量级可并行的 RPC 框架，支持三种打包协议（msgpack,json,php）,传输协议支持 http（tcp/unix以后会支持）
​
>github 地址：https://github.com/laruence/yar  
>php 官网：http://php.net/manual/zh/book.yar.php


# Why
鸟哥举了两个场景(传统 web 应用)：  

1. 一个进程，一个请求，但是涉及到多个没有依赖性的数据源，只能串行处理（依次等待所有数据源处理）完毕后才能响应；  
  >yar 可以并行处理，减少时间开销

2. 一个应用系统随着业务的增加，人员流失，只做加法，等到不可维护性的时候，只能重构
  >yar 可以给系统进行解耦（实际上是 soa 思想）


# How

## server  
放置在 web 服务器上，通过 http 访问(默认get访问会输出 doc 信息)

## client
1. 串行调用
1. 并行调用（yar 内部使用 libcurl + epoll ）

# 高级进阶

## Yar 协议分析
Yar整个协议由82字节长度的yar_header + 8字节的数据打包协议(MSGPACK、JSON、PHP)  + N字节的Body组成。

## 安全性
>未验证

```
typedef struct _yar_header {
  unsigned int   id;            // transaction id
  unsigned short version;       // protocl version
  unsigned int   magic_num;     // default is: 0x80DFEC60
  unsigned int   reserved;
  unsigned char  provider[32];  // reqeust from who
  unsigned char  token[32];     // request token, used for authentication
  unsigned int   body_len;      // request body len
}
```
>struct `_yar_header`中需要注意的是magic_num值。该值在Client与Server都应该保持一致。否则视为不合法的数据。我们可以通过修改这个值，定制一个yar框架，防止其他人恶意请求。

## 跨语言
>利用 yar 可以接收 struct 实现

- client

```
array(
  "i" => '', // transaction id
  "m" => '',  // the method which being called
  "p" => array(), // parameters
)
```
- server

```
array(
   "i" => '',
   "s" => '', //status
   "r" => '', //return value
   "o" => '', //output
   "e" => '', //error or exception
)
```

# 参考资料
1. [Yar – 并行的RPC框架(Concurrent RPC framework)](http://www.laruence.com/2012/09/15/2779.html)
2. [YAR 并行RPC框架研究](http://www.searchtb.com/2013/10/yar-%E5%B9%B6%E8%A1%8Crpc%E6%A1%86%E6%9E%B6%E7%A0%94%E7%A9%B6.html)
3. [Yar协议分析与跨语言实现](http://blog.weixinhost.com/wei-ming-ming/)
