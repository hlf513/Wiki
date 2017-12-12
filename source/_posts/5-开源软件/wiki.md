---
title: Wiki 系统之 Simiki
date: 2017-11-04
tag:
  - atom
toc: true
---

# Wiki 系统

需求：git+md

| 系统                                                  | 实现方式  | 缺点                                            | 优点                   |
|:----------------------------------------------------- |:--------- |:----------------------------------------------- |:---------------------- |
| [dokuwiki](https://www.dokuwiki.org/dokuwiki#)        | php+文本  | 编辑器不好用；数据量大搜索可能有性能问题（txt） | 插件丰富，txt移植方便  |
| [mediawiki](https://www.mediawiki.org/wiki/MediaWiki) | php+mysql | 配置复杂；visualeditor 加载慢                   | 功能全面，visualeditor |
| [simiki](http://simiki.org/)                          | git+md    | 全量生成html；图片支持不好                      | 可定制                 |
| [gitbook](https://www.gitbook.com/)                   | git+md    | 全量生成 html ( 渲染巨慢 )                       | 有编辑器               |

# Simiki 安装

[simiki 文档](http://simiki.org/zh-docs/)

```sh
pip install simiki
mkdir mywiki && cd mywiki
simiki init
```

配置 `_config.yml`

```yaml
url:
title:
keywords:
description:
author:
theme: yasimple_x2

markdown:
  - fenced_code
  - extra
  - codehilite(css_class=hlcode, linenums=False)
  - toc(title=Table of Contents)
```

增加 CNAME 文件

> 为了域名解析；文本内容为待解析域名


# Atom 配置
> 使用 Atom 作为 md 编辑器

详细配置见 [Atom-simike 配置](/intro/atom.html#simiki)


# 发布

##  fab

需要`_config.yml`增加如下配置项：

```yaml
deploy:
  - type: git
    remote: origin
    branch: gh-pages
```

发布的时候如下命令：

```
fab deploy	#不支持python3
```

##  shell

使用自定义命令：

```
wiki_deploy
```

# simiki 命令

## 生成 html

```
simiki g
```

## 预览 wiki

```
simiki p
```


## 实时生成 html

```
simiki p -w
```
