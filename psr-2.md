# 编码风格规范（Coding Style Guide）
link :
 - https://www.php-fig.org/psr/psr-2/

本规范作为 PSR-1(Basic Coding Standard)的扩展使用。

本规范通过制定一系列规范化PHP代码的规则，以减少 phper 在阅读其他同学代码时因代码风格的不同而造成不便。

当多名程序员在项目中合作时，就需要一个共同的编码规范。

本规范的价值在于我们都遵循这个编码风格，而不是在于它本身。

## 1、概览
- [MUST]代码必须遵循[PSR-1]的编码规范。
- [MUST]代码使用4个空格而不是[Tab 键]进行缩进
- 每行字符数[SHOULD]保持在80个以内，理论上[MUST NOT]不能多余120个
- [MUST]在"namespace"命名空间申明语句和"use"命令语句块后必须插入一个空白行
- [MUST]类开始的花括号"{"必须单独成行，关闭的花括号"}"必须单独成行
- [MUST]方法开始的花括号"{"必须单独成行，关闭的花括号"}"必须单独成行
- [MUST]类属性和方法必须声明添加访问修饰符(private、protected、public)，abstract必须在访问修饰符之前，而static必须申明在防卫修饰符之后
- [MUST]控制结构的关键字后必须有一个空格符，[MUST NOT]而调用方法或函数时一定不能有空格
- [MUST]控制结构的开始花括号"{"必须写在申明的同一行，而[MUST]结束花括号"}"必须卸载主体后自成一行
- [MUST]控制结构的开始左括号和结束右括号前，都一定不可有空格符

例子：
```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
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
        // 方法的内容
    }
}
```

## 2、通则
### 2.1 基本代码标准
必须遵循[PSR-1]基本代码规范
### 2.2 文件
- [MUST] 所有PHP文件必须使用"Unix LF (linefeed)"作为行的结束符
- [MUST] 所有PHP文件必须以一个空白行作为结束
- [MUST] 纯的PHP代码文件必须省略最后的"?>"结束标签
### 2.3 行
- [MUST NOT] 行的长度不能有硬性的约束
- [MUST] 软性的长度约束必须要限制在120以内，超过此长度，带代码规范检查的编辑器必须要发出警告
- [SHOULD NOT]每行的长度不应该多余80个字符，大于80个字符应该折成多行
- [MUST NOT]非空行后一定不能有多余的空格符
- [MAY]空行可以使得阅读代码更加方便，以及有助于代码的分块
- [MUST NOT]一行不能存在多条语句

### 2.4 缩进
代码[MUST]必须使用4个空格作为缩进，而[MUST NOT]不能使用"Tab 键"。好处：
- 避免在比较代码差异、打补丁、重阅代码及注释时产生混淆
- 使用空格缩进，让代码变得更方便

### 关键字和 True/False/Null
- [MUST] PHP所有的关键字必须小写
- [MUST] 常量 true、false、null也必须小写

## 3、命名空间和 User 申明
- [MUST] namespace 生命后必须插入一个空白行
- [MUST] 所有 use 必须在 namespace 后声明
- [MUST] 每条 use 声明语句必须只有一个 use 关键字
- [MUST] use 声明语句块后，必须插入一个空白行

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... 更多的 PHP 代码在这里 ...
```
## 4、类、属性和方法
这里的类繁殖所有的 classes、interfaces 和 traits
### 4.1 扩展[extend] 和 继承[implement]
- [MUST] "extend" 和 "implement" 关键字必须声明在类名的同一行
- [MUST] 类的开始花括号"{"必须自成一行，关闭的花括号"}"必须在类主体后独占一行
```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // 这里面是常量、属性、类方法
}
```
- [MAY] implement 继承的列表可以分成多行，这样每个继承的接口就[MUST]分开独立成行，包括第一个
```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // 这里面是常量、属性、类方法
}
```

### 4.2 属性[Property]
- [MUST] 所有的属性都必须声明访问修饰符
- [MUST NOT] 关键字"var"一定不能用于定义属性
- [MUST NOT] 每条语句不能定义超过一个属性
- [SHOULD NOT] 不该使用下划线来区分属性是protected或private
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3 方法[method]
- [MUST] 所有的方法必须添加访问修饰符
- [SHOULD NOT] 不该使用下划线来区分方法是protected或private
- [MUST NOT] 方法名称后一定不能有空格，其开始花括号"{"必须独占一行，结束花括号"}"在方法主体后单独成一行。参数左括号后和有右括号前不能有空格
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

### 4.4 方法参数
- [MUST] 参数列表中，每个逗号后面必须有一个空格，逗号前[MUST NOT]不能有空格
- [MUST] 如果有默认的参数，必须放在列表参数的末尾
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```
- [MAY] 列表参数可以分成多行，这样，包括第一个参数在内的每个参数都必须单独成行
- [MUST] 拆分成多行的参数列表后，结束括号以及方法开始花括号必须在同一行，中间用一个空格分隔
```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // 方法的内容
    }
}
```

### 4.5 abstract，final和static
- [MUST] 需要添加 abstract 或 final 声明时，必须写在访问修饰符前
- [MUST] 需要添加 static 声明时，必须写在访问修饰符后
```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6 方法和函数调用
- [MUST NOT] 方法和函数调用时，方法名和函数名与参数左括号之间不能有空格
- [MUST NOT] 方法和函数调用时，参数右括号前不能有空格
- [MUST NOT] 每个参数前一定不能有空格，但其后[MUST]必须有一个空格
```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```
- [MAY] 参数可以分列成多行，此时包括第一个参数在内的每个参数都必须单独成行
```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```


## 5、控制结构
控制结构的规范
- [MUST] 控制结构的关键字后面必须有一个空格
- [MUST NOT] 左括号"("后不能有空格
- [MUST NOT] 右括号")"前不能有空格
- [MUST NOT] 右括号")"和开始花括号"{"之间必须有一个空格
- [MUST] 结构体主体必须有一次缩进
- [MUST] 结束花括号"}"必须在结构体主体后单独成行
- [MUST] 每个结构体的主体都必须被包含在成对的花括号之中，这能让结构体更加结构化，以及减少加入新航时，出错的可能行

### 5.1 if,elseif,else
- [MUST] else 和 elseif 都与前面的结束花括号"}"在同一行
- [SHOULD] 使用 "elseif" 替代 所有的 "else if"
```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

### 5.2 switch,case
- [MUST] case 语句 相对于 switch 进行一次缩进
- [MUST] break 语句以及 case 内的其它语句都必须与 case 语句进行一次缩进
- [MUST] 如果存在非空的 case 直穿语句，主体里面必须有类似 "// no break" 的注释
```php
<?php
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
```

### 5.3 while, do while
while 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
while ($expr) {
    // structure body
}
```

do while 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4 for
for 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```
### 5.5 foreach
foreach 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6 try,catch
try,catch 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

## 6、闭包[closures]
- [MUST] 闭包声明时，关键词function后以及 use 关键词的前后都必须有一个空格
- [MUST] 开始花括号"{"必须写在神明的同一行，结束花括号"}"必须紧跟主题结束的下一行
- [MUST NOT] 参数列表的左括号后和右括号前，不能有空格
- [MUST NOT] 参数和变量列表中，逗号前不能有空格，逗号后[MUST]必须有空格
- [MUST] 闭包中有默认值的参数必须放在列表的后面
闭包 的规范，注意其「括号」、「空格」以及「花括号」的位置
```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```
- [MAY] 参数列表以及变量列表可以分成多行，这样，包括第一个在内的参数或变量都必须单独成行，而列表的右括号与闭包的开始花括号[MUST]必须放在同一行
```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

