# HTTP消息接口规范（HTTP Message Interface）
link :
 - https://www.php-fig.org/psr/psr-7/

## 0、介绍

- 此文档描述了 「RFC 7230」 和 「RFC 7231」 HTTP 消息传递的接口，还有 「RFC 3986」 里对 HTTP 消息的 URIs 使用。
- HTTP 消息是 Web 技术发展的基础。浏览器或 HTTP 客户端如 curl 生成发送 HTTP 请求消息到 Web 服务器，Web 服务器响应 HTTP 请求。服务端的代码接受 HTTP 请求消息后返回 HTTP 响应消息。
- 通常 HTTP 消息对于终端用户来说是不可见的，但是作为 Web 开发者，我们需要知道 HTTP 机制，如何发起、构建、取用还有操纵 HTTP 消息，知道这些原理，以助我们刚好的完成开发任务，无论这个任务是发起一个 HTTP 请求，或者处理传入的请求。

### 0.1 每一个 HTTP 请求都有专属的格式：
```text
POST /path HTTP/1.1
Host: example.com

foo=bar&baz=bat
```
- 第一行的各个字段意义为： HTTP 请求方法、请求的目标地址（通常是一个绝对路径的 URI 或 者路径），HTTP 协议
- 接下来是 HTTP 头信息，在这个例子中：目的主机
- 接下来是空行
- 然后是消息内容

### 0.2 HTTP 返回消息有类似的结构：
```text
HTTP/1.1 200 OK
Content-Type: text/plain

这是返回的消息内容
```
- 第一行为状态行，包括 HTTP 协议版本，HTTP 状态码，描述文本
- 接下来是 HTTP 头信息，在这个例子中：内容类型
- 接下来是空行
- 然后是消息内容

此文档探讨的是 HTTP 请求消息接口，和构建 HTTP 消息需要的元素数据定义。

## 1、规范详情
### 1.1 消息
- http消息
  - 从客户端发往服务器端的请求（Psr\Http\Message\RequestInterface），必须实现
  - 服务器端返回客户端的响应（Psr\Http\Message\ResponseInterface），必须实现
  - 以上两个接口都继承自（Psr\Http\Message\MessageInterface），必须实现

### 1.2 HTTP 头消息
#### 1.2.1 大小写不敏感的字段名字
- HTTP 消息包含大小写不敏感头信息。使用 MessageInterface 接口来设置和获取头信息，大小写不敏感的定义在于，如果你设置了一个 Foo 的头信息，foo 的值会被重写，你也可以通过 foo 来拿到 FoO 头对应的值。
```php
$message = $message->withHeader('foo', 'bar');
echo $message->getHeaderLine('foo');
// 输出: bar
echo $message->getHeaderLine('FOO');
// 输出: bar
$message = $message->withHeader('fOO', 'baz');
echo $message->getHeaderLine('foo');
// 输出: baz
```

- 虽然头信息可以用大小写不敏感的方式取出，但是接口实现类 必须 保持自己的大小写规范，特别是用 getHeaders() 方法输出的内容。
- 因为一些非标准的 HTTP 应用程序，可能会依赖于大小写敏感的头信息，所有在此我们把主宰 HTTP 大小写的权利开放出来，以适用不同的场景。

#### 1.2.2 对应多条数组的头信息
为了适用一个 HTTP 「键」可以对应多条数据的情况，我们使用字符串配合数组来实现，你可以从一个 MessageInterface 取出数组或字符串，使用 getHeaderLine($name) 方法可以获取通过逗号分割的不区分大小写的字符串形式的所有值。也可以通过 getHeader($name) 获取数组形式头信息的所有值。
```php
$message = $message
    ->withHeader('foo', 'bar')
    ->withAddedHeader('foo', 'baz');

$header = $message->getHeaderLine('foo');
// $header 包含: 'bar, baz'

$header = $message->getHeader('foo');
// ['bar', 'baz']
```

注意：并不是所有的头信息都可以适用逗号分割（例如 Set-Cookie），当处理这种头信息时候， MessageInterface 的继承类 应该 使用 getHeader($name) 方法来获取这种多值的情况。

#### 1.2.3 主机信息
在请求中，Host 头信息通常和 URI 的 host 信息，还有建立起 TCP 连接使用的 Host 信息一致。 然而，HTTP 标准规范允许主机 host 信息与其他两个不一样。

在构建请求的时候，如果 host 头信息未提供的话，实现类库 必须 尝试着从 URI 中提取 host 信息。

RequestInterface::withUri() 会默认的，从传参的 UriInterface 实例中提取 host ， 并替代请求中原有的 host 信息。

你可以提供传参第二个参数为 true 来保证返回的消息实例中，原有的 host 头信息不会被替代掉。

以下表格说明了当 withUri() 的第二个参数被设置为 true 的时，返回的消息实例中调用 getHeaderLine('Host') 方法会返回的内容：
```
{
    当前请求的 Host 头信息
    当前请求 URI 中的 Host 信息
    通过 withUri() 传参进入的 URI 中的 host 信息
}
{
    ''
    ''
    ''
    ''
}
{
    ''
    foo.com
    ''
    foo.com
}
{
    ''
    foo.com
    bar.com
    foo.com
}
{
    foo.com
    ''
    bar.com
    foo.com
}
{
    foo.com
    bar.com
    baz.com
    foo.com
}

```

### 1.3 数据流
HTTP 消息包含开始的一行、头信息、还有消息的内容。HTTP 的消息内容有时候可以很小，有时候确是 非常巨大。尝试使用字符串的形式来展示消息内容，会消耗大量的内存，使用数据流的形式来读取消息 可以解决此问题。StreamInterface 接口用来隐藏具体的数据流读写实现。在一些情况下，消息 类型的读取方式为字符串是能容许的，可以使用 php://memory 或者 php://temp。

StreamInterface 暴露出来几个接口，这些接口允许你读取、写入，还有高效的遍历内容。

数据流使用这个三个接口来阐明对他们的操作能力：isReadable()、isWritable() 和 isSeekable()。这些方法可以让数据流的操作者得知数据流能否能提供他们想要的功能。

每一个数据流的实例，都会有多种功能：可以只读、可以只写、可以读和写，可以随机读取，可以按顺序读取等。

最终，StreamInterface 定义了一个 __toString() 的方法，用来一次性以字符串的形式输出 所有消息内容。

与请求和响应的接口不同的是，StreamInterface 并不强调不可修改性。因为在 PHP 的实现内，基本上没有办法保证不可修改性，因为指针的指向，内容的变更等状态，都是不可控的。作为读取者，可以 调用只读的方法来返回数据流，以最大程度上保证数据流的不可修改性。使用者要时刻明确的知道数据 流的可修改性，建议把数据流附加到消息实例中，来强迫不可修改的特性。


### 1.4 请求目标和URI
根据 「RFC 7230」, 请求消息包含一个"请求目标" 作为请求行 的第二个分割段，可以是一下其中一种形式：
- origin-form：包含路径、查询字符串（如果存在的话），这里同城被称为相对URL。chuan
- absolute-form：
- authority-form：
- asterisk-form：

### 1.5 Service-side Request
### 1.6 Uploaded files