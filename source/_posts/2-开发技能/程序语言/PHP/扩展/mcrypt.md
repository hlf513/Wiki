---
title: "mcrypt"
date: "2018-01-16 14:15"
tag:
  - php
  - openssl
  - mcrypt
---

# 背景

PHP官方文档显示：
> Warning 本扩展从 PHP 7.1.0 开始废弃；自 PHP 7.2.0 起，会移到 PECL。

地址：https://secure.php.net/manual/zh/intro.mcrypt.php

<!-- more -->

# 加解密方法替换公式
使用 `openssl` 替换 `mcrypt`:
```
openssl_encrypt() = base64_encode(mcrypt_encrypt())
openssl_decrypt() = mcrypt_decrypt(base64_decode($encryptString))
```

# 参考资料

* [php7.1微信公众平台消息安全模式的加密及解密](http://blog.csdn.net/sapperlab/article/details/56672443)
