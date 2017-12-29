---
title: "Lumen项目初始化"
date: "2017-12-29 23:23"
toc: true
tags:
  - Lumen
  - php
---

# 开启日志

在 ` bootstrap/app.php ` 写入：
``` php
$app->configureMonologUsing(function($monolog) {
     $monolog->pushHandler(
          new \Monolog\Handler\RotatingFileHandler(
               storage_path('logs/data-entry.log'),
               env('APP_LOG_MAX_FILES', 30),
               constant('\\Monolog\\Logger::' . strtoupper(env('APP_LOG_LEVEL', 'debug')))
          )
     );
     return $monolog;
});
```

# 调试、异常、错误码
> 见laravel项目初始化
