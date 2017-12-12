---
layout: post
title: Http 协议
date: '2017-12-08 19:04'
toc: true
tags:
top: 9997
---

# Http 历史版本
HTTP 是基于 TCP/IP 协议的应用层协议。主要规定了客户端和服务器之间的通信格式，默认使用80端口。

## Http0.9
1991年发布，只有 GET，只能回应 html 格式的字符串。

请求示例：
```
GET /index.html
```

响应示例：
```
<html>
  <body>Hello World</body>
</html>
```

## Http1.0
1996年5月发布，记载于 [RFC1945](https://www.ietf.org/rfc/rfc1945.txt)；可以传输任何类型的内容（文本、图像、视频、二进制文件等）

相对比`http0.9`增加了：
1. POST、HEAD 等请求方法
1. 状态码
2. MIME
3. 首部字段
4. 权限认证（Basic）
5. 字符集
6. 传输各类型内容

详细内容请参考 [RFC1945](https://www.ietf.org/rfc/rfc1945.txt)

请求示例：
```
GET / HTTP/1.0
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)
Accept: */*
```

响应示例：
```
HTTP/1.0 200 OK
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 05 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 5 August 1996 15:55:28 GMT
Server: Apache 0.84

<html>
  <body>Hello World</body>
</html>
```

**缺点:**
每个TCP连接只能发送一个请求。

# Http1.1
1997年1月发布，最初版是 [RFC2068](https://www.ietf.org/rfc/rfc2068.txt)；1999年6月发布的修订版 [RFC2616](https://www.ietf.org/rfc/rfc2616.txt) 沿用至今。

对比`http1.0`最大的变化是：
1. 默认持久连接（`keep-alive`）
1. 引入了 `pipelining`（同一个TCP连接发送多个请求），但是由于服务器是按照请求顺序进行响应，客户端无法确定最优请求顺序，所以未普及
2. 分块传输编码(`chunked`)，针对大数据传输操作，把数据切分为若干数据块[^1]进行传输（流模式）
3. 增加了更多的请求方法：OPTIONS,PUT,DELETE,TRACE,CONNECT
4. 增加了更多的状态码
5. 可以范围取数据,可实现断点续传下载(`range`,`etag`)
6. 可追踪数据请求链(`via`)
7. 增加权限认证
8. 增加了`host`首部，使得一台服务器可以支持虚拟主机

[^1]: 每个非空的数据块之前，会有一个16进制的数值，表示这个块的长度。最后是一个大小为0的块，就表示本次回应的数据发送完了。

**缺点：**
1. 浏览器对同一域名下的持久连接并发有限制，通常是6个
2. 使用`pipelining`会造成 `Head of line blocking`
3. 每次通信都需要传送`header`，多次请求时大部分请求首部字段不变，会增加传输成本

## URL / URI / URN
**定义**
uniform resource identifier（统一资源标识符） URI
uniform resource locator （统一资源定位符）URL
uniform resource name（统一资源名称）URN

**区别**
URL、URN 是 URI 的子集
> URI可被视为定位符（URL），名称（URN）或两者兼备。统一资源名（URN）如同一个人的名称，而统一资源定位符（URL）代表一个人的住址。换言之，**URN定义某事物的身份，而URL提供查找该事物的方法。**

![URI](/images/2017/11/11/uri.png)

## Http 报文

![报文结构](/images/2017/12/05/报文结构.png)

### 通用首部字段
![通用首部字段](/images/2017/12/05/通用首部字段.png)

### 请求首部字段
![请求首部字段](/images/2017/12/05/请求首部字段.png)

### 响应首部字段
![响应首部字段1](/images/2017/12/05/响应首部字段1.png)
![响应首部字段2](/images/2017/12/05/响应首部字段2.png)

### 实体首部字段
![实体首部字段](/images/2017/12/05/实体首部字段.png)

## Http 状态码
![状态码类别](/images/2017/12/05/状态码类别.png)
### 1XX

| 状态码 | 含义                |
| ------ | ------------------- |
| 100    | Continue            |
| 101    | Switching Protocols |

### 2XX
| 状态码 | 含义                          | 说明               |
| ------ | ----------------------------- | ------------------ |
| 200    | OK                            |                    |
| 201    | Created                       |                    |
| 202    | Accepted                      |                    |
| 203    | Non-Authoritative Information |                    |
| 204    | No Content                    | 响应报文中不含主体 |
| 205    | Reset Content                 |                    |
| 206    | Parial Content                | 范围请求；响应报文中含有content-range                   |


### 3XX
| 状态码 | 含义               | 说明                                               |
| ------ | ------------------ | -------------------------------------------------- |
| 300    | Multiple Choices   |                                                    |
| 301    | Moved Permanently  | 永久重定向，丢失搜索引擎权重                       |
| 302    | Found              | 临时重定向                                         |
| 303    | See other          | 有另外的URL，应使用GET去访问另一个URL              |
| 304    | Not Modified       | 没有满足客户端的条件请求的响应需要返回，不包含主体 |
| 305    | Use Proxy          |                                                    |
| 306    | (Unuesed)          |                                                    |
| 307    | Temporary Redirect | 临时重定向，不会把POST改为GET                      |

### 4XX
| 状态码 | 含义                            | 说明                 |
| ------ | ------------------------------- | -------------------- |
| 400    | Bad Request                     | 请求报文存在语法错误 |
| 401    | Unauthorized                    | HTTP认证未通过       |
| 402    | Payment Required                |                      |
| 403    | Forbidden                       | 请求被拒绝           |
| 404    | Not Found                       | 未找到请求资源       |
| 405    | Method NOt Allowed              |                      |
| 406    | Not Acceptable                  |                      |
| 407    | Proxy Authentication Required   |                      |
| 408    | Request Timeout                 |                      |
| 409    | Conflict                        |                      |
| 410    | Gone                            | 请求资源被永久性删除    |
| 411    | Length Required                 |                      |
| 412    | Precondition Failed             |                      |
| 413    | Request Entity Too Large        |                      |
| 414    | Request-URI Too Long            |                      |
| 415    | Unsupported Media Type          |                      |
| 416    | Requested Range Not Satisfiable |                      |
| 417    | Expectation Failed              |        -              |

### 5XX
| 状态码 | 含义                         | 说明                           |
| ------ | ---------------------------- | ------------------------------ |
| 500    | Internal Server Error        | 服务端在执行请求时发生错误     |
| 501    | Not Implemented              |                                |
| 502    | Bad Gateway                  |                                |
| 503    | Service Unavailable          | 服务器超负载，暂时无法处理请求 |
| 504    | Gateway Timeout              |                                |
| 505    | Http Version Not Unsupported |           -                     |

## HTTP 追加协议
### SPDY 协议
Google在2010年发布了 [SPDY协议](http://www.chromium.org/spdy)；希望在协议层面解决http的一些痛点：
1. 一条连接发送一条请求
2. 请求只能从客户端发起
3. 首部未经压缩

![SPDY的设计](/images/2017/12/05/spdy的设计.png)

**SPDY协议的主要功能有：**
1. 多路复用流（单一TCP连接）
2. 请求优先级
3. 压缩Http首部
4. 支持服务端推送数据到客户端
5. 服务器主动提示客户端所需资源（过期）

### WebSocket 协议
Websocket即web浏览器与web服务器之间全双工通信标准。2011年12月11日记载于 [RFC6455](https://www.ietf.org/rfc/rfc6455.txt)；

为了实现Websocket通信，在HTTP连接建立之后，需要完成一次握手：

![Websocket通信](/images/2017/12/05/websocket通信.png)

**Websocket的功能：**
1. 服务器推送
2. 减少通信量（websocket首部信息很小）
3. js可调用 [Websocket API](http://www.w3.org/TR/websockets)

# HTTPS
2000年5月公布，发布于 [RFC2818](https://www.ietf.org/rfc/rfc2818.txt)；默认端口443。因为有加密解密，所以访问会比http慢（可接受），并且会消耗服务器cpu资源。

## Http的缺点
1. 通信使用明文，可能被窃听
2. 不验证通信方身份，可能被伪装
3. 无法证明报文的完整性，可能被篡改

## SSL/TLS 协议
最初是网景设计了 SSL（1.0~3.0）；后来 ITEF 在 SSL3.0 基础上设计了 TLS1.0，目前 TLS 最新版为1.3。

**5次握手：**

第一步，客户端给出协议版本号、一个客户端生成的随机数（Client random），以及客户端支持的加密方法。
第二步，服务端确认双方使用的加密方法，并给出数字证书、以及一个服务器生成的随机数（Server random）。
第三步，客户端确认数字证书有效，然后生成一个新的随机数（Premaster secret），并使用数字证书中的公钥，加密这个随机数，发给服务端。
第四步，服务端使用自己的私钥，获取客户端发来的随机数（即Premaster secret）。
第五步，客户端和服务端根据约定的加密方法，使用前面的三个随机数，生成"对话密钥"（session key），用来加密接下来的整个对话过程。

整体握手是明文传输。

**会话恢复：**

1. session ID
  每一次对话都有一个编号（session ID）。如果对话中断，下次重连的时候，只要客户端给出这个编号，且服务器有这个编号的记录，双方就可以重新使用已有的"对话密钥"，而不必重新生成一把。

2. session ticket
  客户端发送一个服务器在上一次对话中发送过来的session ticket。这个session ticket是加密的，只有服务器才能解密，其中包括本次对话的主要信息，比如对话密钥和加密方法。当服务器收到session ticket以后，解密后就不必重新生成对话密钥了。

## Https的组成
> Http + 加密 + 认证 + 完整性保护 = Https

Http直接和TCP通信，Https先和SSL/TLS通信，再由SSL/TLS和TCP通信。
![HttpVsHttps](/images/2017/12/05/httpvshttps.png)

### 加密
采用「对称加密」[^2]和「非对称加密」[^3]混合加密方式；交换密钥使用「非对称加密」，通信使用「对称加密」。

[^2]: 加密和解密使用同一个密钥；消耗资源少且快
[^3]: 使用公钥进行加密（发送方），使用私钥进行解密（使用方）；消耗资源多且慢

### 认证
采用数字认证机构与其相关机关颁发的「公开密钥证书」进行认证。

1. 网站提交公钥给数字认证机构，机构制作为「公开密钥证书」（非对称加密）
2. 网站服务端把「公开密钥证书」发送给客户端，客户端利用浏览器内置的数字认证机构公钥进行验证（非对称加密）
3. 验证通过后，客户端和服务端使用公钥进行加密通信（对称加密）

### 完整性保护
在通信过程中，应用层会发送MAC（Message Authentication Code）报文摘要（MD5等算法）；MAC可以查知报文是否被篡改。

# HTTP2

2015年5月，发布于 [RFC7540](https://www.ietf.org/rfc/rfc7540.txt)；基于`spdy3.0草案`；Http2 可以基于「明文」，也可以基于「TLS」进行通信。

关于后续版本：
HTTP2后取消了小版本，后续的版本会是HTTP3。

## HTTP2的特性
**1. 二进制协议**
![http2](/images/2017/12/08/http2.png)
- length 定义了整个frame（帧）的大小
- type定义frame的类型（一共10种）
- flags用bit位定义一些重要的参数
- stream id用作流控制
- frame payload就是request的正文。

**2. 多路复用的流**

**帧**：HTTP2 数据通信的最小单位；例如请求和响应等，消息由一个或多个帧组成。

**流**：存在于连接中的一个虚拟通道。流可以承载双向消息，每个流都有一个唯一的整数ID。

HTTP2 中，同域名下所有通信都在单个连接上完成，该连接可以承载任意数量的双向数据流（并发）。流的多路复用意味着在同一连接中来自各个流的数据包会被混合在一起，Stream Identifier将连接上传输的每个帧都关联到一个“流”。

**3. 流的优先级和依赖性**
优先级：每个流都包含一个优先级（也就是“权重”），它被用来告诉对端哪个流更重要。
依赖性：借助于PRIORITY帧，客户端可以告知服务器当前的流依赖于其他哪个流。

**4. header压缩**
HTTP2 对消息头采用 [HPACK](https://www.ietf.org/rfc/rfc7541.txt)（专为http/2头部设计的压缩格式）进行压缩传输，能够节省消息头占用的网络的流量。

HTTP2 对这些首部采取了压缩策略：

1. HTTP2 在客户端和服务器端使用“首部表”来跟踪和存储之前发送的键－值对，对于相同的数据，不再通过每次请求和响应发送；
1. 首部表在HTTP2 的连接存续期内始终存在，由客户端和服务器共同渐进地更新;
1. 每个新的首部键－值对要么被追加到当前表的末尾，要么替换表中之前的值。

**5. 重置消息**
Http2 通过发送RST_STREAM帧可终止当前传输的消息并重新发送一个新的。

**6. 服务器推送**
这个功能通常被称作“缓存推送”。主要的思想是：当一个客户端请求资源X，而服务器知道它很可能也需要资源Z的情况下，服务器可以在客户端发送请求前，主动将资源Z推送给客户端。这个功能帮助客户端将Z放进缓存以备将来之需。

服务器推送需要客户端显式的允许服务器提供该功能。但即使如此，客户端依然能自主选择是否需要中断该推送的流。如果不需要的话，客户端可以通过发送一个RST_STREAM帧来中止。

**7. 流量控制**
类似TCP协议通过sliding window的算法来做流量控制，http2 使用 WINDOW_UPDATE frame 来做流量控制。每个stream都有流量控制，这保证了数据接收方可以只让自己需要的数据被传输。

只有数据帧会受到流量控制。

![Flow Control](/images/2017/12/08/flow-control.png)

# 参考资料
* 《图解 HTTP》
* [Http2协议](https://ye11ow.gitbooks.io/http2-explained/content/part6.html)
* [HTTP2简介和基于HTTP2的Web优化](https://github.com/creeperyang/blog/issues/23)
* [图解SSL/TLS协议](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)

<!--以下是脚注-->
