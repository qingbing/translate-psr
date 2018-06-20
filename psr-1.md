# 基本编码规范（Basic Coding Standard）

## 限定词
参考[RFC 2119]http://www.ietf.org/rfc/rfc2119.txt
- MUST : 严格遵循，无条件遵守
- MUST NOT : 绝对禁止
- SHOULD : 强烈建议这样做，不强求
- SHOULD NOT : 强烈建议不这样做，不强求
- MAY : 可以
- OPTIONAL : 可选
 
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

### 2.3 副作用


https://laravel-china.org/topics/2078/psr-specification-psr-1-basic-coding-specification
https://www.php-fig.org/psr/psr-1/