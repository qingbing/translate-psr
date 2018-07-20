# 自动加载规范（Autoloading Standard）
link :
 - https://www.php-fig.org/psr/psr-0/

自2014-10-21 PSR-0 被标记为 deprecated(废弃)后，目前推荐使用 PSR-4 规范来替代。

强制要求采取以下条目以确保 autoloader 的互通信

## mandatory（强制性）
```
- 一个完全合格的命名空间必须遵循以下格式：\<Vendor Name>\(<Namespace>\)*<Class Name>
- 每一个命名空间必须有一个顶级的命名空间：<Vendor Name>
- 每一个命名空间可以有多个子命名空间
- 每一个命名空间分隔符在从文件系统中加载时会被转换成 DIRECTORY_SEPARATOR （目录分隔符）
- 每一个在类名中的"_"会被转换成一个 DIRECTORY_SEPARATOR，"_"字符没有意义
- 一个完全合格的命名空间和类在从文件系统中加载时都是一个".php"后缀的文件
- <Vendor Name>、<Namespace>、<Class Name>可以是任意大小写字母的组合
```

## examples（示例）
```
# 普通的类文件系统映射
\Doctrine\Common\IsolatedClassLoader => /path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php
\Symfony\Core\Request => /path/to/project/lib/vendor/Symfony/Core/Request.php
\Zend\Acl => /path/to/project/lib/vendor/Zend/Acl.php
\Zend\Mail\Message => /path/to/project/lib/vendor/Zend/Mail/Message.php
# 带下划线的命名空间和类
\namespace\package\Class_Name => /path/to/project/lib/vendor/namespace/package/Class/Name.php
\namespace\package_name\Class_Name => /path/to/project/lib/vendor/namespace/package_name/Class/Name.php
```

## example Implementation(示例)

最低标准的自动加载规范，在php5.3+可以用以下代码实现
```php
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
spl_autoload_register('autoload');
```