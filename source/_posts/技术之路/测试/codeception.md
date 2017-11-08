---
title: Codeception 之验收测试
date: '2016-05-02 15:38:09'
tag:
  - php
  - Codeception
toc: true
---


# Acceptance Testing
验收测试针对整个站点进行测试，模拟真实用户的访问流程。

写测试的人员不需要知道网站的内部实现。

# 两种测试类型
## PhpBrowser

配置`tests/acceptance.suite.yml`

```
class_name: AcceptanceTester
modules:
    enabled:
        - PhpBrowser:
            url:
        - \Helper\Acceptance
```

## Selenium WebDriver

配置`tests/acceptance.suite.yml`  

```
class_name: AcceptanceTester
modules:
    enabled:
        - WebDriver:
            url:
            browser: chrome
        - \Helper\Acceptance
```

配置`Selenium`环境

1. 下载[Selenium Standalone Server](http://docs.seleniumhq.org/download/)
1. 下载[Google Chrome Driver](https://sites.google.com/a/chromium.org/chromedriver/)
1. 启动：

```
//启动selenium-server
java -jar selenium-server-standalone-2.53.0.jar
//启动Chrome Driver
./chromedriver
```

# 编写测试
>测试写在`tests/acceptance`,后缀为`Cept`，例如：`SiginCept.php`

## PhpBrowser测试

文档参考：PHP Browser

- actions
- Assertions  
- Grabbers
- Comments
- Cookies, Urls, Title

## Selenium WebDriver测试

文档参考：Selenium WebDriver

- Session Snapshots

## 其他
### DB配置

1. SQL语句  
  放在 `/tests/_data` 下
2. DB-config  
  修改`codeception.yml`

### Debug
1. 命令行增加 `--debug`
2. 手动输出 `codecept_debug()`
3. 手动下一步 [pauseExecution](http://codeception.com/docs/modules/WebDriver#pauseExecution)
4. 记录测试 [Recorder extension](http://codeception.com/addons#CodeceptionExtensionRecorder)


# 参考资料
1. [Acceptance Testing介绍](http://codeception.com/docs/03-AcceptanceTests)
2. [PHP Browser介绍](http://codeception.com/docs/03-AcceptanceTests#PHP-Browser)
3. [PHP Browser文档](http://codeception.com/docs/modules/PhpBrowser)
4. [Selenium WebDriver文档](http://codeception.com/docs/modules/WebDriver)
