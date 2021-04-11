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
>
> 1. 新建会话
>
> > screen -S hahah  			防止中途过程中网络中断而造成的数据无法恢复、
> >
> > screen -ls 						查看当前有哪些会话
> >
> > screen -r hahah/**编号** 	恢复会话
> >
> > **srceen vim hahah** 		也可以临时创建一个小型会话
>
> 2. 生成密钥(可直接进行密钥登录)
>
> > ssh-keygen 		生成密钥(家目录)
> >
> > cat .ssh/id_rsa.pub  公钥信息
> >
> > cat .ssh/id_rsa   	私钥信息
> >
> > ssh-copy-id   172.21.230.72	服务端服务器IP、
> >
> > 服务器的家目录就会生成一个公钥文件 cat .ssh/authorized_keys
> >
> > 接下来客户端就可直接连接服务器 ssh 172.21.230.72



#### ftp

> apt-get install vsftp  安装vsftp
>
> mkdir /home/ftpuser 创建ftp用户家目录
>
> useradd -d /home/ftpuser -s /bin/bash ftpuser 创建用户
>
> passwd ftpuser 修改密码
>
> vim /etc/vsftp.conf 修改配置文件
>
> vim /etc/allow_users  #向文件中写入使用ftp的用户如:ftpuer
>
> ```
>  userlist_file=/etc/allowed_users   #允许/etc/allowed_users 这个目录下的用户使用ftp
> 
> write_enable=YES  #把权限设为yes能够上传文件
> ```
>
> > 查看文件/etc/ftpusers，文件中的列表是禁止访问用户
>
> * 重启ftp /etc/int.d/vsftp restart
>
>   ##### ftp命令
>
>   <img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210408113721429.png" alt="image-20210408113721429" style="zoom: 67%;" />

#### 防火墙

> raw 流量跟踪  PREROUTING OUTPUT 
>
> mangle 流量整形  PREROUTING INPUT FOWRWARD OUTPUT POSTROUTING
>
> nat 网络地址转换 PREROUTING POSRTOUTING OUTPUT
>
> filter  过滤  INPUT FOWRWARD OUTPUT
>
> **优先级从上到下**
>
> **iptables [-t 表名] 选项 [链名] [条件] [-j 控制类型]**
>
> > **这要是连接端口5901就丢弃**
> >
> > iptables -t mangle -I(插入到第一个)/A(最后一个) INPUT -p tcp --dport 5901 -j DROP 
>
> > **对ping 进行限制**
> >
> > iptables -t filter -p icmp -j ACCEPT/DROP/REJEST
>
> > **时刻观察防火墙状态**
> >
> > watch -n1 iptables -nvL 
> >
> > **查看行号**
> >
> > iptables -nvL --line-numbers
>
> > **删除规则**
> >
> > iptables -D INPUT 1 删除input中的第一条规则
>
> > **清空规则**
> >
> > iptables 【-t 表名】 -F
>
> > **参数**
> >
> > ![image-20210409195912472](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409195912472.png)
>
> > ![image-20210409203804945](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409203804945.png)
> >
> > ![image-20210409204122595](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409204122595.png)
> >
> > ![image-20210409204437001](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409204437001.png)
> >
> > ![image-20210409205837498](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409205837498.png)

#### apache(centos)

> yum install httpd
>
> systemctl restart httpd
>
> systemctl enable httpd 	(加如开机启动项)

> **修改默认文件存放目录**
>
> 默认位置：/var/www/html
>
> **修改**：**vim /etc/http/conf/httpd.conf **
>
> > (119行)   DocumentRoot "/var/www/html"   --> 修改指定位置
> >
> > (124)       <Directory "/vat/www/html"    --> 修改指定位置
>
> **设置每个用户都有独立的网站**
>
> > vim /etc/httpd/conf.d/userdir.conf
> >
> > UserDir  disabled ---> UserDir public_html    这样用户就可以在自己的家目录里新建一个目录为public_html的文件来存放网站html
> >
> > su -jwgkali   
> >
> > mkdir public_html&&cd public_html
> >
> > vim a.html  
> >
> > 最后设置 setcebool -P httpd_enable_homedirs=on
>
> **对网站进行密码登录操作**
>
> > vim /etc/httpd/conf.d/userdir.conf
> >
> > htpasswd -c /etc/httpd/passwd(密码文件) linuxprobe(用户名)   生成该用户网站的密码文件
> >
> > ![image-20210410183354803](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210410183354803.png)
>
> ***使用多个IP来定义网站***
>
> > vim /etc/httpd/conf.d/userdir.conf
> >
> > ```
> > 113 <VirtualHost 192.168.10.10>  定义哪个IP的网站
> > 114 DocumentRoot /home/wwwroot/10  定义这个IP的网站存放路径
> > 115 ServerName www.linuxprobe.com  定义域名
> > 116 <Directory /home/wwwroot/10 >  设定权限
> > 117 AllowOverride None             禁止伪协议
> > 118 Require all granted							所有人都可以访问
> > 119 </Directory>										结束权限标志
> > 120 </VirtualHost>									结束定义标志
> > ```
> >
> > **设置SE上下文权限(限制网站权限)**
> >
> > ```
> > vim /etc/selinux/config
> > 设置
> > SELINUX=enforcing
> > ```
> >
> > **设置这个目的是为了防止黑客拿到网站权限以后肆意破环(限定权限)**
> >
> > 可以使用getenforce命令获得当前SELinux服务的运行模式：
> >
> > > enforcing：强制启用安全策略模式，将拦截服务的不合法请求。
> > >
> > > permissive：遇到服务越权访问时，只发出警告而不强制拦截。
> > >
> > > disabled：对于越权的行为不警告也不拦截。
> >
> > **具体设置**
> >
> > ```
> > [root@linuxprobe ~]# setenforce 1
> > [root@linuxprobe ~]# ls -Zd /var/www/html
> > drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html
> > [root@linuxprobe ~]# ls -Zd /home/wwwroot
> > drwxrwxrwx. root root unconfined_u:object_r:home_root_t:s0 /home/wwwroot
> > [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
> > [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/*
> > ```
>
> ***对网站进行域名设置***
>
> 1. vim /etc/hosts
>
> ```
> 按照这种格式写 IP 域名 域名 ......
> 192.168.10.10 www.linuxprobe.com bbs.linuxprobe.com tech.linuxprobe.com
> ```
>
> 2. 然后创建网站目录
>
> ```
> [root@linuxprobe ~]# mkdir -p /home/wwwroot/www
> [root@linuxprobe ~]# mkdir -p /home/wwwroot/bbs
> [root@linuxprobe ~]# mkdir -p /home/wwwroot/tech
> [root@linuxprobe ~]# echo "WWW.linuxprobe.com" > /home/wwwroot/www/index.html
> [root@linuxprobe ~]# echo "BBS.linuxprobe.com" > /home/wwwroot/bbs/index.html
> [root@linuxprobe ~]# echo "TECH.linuxprobe.com" > /home/wwwroot/tech/index.html
> ```
>
> 3. 修改网站配置信息
>
> ```
> vim /etc/httpd/conf/httpd.conf
> ===========================================
> 113 <VirtualHost 192.168.10.10>
> 114 DocumentRoot "/home/wwwroot/www"
> 115 ServerName "www.linuxprobe.com"
> 116 <Directory "/home/wwwroot/www">
> 117 AllowOverride None
> 118 Require all granted
> 119 </directory> 
> 120 </VirtualHost>
> =============================================
> 121 <VirtualHost 192.168.10.10>
> 122 DocumentRoot "/home/wwwroot/bbs"
> 123 ServerName "bbs.linuxprobe.com"
> 124 <Directory "/home/wwwroot/bbs">
> 125 AllowOverride None
> 126 Require all granted
> 127 </Directory>
> 128 </VirtualHost>
> =============================================
> 129 <VirtualHost 192.168.10.10>
> 130 DocumentRoot "/home/wwwroot/tech"
> 131 ServerName "tech.linuxprobe.com"
> 132 <Directory "/home/wwwroot/tech">
> 133 AllowOverride None
> 134 Require all granted
> 135 </directory>
> 136 </VirtualHost>
> ```
>
> 4. **如果设置SE安全则需要进行设置**
>
> ```
> [root@linuxprobe ~]# ls -Zd /var/www/html
> drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html
> [root@linuxprobe ~]# ls -Zd /home/wwwroot
> drwxrwxrwx. root root unconfined_u:object_r:home_root_t:s0 /home/wwwroot
> 
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www/*
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs/*
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech/*
> [root@linuxprobe ~]# restorecon -Rv /home/wwwroot/
> ```
>
> ***使用端口号进行网站设置***
>
> ```
> vim /etc/httpd/conf/httpd.conf 
>  43 Listen 6111  #设置监听端口号
>  44 Listen 6222
> ====================================== 
> 113 <VirtualHost 192.168.10.10:6111>
> 114 DocumentRoot "/home/wwwroot/6111"  #对应这个端口号的网站目录路径(前提已经创建好了)
> 115 ServerName www.linuxprobe.com
> 116 <Directory "/home/wwwroot/6111">
> 117 AllowOverride None
> 118 Require all granted
> 119 </Directory> 
> 120 </VirtualHost>
> ======================================
> 121 <VirtualHost 192.168.10.10:6222>
> 122 DocumentRoot "/home/wwwroot/6222"
> 123 ServerName bbs.linuxprobe.com
> 124 <Directory "/home/wwwroot/6222">
> 125 AllowOverride None
> 126 Require all granted
> 127 </Directory>
> 128 </VirtualHost>
> ```
>
> **然后设置SE**
>
> ```
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6111
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6111/*
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6222
> [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6222/*
> [root@linuxprobe ~]# restorecon -Rv /home/wwwroot/
> ```
>
> **然后再设置SE对端口号的支持**
>
> ```
> semanage port -l | grep http  #先查看支持没如果支持了就不用再设置了
> ===========进行设置
> [root@linuxprobe ~]# semanage port -a -t http_port_t -p tcp 6111  
> [root@linuxprobe ~]# semanage port -a -t http_port_t -p tcp 6222
> # -a 设置添加
> # -t 端口号
>           
> ```

