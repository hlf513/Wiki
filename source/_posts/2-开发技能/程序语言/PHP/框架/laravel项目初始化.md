---
title: "Laravel项目初始化"
date: "2017-12-29 23:10"
tags:
  - Laravel
  - php
toc: true
---

# 配置 PHPSTORM
1. 安装 ide-helper : https://github.com/barryvdh/laravel-ide-helper
2. 安装 phpstorm 的 tools
    1. 进入设置 Preferences | Tools | Command Line Tool Support
    2. 选择 tool bases on Symfony console

# 生产环境监控
Sentry ：https://sentry.io/for/laravel/

# 修改时区
修改 `config/app.php` 中 `'timezone' => 'Asia/Shanghai',`

# Web 应用
## 调试
php-debuger : https://github.com/barryvdh/laravel-debugbar

## RBAC
entrust: https://github.com/Zizaco/entrust

# Api 应用
## 调试

clockwork : https://github.com/itsgoingd/clockwork

## 开启 SQL 日志

写入：`AppServiceProvider->boot()`

``` php
// 监听运行的sql,写入到log中
if ($this->app->environment() !== 'production') {
    \DB::listen(
        function ($sql) {
            foreach ($sql->bindings as $i => $binding) {
                if ($binding instanceof \DateTime) {
                    $sql->bindings[$i] = $binding->format('\'Y-m-d H:i:s\'');
                } else {
                    if (is_string($binding)) {
                        $sql->bindings[$i] = "'$binding'";
                    }
                }
            }
            // Insert bindings into query
            $query = str_replace(array('%', '?'), array('%%', '%s'), $sql->sql);
            $query = vsprintf($query, $sql->bindings);
            // Save the query to Log file
            \Log::debug('SQL', [$query]);
        }
    );
} // end_if
```
## 关闭 Cookie
> api 应用时使用。

注释以下文件中的 cookie 相关：
1. app/Http/Kernel.php
2. config/app.php

## Helper 函数

`app/Common/Helper.php`：

```php
<?php
/**
 * 响应成功
 *
 * @param array $data
 *
 * @return \Illuminate\Http\JsonResponse
 */
function success(array $data)
{
	$json = [
		'statusCode' => \App\Common\Status::Success,
		'message'    => 'ok',
		'data'       => $data,
	];

	return response()->json($json);
}

/**
 * 响应失败
 *
 * @param array $errData
 *
 * @param mixed $detail
 *
 * @return \Illuminate\Http\JsonResponse
 */
function fail(array $errData, $detail = null)
{
	if ( ! isset($errData['errCode']) or ! isset($errData['errMsg'])) {
		fail(\App\Common\Status::SystemError['codeException']);
	}
	$json = [
		'statusCode' => $errData['errCode'],
		'message'    => $errData['errMsg'],
	];
	if (App::environment() != 'production') {
		$json['detail'] = $detail;
	}

	return response()->json($json);
}
```

## 错误码
`app/Common/Status.php`：
```php
<?php

namespace App\Common;

class Status
{
	const Success = 0;

	// 100 + 001
	const SystemError = [
		'codeException' => [
			'errCode' => 100001,
			'errMsg'  => 'The format of errorCode was error'
		]
	];

	// 101 + 001
	const ClientError = [
		'paramException' => [
			'errCode' => 101001,
			'errMsg'  => 'The given data was invalid'
		],
	];

	// 102 + 001
	const Access = [
		'fail'        => [
			'errCode' => 102001,
			'errMsg'  => 'No access'
		],
		'noLogin'     => [
			'errCode' => 102002,
			'errMsg'  => 'Please login first'
		],
		'unknownUser' => [
			'errCode' => 102003,
			'errMsg'  => 'The user is not exist'
		],
	];
}
```

## 异常处理

注册两个异常类：
* NormalException # 错误信息封装为json
* EmailException  # 发送邮件

`NormalException`:
`php artisan make:exception NormalException`
```php
<?php

namespace App\Exceptions;

use App\Common\Status;
use Exception;
use Throwable;

class NormalException extends Exception
{
	protected $exception;

	/**
	 * ResponseException constructor.
	 *
	 * @param array           $message
	 * @param int             $code
	 * @param \Throwable|null $previous
	 */
	public function __construct(array $message, int $code = 0, \Throwable $previous = null)
	{
		if ( ! isset($message['errMsg']) or ! isset($message['errCode'])) {
			return fail(Status::SystemError['codeException']);
		}
		$this->exception = $message;

		parent::__construct($this->exception['errMsg'], $this->exception['errCode'], $previous);
	}

	/**
	 * 自定义异常响应
	 *
	 * @param $request
	 *
	 * @return \Illuminate\Http\JsonResponse
	 */
	public function render($request)
	{
		return fail($this->exception);
	}
}
```

`EmailException`:
`php artisan make:exception EmailException`
```php
<?php

namespace App\Exceptions;

use App\Common\Status;
use Exception;
use Illuminate\Support\Facades\Cache;
use Illuminate\Support\Facades\Mail;
use Illuminate\Support\Facades\Redis;
use Throwable;

class EmailException extends Exception
{
	protected $exception;

	/**
	 * ResponseException constructor.
	 *
	 * @param array           $message
	 * @param int             $code
	 * @param \Throwable|null $previous
	 */
	public function __construct(array $message, int $code = 0, \Throwable $previous = null)
	{
		if ( ! isset($message['errMsg']) or ! isset($message['errCode'])) {
			return fail(Status::SystemError['codeException']);
		}
		$this->exception = $message;

		parent::__construct($this->exception['errMsg'], $this->exception['errCode'], $previous);
	}

	/**
	 * 自定义异常响应
	 *
	 * @param $request
	 *
	 * @return \Illuminate\Http\JsonResponse
	 */
	public function render($request)
	{
		return fail($this->exception);
	}

	public function report()
	{
		// 相同错误码一定间隔内只发送一次
		if (Cache::lock($this->getCode(), env('EXCEPTION_MAIL_INTERVAL', 3600))->get()) {
			$tos = explode(',', env('MAIL_TOS'));
			$message = (new \App\Mail\Exception(
				$this->getCode(),
				$this->getMessage(),
				$this->getTraceAsString()
			))->onConnection('redis')->onQueue('email');
			Mail::to($tos)->queue($message);
		}
	}
}
```

`Http/Exceptions/Handler.php` 中增加：

``` php
// 处理未授权帐号
protected function unauthenticated($request, AuthenticationException $exception)
{
	return fail(Status::Access['unknownUser']);
}

// 处理参数校验失败
protected function convertValidationExceptionToResponse(ValidationException $e, $request)
{
	$detail = $e->validator->getMessageBag()->getMessages();

	return fail(Status::ClientError['paramException'], $detail);
}
```

## 增加邮件模板
`php artisan make:mail Exception --markdown=emails.exception`

修改 `app/Mail/Exception.php`:

```php
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;

class Exception extends Mailable
{
	use Queueable, SerializesModels;

	protected $code;
	protected $message;
	protected $trace;

	/**
	 * Create a new message instance.
	 *
	 * 参数不能是对象，否则不能设置队列发送
	 *
	 * @param $code
	 * @param $message
	 * @param $trace
	 */
	public function __construct($code, $message, $trace)
	{
		$this->code = $code;
		$this->message = $message;
		$this->trace = $trace;
	}

	/**
	 * Build the message.
	 *
	 * @return $this
	 */
	public function build()
	{
		return $this->markdown('emails.exception')
			->with([
				'errCode' => $this->code,
				'errMsg'  => $this->message,
				'trace'   => $this->trace,
			]);
	}
}
```

修改 ` resources/views/emails/exception.blade.php `:

```
@component('mail::message')
# xxxx-报警邮件

错误码：{{ $errCode }}
错误信息：{{ $errMsg }}

------

{{ $trace }}

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```

## 修改.env
```
CACHE_DRIVER=redis
QUEUE_DRIVER=redis

MAIL_FROM_ADDRESS=
MAIL_FROM_NAME=
MAIL_TOS=
EXCEPTION_MAIL_INTERVAL=3600
```

## 自定义权限验证

见：[laravel自定义用户权限校验](http://www.helongfei.com/2017/laravel%E8%87%AA%E5%AE%9A%E4%B9%89%E7%94%A8%E6%88%B7%E6%9D%83%E9%99%90%E6%A0%A1%E9%AA%8C/)
