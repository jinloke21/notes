### udp客户端

```python
import socket
import sys


def mian(argv):
  #创建套接字，类型为ipv4(AF_INET),协议为UDP(SOCK_DGRAM)
    up_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    while 1:
        send_data=input("input your speak:")
        
        if send_data=="break":
            break
        #发送数据给指定的Ip和端口号，内容类型为utf-8
       # up_socket.sendto(b"hahhahah",("172.21.204.107",8080))
        up_socket.sendto(send_data.encode("utf-8"),(argv,8080))
    #使用完要关闭套接字
    up_socket.close()

if __name__=="__main__":
    mian(sys.argv[1])
```

### tcp服务端

```python
import socket

def main():
	  #创建套接字，类型为ipv4(AF_INET),协议为TCP(SOCK_STREAM)
    tcp_socket=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
		
    #服务端绑定端口和Ip
    tcp_socket.bind(("",8080))
		
    #设置同时能接听为128台服务
    tcp_socket.listen(128)
    while True:
        print("wait a newclinet come to...")
        #阻塞等待客户端连接，accpet函数返回值为跟客户端连接的套接字和客户端的IP和端口号
        new_client,ipaddr = tcp_socket.accept()
        print("a new clinet come here!....")

        print(str(ipaddr))
        while 1:
						#TCP接受数据使用recv函数
            #UDP接受数据使用recvfrom函数
            recv_data = new_client.recv(1024)
            #对发来的数据进行utf-8解码
            if recv_data.decode("utf-8")=="break":
                break;
            else:
                print("data from clinet:%s"%recv_data.decode("utf-8"))
								
                #TCP发送数据使用send函数
                #UDP发送数据使用sendto函数
                new_client.send("-------ok---------".encode("utf-8"))

        #同理使用完关闭套接字
        new_client.close()
        print("had to close")
        tcp_socket.close()


if __name__ =="__main__":
    main()
```

### UDP服务端

```python
import socket
import sys

def main():

  a = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
  #绑定ip和端口，只能绑定自己
  localaddr=("",8080)
  #不用说服务端肯定绑定ip和端口
  a.bind(localaddr)

  #不用使用accpect函数进行阻塞等待
  while 1:
      #一直接受信息，大小为1024
      data = a.recvfrom(1024)
      #打印元组中的正确信息
      data1 = data[0]
      data2 = data[1] #对方客户端的IP和端口号
      if data1 == "break":
          break
      else:
          #解码utf-8
          print("%s:%s"%(str(data2),data1.decode("utf-8")))
  a.close()

if __name__ =="__main__":
    main()

```

### TCP客户端

```python
import socket

def main():

      up_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

      ipaddr = input("请输入你要传的ip:")
      ports = int(input("请输入端口:"))

      new_client=(ipaddr,ports)
      #tcp需要进行连接服务器
      if up_socket.connect(new_client):
          print("connect is ok!")

      send_data = input("请输入你要传的数据:")

      up_socket.send(send_data.encode("utf-8"));
      if up_socket.recv(1024):
          data = up_socket.recv
          print("data from service:%s"% str(data))


      else:
          up_socket.close()

if __name__ =="__main__":
    main()

```

