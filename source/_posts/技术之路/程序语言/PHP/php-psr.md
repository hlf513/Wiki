---
title: PSR
date: '2016-04-03 00:45'
tag:
  - php
  - PSR
toc: true
---

# PSR
>PSR 来自 PHP FIG(框架协同工作组)

现有规范

- PSR-0/PSR-4 自动加载（2014.10.21起弃用，请使用 PSR-4）
- PSR-1 基本规范
- PSR-2 代码风格
- PSR-3 日志接口


## PSR-0/PSR-4

>自动加载  
>class 指 class,interface,trait和其他相似的结构


### PSR-0(弃用)

按照目录拼接类名

```
/path/to/src/
    VendorFoo/
        Bar/
            Baz.php     # VendorFoo_Bar_Baz
```

### PSR-4

1. 类名中下划线转为目录
2. 每个命名空间必须有顶级命名空间（vender）

```
Vendor_Name
  |- Name_space
    |- Class
    	|-Name.php   # \Vendor_Name\Name_space\Class_Name
```


### 自动加载方法

* 函数

```
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

* 类: [SplClassLoader](http://gist.github.com/221634)


## PSR-1
>基本代码规范  

- 「文件」源文件`必须`只使用 `<?php` 和 `<?=` 这两种标签。
- 「文件」源文件中php代码的编码格式`必须`只使用不带`字节顺序标记(BOM)`的`UTF-8`。
- 「类」`类名(class name)` `必须`使用`骆驼式(StudlyCaps)`写法。
- 「类」`类(class)`中的常量`必须`只由大写字母和`下划线(_)`组成。
- 「类」`方法名(method name)` `必须`使用`驼峰式(cameCase)`。
- 「文件」一个源文件`建议`只用来做声明（`类(class)`，`函数(function)`，`常量(constant)`等）或者只用来做一些引起副作用的操作（例如：输出信息，修改`.ini`配置等）,但`不建议`同时做这两件事。

## PSR-2
>代码风格指南  
>针对 PSR-1 的继承和扩展

- 「缩进」代码`必须`使用4个空格来进行缩进，而不是用制表符。
- 「行」一行代码的长度`不建议`有硬限制；软限制`必须`为120个字符，`建议`每行代码80个字符或者更少；非空行`不可`有空格；一行`不可`多于一个语句.
- 「文件」纯 php 文件 ```?>``` `必须` 省略;`必须`以空行结束
- 「命名空间」在`命名空间(namespace)`的声明下面`必须`有一行空行，并且在`导入(use)`的声明下面也`必须`有一行空行。
- 「类」`类(class)`的左花括号`必须`放到其声明下面自成一行，右花括号则`必须`放到类主体下面自成一行。
- 「方法」`方法(method)`的左花括号`必须`放到其声明下面自成一行，右花括号则`必须`放到方法主体的下一行。
- 「属性|方法」所有的`属性(property)`和`方法(method)` `必须`有可见性声明；`抽象(abstract)`和`终结(final)`声明`必须`在可见性声明之前；而`静态(static)`声明`必须`在可见性声明之后。
- 「关键字和常量」关键字和常量(true，false，null)`必须`小写
- 「控制结构」在控制结构关键字的后面`必须`有一个空格；而`方法(method)`和`函数(function)`的关键字的后面`不可`有空格。
- 「控制结构」控制结构的左花括号`必须`跟其放在同一行，右花括号`必须`放在该控制结构代码主体的下一行。
- 「控制结构」控制结构的左括号之后`不可`有空格，右括号之前也`不可`有空格。
- 「类」`不建议`使用`_`来表明方法和属性的保护性


```
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements
	FooInterface,
	Bar
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
       $foo->bar(
		    $longArgument,
		    $longerArgument,
		    $muchLongerArgument
		);

		$longArgs_shortVars = function (
		    $longArgument,
		    $longerArgument,
		    $muchLongerArgument = null
		) use ($var1) {
		   // body
		};
    }

     public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
		switch ($expr) {
		    case 0:
		        echo 'First case, with a break';
		        break;
		    case 1:
		        echo 'Second case, which falls through';
		        // no break
		    case 2:
		    case 3:
		    case 4:
		        echo 'Third case, return instead of break';
		        return;
		    default:
		        echo 'Default case';
		        break;
		}

		while ($expr) {
		    // structure body
		}

		do {
		    // structure body;
		} while ($expr);

		for ($i = 0; $i < 10; $i++) {
		    // for body
		}

		foreach ($iterable as $key => $value) {
		    // foreach body
		}

		try {
		    // try body
		} catch (FirstExceptionType $e) {
		    // catch body
		} catch (OtherExceptionType $e) {
		    // catch body
		}
    }
}
```

## PSR-3
>日志接口  
>主要目标是让类库获得一个Psr\Log\LoggerInterface对象并能通过简单通用的方式来写日志  

1. 8个等级（debug, info, notice, warning, error, critical, alert, emergency）
2. log 方法(等级，日志)；
  - 等级`必须`一致，否则`必须`抛出`Psr\Log\InvalidArgumentException`
  - 日志`必须`是字符串，或者是可`__toString`的对象
3. ....



# 代码检查工具

## [PHP Mess Detector](http://phpmd.org/)

PHP项目体检工具，根据你设定的标准（如单一文件代码体积，未使用的参数个数，未使用的方法数）检查PHP代码，超出设定的标准时报警。

## [PHP Copy Paste Detector](https://github.com/sebastianbergmann/phpcpd)

顾名思义，检查冗余代码的

## [PHP Dead Code Detector](https://github.com/sebastianbergmann/phpdcd)

看名字就知道了，检查从未被调用过的方法

## [PHP Code Sniffer](http://pear.php.net/package/PHP_CodeSniffer)

## [PHPLint](http://www.icosaedro.it/phplint/)

# 持续集成

## [jenkins](http://jenkins-php.org/)

把上述工具以plugins形式整合起来

## [xinc+phing](http://code.google.com/p/xinc/)

跟上述工具集成起来做持续集成后的自动化打包发布


# 参考资料

- [PHP PSR代码标准中文版](https://github.com/hfcorriez/fig-standards)
- [PHP中有什么好的代码自动检查工具吗](https://segmentfault.com/q/1010000000119048)
