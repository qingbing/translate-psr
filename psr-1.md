# 基本编码规范（Basic Coding Standard）
link :
 - https://www.php-fig.org/psr/psr-1/
 
## 1.概览
- MUST：PHP代码文件 只能使用"<?php" 或者 "<?=" 标签开始
- MUST：PHP代码文件 必须使用无 BOM 的 UTF-8 编码格式
- SHOULD：PHP代码 应该只有声明符号（类、函数、常量等）或者 SHOULD NOT：产生其他副作用的操作（产生输出，修改.ini配置文件等），二选一
- MUST：命名空间和类必须遵循"autoloading", [PSR-0, PSR-4]
- MUST：类名必须符合首字母大写的驼峰命名规范：StudlyCaps
- MUST：类常量必须全部大写，多个单词之间用"_"隔开
- MUST：方法名必须用首字母小写的驼峰命名规范：camelCase

## 2.文件
### 2.1 PHP 标签
- MUST：使用长标签"<?php ?>" 或短标签"<?= ?>"
- MUST NOT：不能使用其他形式的标签

### 2.2 字符编码
- MUST：必须使用无 BOM 的 UTF-8 编码方式

### 2.3 从属效应（副作用）
- 一个文件要么定义新标记（类，函数，常量等），要么产生从属效果的操作，不能两者都有
- 从属效应“side effects”：通过包含文件，不直接声明类、函数、常量等，而执行的逻辑操作

“Side effects”包括但不限于以下方式：
- 生成输出
- 直接require或include
- 连接外部服务
- 修改 ini 配置
- 抛出错误和异常
- 修改全局或静态变量
- 读取或写文件

#### 反例（不允许）：既有副作用，又有声明
```php
<?php
// [副作用] : 修改 ini 配置
ini_set('error_reporting', E_ALL);

// [副作用] : 引入文件
include "file.php";

// [副作用] : 生成输出
echo "<html>\n";

// 声明[declarations]
function foo()
{
    // function body
}
```

#### 例子：没有副作用的声明
```php
<?php
// 声明函数
function foo()
{
    // 函数主体部分
}

// 条件声明 不属于「副作用」
if (! function_exists('bar')) {
    function bar()
    {
        // 函数主体部分
    }
}
```

## 3.命名空间和类名
命名空间和类 MUST 遵循 “autoloading”的 PSR : [PSR-0,PSR-4]。

根据规范，每一个类都是用其自己的类名作为文件名，每一个命名空间至少都有一个层次：顶级的组织名称（vendor name）

类名 MUST 需要采用首字母大写驼峰法：StudlyCaps

php5.3及以后版本的代码（MUST）采用正式的命名空间

例子：
```php
<?php
// PHP 5.3及以后版本的写法
namespace Vendor\Model;

class Foo
{
}
```

5.2.x 及之前的版本 [SHOULD] 使用伪命名空间的写法，约定俗成使用顶级的组织名称（vendor name）如 Vendor_ 为类前缀。
```php
<?php
// 5.2.x及之前版本的写法
class Vendor_Model_Foo
{
}
```
## 4.类常量、属性、方法
这里的类名泛指所有的 类（classes）、接口（interfaces）、可复用代码块（traits）
### 4.1 类常量
类常量 必须[MUST] 全部声明为大写
```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2 属性
类属性命名可遵循：
- 首字母大写的驼峰法：$StudlyCaps
- 首字母小写的驼峰法：$camelCase——作者首推
- 下划线分割：$under_score

上述命名规范不做强制，但是，无论采用哪种，在一定范围内 应该[SHOULD] 统一。这个范围可以是：团队、包、类、方法

### 4.3 方法
方法必须采用首字母小写的驼峰法：camelCase()