# 5-8.29 网络编程(三) tcp粘包及解决办法

## 1.粘包概念及产生原因

### 1.1粘包概念:

- TCP粘包是指发送方发送的若干包数据到接收方接收时粘成一包，从接收缓冲区看，后一包数据的头紧接着前一包数据的尾。
- 粘包可能由发送方造成，也可能由接收方造成。
- 只有TCP有粘包现象，UDP永远不会粘包
- 粘包不一定会发生

### 1.2粘包原因:

```所谓粘包问题主要还是因为接收方不知道消息之间的界限，不知道一次性提取多少字节的数据所造成的。```

- 发送端原因:  由于TCP协议本身的机制（面向连接的可靠地协议-三次握手机制）客户端与服务器会**维持一个连接**（Channel），数据在连接不断开的情况下，可以**持续不断地将多个数据包发往服务器**，但是如果发送的网络数据包太小，那么他本身会启用Nagle算法（可配置是否启用）**对较小的数据包进行合并**（基于此，TCP的网络延迟要UDP的高些）然后再发送（超时或者包大小足够）。那么这样的话，服务器在接收到消息（数据流）的时候就**无法区分哪些数据包是客户端自己分开发送的**，这样产生了粘包.
- 接收端原因:  服务器在接收到数据库后，**放到缓冲区**中，如果**消息没有被及时从缓存区取走**，下次在取数据的时候可能就会出现**一次取出多个数据包**的情况，造成粘包现象。

## 2. tcp粘包解决办法

- 在每次使用tcp协议发送数据流时,在开头标记一个数据流长度信息,并固定该报文长度(自定义协议).在客户端接收数据时先接收该长度字节数据,判断客户端发送数据流长度,并只接收该长度字节数据,就可以实现拆包,完美解决tcp粘包问题.

```python
#struct模块
#该模块可以把一个类型，如数字，转成固定长度为4的bytes类型
import struct
res = struct.pack('i',12345)	#i表示整数int
print(res,len(res),type(res))  #长度是4

res2 = struct.pack('i',12345111)
print(res,len(res),type(res2))  #长度也是4

unpack_res =struct.unpack('i',res2)
print(unpack_res)  #(12345111,)
print(unpack_res[0]) #12345111

```

```python
###################客户端client###################
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import socket
import struct
sock=socket.socket()	
sock.connect(('127.0.0.1', 13459))	

content1='我好'.encode('utf-8')		#要发送消息
content2='他也好'.encode('utf-8')

con1_len=struct.pack('i',len(content1))	# 计算要发送消息(字节)的长度,并使用struct模块转化为长度为4的字节b'\x06\x00\x00\x00'
sock.send(con1_len)						#先把这个4字节的报文发送
sock.send(content1)						#发送内容

con2_len=struct.pack('i',len(content2))
sock.send(con2_len)
sock.send(content2)


sock.close()
```

```python
###################服务端server###################
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import struct
import socket

sock = socket.socket()		#买手机
sock.bind(('127.0.0.1', 13459))		#插卡
sock.listen(10)  	#开机(同时最大连接10)

conn, addr = sock.accept()		#(受)与cilent端connect(攻)对应.
msg = conn.recv(4)				#首先接收4个字节(4个字节由client端struct模块转化)
len_msg= struct.unpack('i',msg)	#struct模块读取报文,判断跟随数据长度.返回值是一个元祖(6,)
size_msg=len_msg[0]					#取值判断跟随数据长度
msg = conn.recv(size_msg)			#接收报文读取长度字节
print(msg.decode('utf-8'))			#解码输出

msg=conn.recv(4)
len_msg=struct.unpack('i',msg)
size_msg=len_msg[0]
msg = conn.recv(size_msg)
print(msg.decode('utf-8'))


conn.close()
sock.close()

```

**!重要struct模块转化与读取都是对字节进行操作**!