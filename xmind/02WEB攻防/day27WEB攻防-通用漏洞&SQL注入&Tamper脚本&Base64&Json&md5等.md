# WEB攻防-通用漏洞&SQL注入&Tamper脚本&Base64&Json&md5等

![day27](/Users/yangluchao/Documents/GitHub/security/image/day27.png)

```
#知识点：
1、数据表现格式类型注入
2、字符转义绕过-宽字节注入
3、数字&字符&搜索&编码&加密等

#参考资料：
https://www.cnblogs.com/bmjoker/p/9326258.html
扫描，利用工具等都不会自动判断数据类型，格式等，所以即使有漏洞也测不出来，具体还是需要人工的去观察，进行工具的修改或加载插件再次探针才可以。


#SQL注入课程体系：
1.数据库注入    - access mysql mssql oracle mongodb postgresql等
2.数据类型注入 - 数字型 字符型 搜索型 加密型(base64 json)等
3.提交方式注入 - get post cookie http头等 
4.查询方式注入 - 查询 增加 删除 更新 堆叠等
5.复杂注入利用 - 二次注入 dnslog注入 绕过bypass等

```

演示案例：

------

-   本地源码-数字&字符&搜索&编码&JSON

-   墨者靶场-字符转义处理防护-宽字节注入

-   真实WEB-数据编码接受处理-base64注入

-   真实WEB-JSON数据格式&MD5加密数据

-   工具脚本-SQLMAP-脚本Tamper使用指南

    