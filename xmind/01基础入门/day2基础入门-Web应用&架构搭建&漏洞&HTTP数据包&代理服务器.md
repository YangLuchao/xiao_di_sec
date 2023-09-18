# 基础入门-Web应用&架构搭建&漏洞&HTTP数据包&代理服务器                 

![day2](/Users/yangluchao/Documents/GitHub/security/image/day2.png)

```
#知识点：
1、网站搭建前置知识
2、WEB应用环境架构类
3、WEB应用安全漏洞分类
4、WEB请求返回过程数据包
```

```
#网站搭建前置知识
域名，子域名，DNS，HTTP/HTTPS，证书等

#WEB应用环境架构类
理解不同WEB应用组成角色功能架构:
开发语言，程序源码，中间件容器，数据库类型，服务器操作系统，第三方软件等
开发语言：asp,php,aspx,jsp,java,python,ruby,go,html,javascript等
程序源码：根据开发语言分类；应用类型分类；开源CMS分类；开发框架分类等
中间件容器：IIS,Apache,Nginx,Tomcat,Weblogic,Jboos,glasshfish等
数据库类型：Access,Mysql,Mssql,Oracle,db2,Sybase,Redis,MongoDB等
服务器操作系统：Windows系列，Linux系列，Mac系列等
第三方软件：phpmyadmin,vs-ftpd,VNC,ELK,Openssh等

#WEB应用安全漏洞分类
SQL注入，文件安全，RCE执行，XSS跨站，CSRF/SSRF/CRLF，
反序列化，逻辑越权，未授权访问，XXE/XML，弱口令安全等

#WEB请求返回过程数据包参考：
https://www.jianshu.com/p/558455228c43
https://www.cnblogs.com/cherrycui/p/10815465.html
请求数据包，请求方法，请求体，响应包，响应头，状态码，代理服务器等
Request,Response,User-Agent,Cookie,Server,Content-Length等
```

------

请求头含义

```
Accept：指浏览器或其他客户可以接爱的MIME文件格式。Servlet可以根据它判断并返回适当的文件格式。

User-Agent：是客户浏览器名称

Host：对应网址URL中的Web名称和端口号。

Accept-Langeuage：指出浏览器可以接受的语言种类，如en或en-us，指英语。

connection：用来告诉服务器是否可以维持固定的HTTP连接。http是无连接的，HTTP/1.1使用Keep-Alive为默认值，这样，当浏览器需要多个文件时(比如一个HTML文件和相关的图形文件)，不需要每次都建立连接

Cookie：浏览器用这个属性向服务器发送Cookie。Cookie是在浏览器中寄存的小型数据体，它可以记载和服务器相关的用户信息，也可以用来实现会话功能。

Referer：表明产生请求的网页URL。如比从网页/icconcept/index.jsp中点击一个链接到网页/icwork/search，在向服务器发送的GET/icwork/search中的请求中，Referer是http://hostname:8080/icconcept/index.jsp。这个属性可以用来跟踪Web请求是从什么网站来的。

Content-Type：用来表名request的内容类型。可以用HttpServletRequest的getContentType()方法取得。

Accept-Charset：指出浏览器可以接受的字符编码。英文浏览器的默认值是ISO-8859-1.

Accept-Encoding：指出浏览器可以接受的编码方式。编码方式不同于文件格式，它是为了压缩文件并加速文件传递速度。浏览器在接收到Web响应之后先解码，然后再检查文件格式。
```

状态码分类

| 分类 | 分类描述                                       |
| :--- | :--------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

状态码列表

| 状态码 | 状态码英文名称                  | 中文描述                                                     |
| :----- | :------------------------------ | :----------------------------------------------------------- |
| 100    | Continue                        | 继续。客户端应继续其请求                                     |
| 101    | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|        |                                 |                                                              |
| 200    | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201    | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202    | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203    | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204    | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205    | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206    | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
|        |                                 |                                                              |
| 300    | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301    | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302    | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303    | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304    | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305    | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306    | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307    | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
|        |                                 |                                                              |
| 400    | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401    | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402    | Payment Required                | 保留，将来使用                                               |
| 403    | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404    | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405    | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406    | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407    | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408    | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409    | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410    | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411    | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412    | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413    | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414    | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415    | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416    | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417    | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
|        |                                 |                                                              |
| 500    | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501    | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502    | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503    | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504    | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505    | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |

![day2_1](/Users/yangluchao/Documents/GitHub/security/image/day2_1.png)

![day2_2](/Users/yangluchao/Documents/GitHub/security/image/day2_2.png)

![day2_3](/Users/yangluchao/Documents/GitHub/security/image/day2_3.png)

演示案例：

-   架构-Web应用搭建-域名源码解析
-   请求包-新闻回帖点赞-重放数据包
-   请求包-移动端&PC访问-自定义UA头
-   返回包-网站文件目录扫描-返回状态码
-   数据包-WAF文件目录扫描-代理服务器
