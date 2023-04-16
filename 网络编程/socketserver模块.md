# 5-9.30 网络编程(四)socketserver

## 1. 粘包回顾补充

```python
# tcp协议的粘包现象

# 什么是粘包现象?
    # 发生在发送端的粘包
        # 由于两个数据的发送时间间隔短+数据的长度小
        # 所以由tcp协议的优化机制将两条信息作为一条信息发送出去了
        # 为了减少tcp协议中的“确认收到”的网络延迟时间
    # 发生再接收端的粘包
        # 由于tcp协议中所传输的数据无边界，所以来不及接收的多条
        # 数据会在接收放的内核的缓存端黏在一起
    # 本质： 接收信息的边界不清晰

# 解决粘包问题.
    # 自定义协议1
        # 首先发送报头
            # 报头长度4个字节
            # 内容是 即将发送的报文的字节长度
            # struct模块
                # pack 能够把所有的数字都固定的转换成4字节
        # 再发送报文
    # 自定义协议2
        # 我们专门用来做文件发送的协议
            # 先发送报头字典的字节长度
            # 再发送字典（字典中包含文件的名字、大小。。。）
            # 再发送文件的内容
```

## 2. tcp协议与udp协议的特点:

- tcp协议:是一个面向连接的,流式的(无边界),可靠地,慢的,全双工通信协议.
  - 邮件发送,文件传输,http,web

- udp协议是一个面向数据报的,不可靠,快的,能够完成一对多/多对多/多对一的高效通讯协议
  - 即时聊天工具,视频在线观看

## 3. tcp三次握手与四次挥手

```
# 三次握手
    # accept接受过程中等待客户端的连接
    # connect客户端发起一个syn链接请求
        # 如果得到了server端响应ack的同时还会再收到一个由server端发来的syc链接请求
        # client端进行回复ack之后，就建立起了一个tcp协议的链接
    # 三次握手的过程再代码中是由accept和connect共同完成的，具体的细节再socket中没有体现出来

# 四次挥手
    # server和client端对应的在代码中都有close方法
    # 每一端发起的close操作都是一次fin的断开请求，得到'断开确认ack'之后，就可以结束一端的数据发送
    # 如果两端都发起close，那么就是两次请求和两次回复，一共是四次操作
    # 可以结束两端的数据发送，表示链接断开了
```

## 4. socket    

- 阻塞模型

- 非阻塞模型

## 5. 验证客户端的合法性

- 验证

- hmac/md加密
- 随机字符串:os.path.ran

## 6.socketserver模块

- socketserver实现一对多

- socketserver是socket的再封装，进一步简化了socket sever端的编写

- **固定格式**编写分三步来进行：

  - 先定义一个 处理消息类，继承 socketserver.BaseRequestHandler 类。
  - 重构该类中的 handle() 函数，这是你接收的数据最先到达的函数，同时根据在该类中完善其他的函数。
  - 实例化 socketserver.ThreadingTCPServer()类，将自己构建的类作为参数传递进去。

  ```python
  import socketserver
  
  # 第一步，定义消息处理函数
  class Myserver(socketserver.BaseRequestHandler):
      # 第二步，重写 handle() 类，让其处理消息
      def handle(self):
          while True:
              try:
                  self.data = self.request.recv(1024).strip()
                  print("{} worte:".format(self.client_address[0]))
                  print(self.data)
                  self.request.send(self.data.upper())
               
              except ConnectionResetError as e: # 断开连接通过异常处理
                  print("err",e)
                  break
  
  # 第三步，实例化对象
  server = socketserver.ThreadingTCPServer(('ip', port), Myserver)
  server.server_forever() # 开启服务器
  
  ```

  