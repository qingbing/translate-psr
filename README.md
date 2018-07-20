# PSR 规范（译）
link :
 - https://www.php-fig.org/psr/psr-1/
 - https://psr.phphub.org/
 - https://laravel-china.org/docs/psr
 
 
 https://www.kancloud.cn/thinkphp/php-fig-psr/3140

## 限定词
参考[RFC 2119]http://www.ietf.org/rfc/rfc2119.txt
- MUST : 严格遵循，无条件遵守
- MUST NOT : 绝对禁止
- SHOULD : 强烈建议这样做，不强求
- SHOULD NOT : 强烈建议不这样做，不强求
- MAY : 可以
- OPTIONAL : 可选

## Accepted(通过)
```
1. Basic Coding Standard : 基础代码规范
2. Coding Style Guide : 编码风格规范
3. Logger Interface : 日志接口规范
4. Autoloading Standard : 自动加载规范
6. Caching Interface : 缓存接口规范
7. HTTP Message Interface : HTTP消息接口规范
11. Container Interface : 服务器容器接口
13. Hypermedia Links : 超媒体链接规范
15. HTTP Handlers : HTTP句柄
16. Simple Cache : 简单缓存
```

## Review（审查）
```
12. Extended Coding Style Guide : 扩展编码风格规范
```

## draft（草案）
```
14. Event Manager : 事件管理规范
17.	HTTP Factories : HTTP 收藏规范
18.	HTTP Client : HTTP Client 规范
```

## Abandoned（放弃）
```
5. PHPDoc Standard : PHPDoc 标准
8. Huggable Interface : Huggable 接口
9. Security Advisories : 安全公示
10. Security Reporting Process : 安全上报过程
```

## Deprecated（废弃）
```
0. Autoloading Standard : 自动加载规范
```

# 列表
```
0. Autoloading Standard	                Deprecated
1. Basic Coding Standard                Accepted
2. Coding Style Guide                   Accepted
3. Logger Interface                     Accepted
4. Autoloading Standard	                Accepted
5. PHPDoc Standard                      Abandoned
6. Caching Interface                    Accepted
7. HTTP Message Interface               Accepted
8. Huggable Interface                   Abandoned
9. Security Advisories                  Abandoned
10. Security Reporting Process          Abandoned
11. Container Interface                 Accepted
12. Extended Coding Style Guide         Review
13. Hypermedia Links                    Accepted
14. Event Manager                       Draft
15. HTTP Handlers                       Accepted
16. Simple Cache                        Accepted
17. HTTP Factories                      Draft
18. HTTP Client	Tobias Nyholm           Draft
```