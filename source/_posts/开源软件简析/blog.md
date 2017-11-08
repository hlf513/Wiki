---
title: "Blog 系统之 Hexo"
date: "2017-11-05 14:26"
tag:
  - atom
toc: true
---

# Blog 系统
之前用的是 Octopress ，由于不更新了，且使用的是 ruby 开发，生成 html 比较慢，所以迁移到 hexo。

# Hexo

## 安装 hexo

[hexo 文档](https://hexo.io/zh-cn/docs/)

```shell
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

安装完毕后，请自行修改 `/_config.yml`

** 最后别忘记添加 CNAME**

## 主题

采用 Hiker 主题；[[中文说明]](https://github.com/iTimeTraveler/hexo-theme-hiker/blob/master/README.cn.md) [[Github]](https://github.com/iTimeTraveler/hexo-theme-hiker)

**删除作者自己的 cnzz 统计代码**

```js
# source/js/scripts.js 中，删除以下代码
var s = [
        '<div style="display: none;">',
          '<script src="https://s11.cnzz.com/z_stat.php?id=1260716016&web_id=1260716016" language="JavaScript"></script>',
        '</div>'
      ].join('');

  var di = $(s);

  $('#container').append(di);
```

**修改主题的配置文件**

```yaml
# /themes/hiker/_config.yml
# 清空以下信息，都是作者本人信息
social:
donate:
# 其他数据请自行填充
```

# Atom 配置
> 采用 atom 作为 md 的编辑器

详细配置见：[atom-hexo配置](/intro/atom.html#hexo)

# Hexo 命令

> 详细命令清单见：<kbd>[hexo 命令](https://hexo.io/zh-cn/docs/commands.html)</kbd>

## 新增文章

```sh
$ hexo new [layout] <title>
```

## 预览

```sh
$ hexo g
```


## 实时更新预览

```sh
$ hexo g -w
```

## 发布

```sh
$ hexo d -g
```

# 自定义命令

**配置**

bash/zsh 的配置文件 .basrc/.zshrc  增加如下命令

```sh
export BLOG_PATH="blog的目录"
alias blog_root="cd $BLOG_PATH/"
PATH="...:$BLOG_PATH/shell"
```

## 进入 Hexo 根目录
```sh
blog_root
```

## 预览
```sh
blog_preview
```

## 生成 html
```sh
blog_generate
```

## 发布
```
blog_deploy
```

## 打开 atom
```
blog_atom
```
