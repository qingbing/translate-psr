# 日志接口规范（Logging Interface）
link :
 - https://www.php-fig.org/psr/psr-4/
 
## 1、概览
本规范是关于类的自动加载对应文件路径的说明。包括自动载入的类对应的文件存放路径规范。

## 2、详细说明
### 2.1 这里“类”泛指 类（classes）、接口（interfaces），复用代码块（traits）以及其他类似结构
### 2.2 完整类型拥有一下结构形式：
```shell
 \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>
```
- 完整的类名[MUST]必须有顶级命名空间，被称为“vendor namespace”
- 完整的类名[MAY]可以有一个或多个自命名空间
- 完整的类名[MUST]必须有一个最终的类名
- 完整的类名中的下划线没有任何特殊意义
- 完整的类名[MAT]可以有任意字母的大小写组成
- 所有的类名都是大小写敏感的

### 2.3 根据完整的类名载入类文件
- 完整类名中去掉最前面的命名空间分隔符，前面连续的一个或多个命名空间和字命名空间，作为"命名空间前缀"，其[MUST]必须与至少一个文件基目录相对应
- 紧接命名空间前缀后的子命名空间[MUST]必须与相应的"文件基目录"相匹配，其中命名空间分隔符将作为目录分隔符
- 末尾的类名[MUST]必须与对应的以".php"为后缀的文件同名

### 2.4 自动记载器（autoloader）的实现特点
- [MUST NOT]不能抛出异常
- [MUST NOT]不能促发任意级别的错误信息
- [SHOULD NOT]不该有返回值

## 3、示例
下表展示了符合规范完整类名、命名空间前缀和文件基目录所对应的文件路径
```
{
    完整类名
    命名空间前缀
    文件基目录
    文件路径
}
{
    \Acme\Log\Writer\File_Writer
    Acme\Log\Writer
    ./acme-log-writer/lib/
    ./acme-log-writer/lib/File_Writer.php
}
{
    \Aura\Web\Response\Status
    Aura\Web
    /path/to/aura-web/src/
    /path/to/aura-web/src/Response/Status.php
}
{
    \Symfony\Core\Request
    Symfony\Core
    ./vendor/Symfony/Core/
    ./vendor/Symfony/Core/Request.php
}
{
    \Zend\Acl
    Zend
    /usr/includes/Zend/
    /usr/includes/Zend/Acl.php
}
```