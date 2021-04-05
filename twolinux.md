#### telnet

> yum install telnet-server
>
> xinetd 轻量级服务管理器 	telnet 23 明文
>
> vim /etc/xinetd.d/telnet 修改yes为no之后 重启xinet服务
>
> 观察23端口

#### SSH

> ssh wencoll@172.21.230.100
>
> ssh远程连接服务
>
> * 如果要使用图形功能
>
>   > ssh -X wencoll@172.21.230.100
>   >
>   > 如： gedit 文件

#### 防火墙

> ![image-20210404204955088](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210404204955088.png)
>
> 防火墙开启telnet 23号端口
>
> * 普通用户登录系统后有切换root的可能,如何杜绝
>
>   > vim /etc/pam.d/su  第六行只有wheel组成员可以切换成root ,解开注释即可
>
> telnet IP    //telnet 连接服务

> **规则**
>
> * iptables -t filter -nvL 查看防火墙状态

#### openssl 

> * 生成密钥
>
> >  openssl genrsa -out testrsa.key 2048    //输出一个文件大小为2048字节的testrsa.key私钥文件
>
> * 用私钥生成公钥
>
>   >openssl rsa -in testrsa.key -pubout -out testpub.key
>
> * 利用公钥加密想要的文件
>
>   > openssl rsautl -encrypt -inkey testpub.key -pubin -in /tmp/test.txt -out /tmp/new1.txt
>   >
>   > 使用公钥testpub.key加密/tmp/test.txt文件生成最终为/tmp/new1.txt的加密文件
>
> * 利用私钥解密
>
>   > openssl  rsautl -decrypt -inkey testrsa.key -in /tmp/new1.txt -out /tmp/test1.txt
>   >
>   > 使用私钥testrsa.key 解密 /tmp/new1.txt文件最终生成/tmp/test1.txt文件

#### ssh远程控制

> * 不交互登录控制对方
>
> > ssh root@172.21.230.100 "rm -fr /tmp/hello.txt; echo '123' > /tmp/hello.txt"
>
> * 拷贝对方文件
>
> > scp root@172.21.230.100:/tmp/hello.txt /root/Desktop
>
> * 把本地文件拷贝对方服务器
>
> > scp /root/Desktop/hello.txt root@172.21.230.100:/root/Desktop
>
> * 使用ssh协议ftp形式登录对方主机实现文件上传与下载
>
> > sftp root@172.21.230.100
> >
> > get 文件
> >
> > put 文件

