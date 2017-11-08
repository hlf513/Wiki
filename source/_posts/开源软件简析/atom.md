---
title: Atom 编辑器
date: '2017-11-05 05:51'
tag:
  - atom
toc: true
---

# 安装插件
*  markdown-preview-enhanced ：增强版 md 预览
*  markdown-writer ：写博客必备利器
*  tool-bar ：工具栏基类
*  tool-bar-markdown-writer ： md-writer的工具栏
*  vim-mode-plus ：vim 插件
*  markdown-table-editor ：表格编辑工具
*  atom-terminal ：终端

**tool-bar-markdown-writer**

修改文件 `~/.atom/packages/tool-bar-markdown-writer/lib/tool-bar-markdown-writer.coffee` 增加如下功能：

* 增加 publish-draft
* Preview markdown 增加支持 `markdown-preview-enhanced`

新增的配置如下：
```js
{
  'icon': 'content-duplicate'
  'tooltip': 'Publish Draft'
  'callback': 'markdown-writer:publish-draft'
}
{
  'icon': 'markdown'
  'tooltip': 'Preview Markdown'
  'data': ['markdown-preview-enhanced', 'markdown-preview-plus','markdown-preview']
  'visible': (data) ->
    pkg = data.find (pkg) -> !!atom.packages.getLoadedPackage(pkg)
    "#{pkg}:toggle" if pkg
}
```

**Atom 配置文件**
```yaml
"*":
  core:
    packagesWithKeymapsDisabled: [
      "markdown-preview"
    ]
    telemetryConsent: "no"
  "exception-reporting":
    userId: "5a8289c3-5832-4d26-af7b-3235427b72da"
  "markdown-preview-enhanced":
    enableExtendedTableSyntax: true
    imageDropAction: "copy to image folder"
    mathRenderingOption: "MathJax"
  "markdown-writer":
    fileExtension: ".md"
    renameImageOnCopy: true
  "tool-bar": {}
  "tool-bar-markdown-writer": {}
  "vim-mode-plus":
    notifiedCoffeeScriptNoLongerSupportedToExtendVMP: true
  welcome:
    showOnStartup: false

```

# Simiki 配置
> 主要使用的是 md-writer

生成项目专用的配置文件：

```js
点击 packages -> markdown-writer -> configurations -> create project configs
```
最终在项目的根目录下生成 `_mdwriter.cson`

修改如下配置：

```yaml
# Directory to drafts from siteLocalDir
siteDraftsDir: "content/"
# Directory to posts from siteLocalDir
sitePostsDir: "content/"
# Directory to images from siteLocalDir
# - E.g. to use the current filename directory, can use {directory}
siteImagesDir: "attach/img/{year}/{month}/"

# Filename format of new drafts created
newDraftFileName: "{slug}{extension}"
# Filename format of new posts created
newPostFileName: "{slug}{extension}"

# Front matter date format, determines the {date} in frontMatter
frontMatterDate: "{year}-{month}-{day} {hour}:{minute}"
# Front matter template
frontMatter: """
---
title: "{title}"
date: "{date}"
layout: "page"
---

[TOC]
"""

# File extension of posts/drafts
fileExtension: ".md"
# File slug separator
slugSeparator: "-"

tableExtraPipes: true
```

# Hexo 配置

> 主要使用的是 md-writer

生成项目专用的配置文件：

```
点击 packages -> markdown-writer -> configurations -> create project configs
```
最终在项目的根目录下生成 `_mdwriter.cson`

修改如下配置：

```yaml
siteEngine: "hexo"
# Website URL of your blog
siteUrl: "http://www.helongfei.com"

# Directory to drafts from siteLocalDir
siteDraftsDir: "source/_drafts/"
# Directory to posts from siteLocalDir
sitePostsDir: "source/_posts/{year}/"
# Directory to images from siteLocalDir
# - E.g. to use the current filename directory, can use {directory}
siteImagesDir: "source/images/{year}/{month}/"


# Filename format of new drafts created
newDraftFileName: "{slug}{extension}"
# Filename format of new posts created
newPostFileName: "{slug}{extension}"

# Front matter date format, determines the {date} in frontMatter
frontMatterDate: "{year}-{month}-{day} {hour}:{minute}"
# Front matter template
frontMatter: """
---
layout: "{layout}"
title: "{title}"
date: "{date}"
---
"""

# File extension of posts/drafts
fileExtension: ".md"
# File slug separator
slugSeparator: "-"

# Table row continuation
# - Enable to auto insert table columns when you press enter in a table row
tableNewLineContinuation: true
```
