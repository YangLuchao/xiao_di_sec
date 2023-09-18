# WEB攻防-通用漏洞&SQL注入&HTTP头XFF&COOKIE&POST请求

![day28](/Users/yangluchao/Documents/GitHub/security/image/day28.png)

```
#知识点：
1、数据请求方式-GET&POST&COOKIE等
2、常见功能点请求方式-用户登录&IP记录等
3、黑盒白盒注入测试要点-SQLMAP注入参数

#补充点：
黑盒测试：功能点分析
白盒测试：功能点分析&关键代码追踪

1.数据库注入    - access mysql mssql oracle mongodb postgresql等
2.数据类型注入 - 数字型 字符型 搜索型 加密型(base64 json)等
3.提交方式注入 - get post cookie http头等 
4.查询方式注入 - 查询 增加 删除 更新 堆叠等
5.复杂注入利用 - 二次注入 dnslog注入 绕过bypass等
```

演示案例：

------

-   GET&POST&COOKIE&SERVER

-   实例黑盒-后台表单登陆框-POST注入

-   实例白盒-ESPCMS-商品购买-COOKIE注入

-   实例白盒-ZZCMS-IP记录功能-HTTP头XFF注入

    ```php
    #部分语言接受代码块
    <?php
    header("Content-Type: text/html; charset=utf-8");
    
    $get=$_GET['g'];
    $post=$_POST['p'];
    $cookie=$_COOKIE['c'];
    $request=$_REQUEST['r'];
    $host=$_SERVER['HTTP_HOST'];
    $user_agent=$_SERVER["HTTP_USER_AGENT"];
    $ip=$_SERVER["HTTP_X_FORWARDED_FOR"];
    
    echo $get."<hr>";
    echo $post."<hr>";
    echo $cookie."<hr>";
    echo $request."<hr>";
    echo $host."<hr>";
    echo $user_agent."<hr>";
    echo $ip;
    ?>
    
    Java Spring  不同框架，不同写法
    method=RequestMethod.GET
    method=RequestMethod.POST
    request.getParameter("参数名");
    可以直接获取get请求的参数key对应的value                               
    也可以从请求体中获取参数的key对应的value
    
    Python flask 不同框架，不同写法
    requests.get
    requests.post
    request.args.get(key)
    request.form.get(key)
    request.values.get(key)
    
    ```

    
