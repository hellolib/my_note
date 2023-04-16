# HTML5 WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

![img](https://www.runoob.com/wp-content/uploads/2016/03/ws.png)

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。

当你获取 Web Socket 连接后，你可以通过 **send()** 方法来向服务器发送数据，并通过 **onmessage** 事件来接收服务器返回的数据。

以下 API 用于创建 WebSocket 对象。

```
var Socket = new WebSocket(url, [protocol] );
```

以上代码中的第一个参数 url, 指定连接的 URL。第二个参数 protocol 是可选的，指定了可接受的子协议。

------

## WebSocket 属性

以下是 WebSocket 对象的属性。假定我们使用了以上代码创建了 Socket 对象：

| 属性                  | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| Socket.readyState     | 只读属性 **readyState** 表示连接状态，可以是以下值：0 - 表示连接尚未建立。1 - 表示连接已建立，可以进行通信。2 - 表示连接正在进行关闭。3 - 表示连接已经关闭或者连接不能打开。 |
| Socket.bufferedAmount | 只读属性 **bufferedAmount** 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。 |

------

## WebSocket 事件

以下是 WebSocket 对象的相关事件。假定我们使用了以上代码创建了 Socket 对象：

| 事件    | 事件处理程序     | 描述                       |
| :------ | :--------------- | :------------------------- |
| open    | Socket.onopen    | 连接建立时触发             |
| message | Socket.onmessage | 客户端接收服务端数据时触发 |
| error   | Socket.onerror   | 通信发生错误时触发         |
| close   | Socket.onclose   | 连接关闭时触发             |

------

## WebSocket 方法

以下是 WebSocket 对象的相关方法。假定我们使用了以上代码创建了 Socket 对象：

| 方法           | 描述             |
| :------------- | :--------------- |
| Socket.send()  | 使用连接发送数据 |
| Socket.close() | 关闭连接         |

------

## WebSocket 实例

WebSocket 协议本质上是一个基于 TCP 的协议。

为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。

### 客户端的 HTML 和 JavaScript

目前大部分浏览器支持 WebSocket() 接口，你可以在以下浏览器中尝试实例： Chrome, Mozilla, Opera 和 Safari。

runoob_websocket.html 文件内容

```
<!DOCTYPE HTML>
<html>
   <head>
   <meta charset="utf-8">
   <title>菜鸟教程(runoob.com)</title>
    
      <script type="text/javascript">
         function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
                
               ws.onopen = function()
               {
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
                
               ws.onmessage = function (evt) 
               { 
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
                
               ws.onclose = function()
               { 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
            
            else
            {
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
      </script>
        
   </head>
   <body>
   
      <div id="sse">
         <a href="javascript:WebSocketTest()">运行 WebSocket</a>
      </div>
      
   </body>
</html>
```

------

## 安装 pywebsocket

在执行以上程序前，我们需要创建一个支持 WebSocket 的服务。从 [pywebsocket](https://github.com/google/pywebsocket) 下载 mod_pywebsocket ,或者使用 git 命令下载：

```
git clone https://github.com/google/pywebsocket.git
```

mod_pywebsocket 需要 python 环境支持

mod_pywebsocket 是一个 Apache HTTP 的 Web Socket扩展，安装步骤如下：



- 解压下载的文件。

- 进入 **pywebsocket** 目录。

- 执行命令：

  ```
  $ python setup.py build
  $ sudo python setup.py install
  ```

- 查看文档说明:

  ```
  $ pydoc mod_pywebsocket
  ```

### 开启服务

在 **pywebsocket/mod_pywebsocket** 目录下执行以下命令：

```
$ sudo python standalone.py -p 9998 -w ../example/
```

以上命令会开启一个端口号为 9998 的服务，使用 -w 来设置处理程序 echo_wsh.py 所在的目录。

现在我们可以在 Chrome 浏览器打开前面创建的 runoob_websocket.html 文件。如果你的浏览器支持 WebSocket(), 点击"运行 WebSocket"，你就可以看到整个流程各个步骤弹出的窗口，

# Django中的websocket的实现

 https://www.cnblogs.com/baijinshuo/p/10550484.html 

## 1.安装dwebsocket

```
pip install  dwebsocket2
```

## 2.使用方法

​		如果你想为一个单独的视图处理一个websocklet连接可以使用accept_websocket装饰器，它会将标准的HTTP请求路由到视图中。使用require_websocke装饰器只允许使用WebSocket连接，会拒绝正常的HTTP请求。

​		在设置中添加设置MIDDLEWARE_CLASSES=dwebsocket.middleware.WebSocketMiddleware这样会拒绝单独的视图实用websocket，必须加上accept_websocket 装饰器。

​		设置WEBSOCKET_ACCEPT_ALL=True可以允许每一个单独的视图实用websockets

### 属性方法：

**`1.request.is_websocket()`**如果是个websocket请求返回True，如果是个普通的http请求返回False,可以用这个方法区分它们。

`2.request.websocket`在一个websocket请求建立之后，这个请求将会有一个websocket属性，用来给客户端提供一个简单的api通讯，如果`request.is_websocket()是False，这个属性将是None。`

`3.WebSocket.wait()`返回一个客户端发送的信息，在客户端关闭连接之前他不会返回任何值，这种情况下，方法将返回None

`4.WebSocket.read()` 如果没有从客户端接收到新的消息，read方法会返回一个新的消息，如果没有，就不返回。这是一个替代wait的非阻塞方法

`5.WebSocket.count_messages()` 返回消息队列数量

`6.WebSocket.has_messages()`如果有新消息返回True，否则返回False

`7.WebSocket.send(message)`向客户端发送消息

`8.WebSocket.__iter__()`websocket迭代器

## 3.示例

 功能：让我们从客户端接收一条消息，将该消息发送回客户端并关闭连接。 

1.新建一个django项目

![img](https://images2015.cnblogs.com/blog/668465/201703/668465-20170324153958002-1404927354.png)

 

2.新建index.html在templates文件夹下，编写我们的客户端

```html
<!DOCTYPE html>
<html>
<head>
    <title>django-websocket</title>
    <script src="http://code.jquery.com/jquery-1.11.1.min.js"></script>
    <script type="text/javascript">//<![CDATA[
    $(function () {
        $('#send_message').click(function () {
            var socket = new WebSocket("ws://" + window.location.host + "/echo_once/");
            socket.onopen = function () {
                console.log('WebSocket open');//成功连接上Websocket
                socket.send($('#message').val());//发送数据到服务端
            };
            socket.onmessage = function (e) {
                console.log('message: ' + e.data);//打印服务端返回的数据
                $('#messagecontainer').prepend('<p>' + e.data + '</p>');
            };
        });
    });
    //]]></script>
</head>
<body>
<br>
<input type="text" id="message" value="Hello, World!"/>
<button type="button" id="send_message">发送 message</button>
<h1>Received Messages</h1>
<div id="messagecontainer">

</div>
</body>
</html>
```

 3.app的views.py编写我们的服务端

```python
from dwebsocket import require_websocket

@require_websocket
def echo_once(request):
    message = request.websocket.wait()
    request.websocket.send(message)
```

 4.url路由设置

```python
from demo import views as v

urlpatterns = [

    url(r'^index/', v.index),
    url(r'^echo_once', v.echo_once),
]
```

