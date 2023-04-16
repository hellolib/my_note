# Django信号系统

- django自带一套信号发射系统来帮助我们在框架的不同位置传递信息．也就是说，当某一事件发生时，信号系统可以允许一个或多个发送者（senders）将通知或信号（signals）推送给一组接受者（receivers）.

- 信号系统包含以下要素：
  1. 发送者（senders）－谁发送了信号
  2. 信号（signals）－发送的信号本身
  3. 接收者（receivers）－信号是发给谁的

## Django中内置的signal

### 1. model signal

```python
#django.db.models.signals.pre_save
pre_init                        # Django中的model对象执行其构造方法前,自动触发
post_init                       # Django中的model对象执行其构造方法后,自动触发
pre_save                        # Django中的model对象保存前,自动触发
post_save                       # Django中的model对象保存后,自动触发
pre_delete                      # Django中的model对象删除前,自动触发
post_delete                     # Django中的model对象删除后,自动触发
m2m_changed                     # Django中的model对象使用m2m字段操作数据库的第三张表(add,remove,clear,update),自动触发
class_prepared                  # 程序启动时,检测到已注册的model类,对于每一个类,自动触发
```

### 2.Managemeng_signals

```
pre_migrate                     # 执行migrate命令前,自动触发
post_migrate                    # 执行migrate命令后,自动触发 
```

### 3.Request/response_signals

```
request_started                 # 请求到来前,自动触发
request_finished                # 请求结束后,自动触发
got_request_exception           # 请求异常时,自动触发
```

### 4.Test_signals

```
setting_changed                 # 配置文件改变时,自动触发
template_rendered               # 模板执行渲染操作时,自动触发
```

### 5.Datebase_Wrapperd

```
connection_created              # 创建数据库连接时,自动触发
```

##  一、监听信号

要接收信号，请使用`Signal.connect()`方法注册一个接收器。当信号发送后，会调用这个接收器。

方法原型：

```python
Signal.connect(receiver, sender=None, weak=True, dispatch_uid=None)
```

参数：

```
receiver ：当前信号连接的回调函数，也就是处理信号的函数。 
sender ：指定从哪个发送方接收信号。 
weak ： 是否弱引用
dispatch_uid ：信号接收器的唯一标识符，以防信号多次发送。
```

下面以如何接收每次HTTP请求结束后发送的信号为例，连接到Django内置的`request_finished`信号。

### 1. 编写接收器

接收器其实就是一个Python函数或者方法：

```python
def my_callback(sender, **kwargs):
    print("Request finished!")
```

请注意，所有的接收器都必须接收一个sender参数和一个`**kwargs`通配符参数。

### 2. 连接信号

其实就是监听信号。有两种方法可以连接信号，一种是下面的手动方式：

```python
from django.core.signals import request_finished

request_finished.connect(my_callback)
```

另一种是使用receiver()装饰器：

```python
from django.core.signals import request_finished
from django.dispatch import receiver

@receiver(request_finished)
def my_callback(sender, **kwargs):
    print("Request finished!")
```

### 3. 接收特定发送者的信号

一个信号接收器，通常不需要接收所有的信号，只需要接收特定发送者发来的信号，所以需要在sender参数中，指定发送方。下面的例子，只接收MyModel模型的实例保存前的信号。

```python
from django.db.models.signals import pre_save   # 另外一个内置的常用信号
from django.dispatch import receiver
from myapp.models import MyModel


@receiver(pre_save, sender=MyModel)
def my_handler(sender, **kwargs):
    ...
```

`my_handler`函数只在MyModel实例保存时被调用。

### 4. 防止重复信号

为了防止重复信号，可以设置`dispatch_uid`参数来标识你的接收器，标识符通常是一个字符串，如下所示：

```python
from django.core.signals import request_finished

request_finished.connect(my_callback, dispatch_uid="my_unique_identifier")
```

最后的结果是，对于每个唯一的`dispatch_uid`值，你的接收器都只绑定到信号一次。

## 二、自定义信号

除了Django为我们提供的内置信号（比如前面列举的那些），很多时候，我们需要自己定义信号。

所有的信号都是`django.dispatch.Signal`的实例。

### 1. 创建信号

- 说明：pizza_done是信号名，providing_args=["toppings", "size"]，这个是触发者的时候需要传递的参数　　　　

```python
import django.dispath

pizza_done=django.dispath.Signal(providing_args=["toppings","size"])
```

### 2. 注册信号　　　　

```python
def callback(sender, **kwargs):  #创建注册信号的函数，这边的注册函数可以写多个
    print("callback")
    print(sender,kwargs)
  
pizza_done.connect(callback)  #注册信号
```

### 3. 发送信号　　

```python
from 路径 import pizza_done
pizza_done.send(sender='zhangqigao',toppings=123, size=456)
```

Django中有两种方法用于发送信号。

- Signal.send(sender, **kwargs)

- Signal.send_robust（sender，** kwargs）

必须提供sender参数（大部分情况下是一个类名），并且可以提供任意数量的其他关键字参数。

例如，这样来发送前面的`pizza_done`信号：

```python
class PizzaStore(object):
    ...

    def send_pizza(self, toppings, size):
        pizza_done.send(sender=self.__class__, toppings=toppings, size=size)
        ...
```

`send()`和`send_robust()`返回一个元组对的列表`[（receiver, response）， ... ]`，表示接收器和响应值二元元组的列表。

## 三、断开信号

```python
Signal.disconnect(receiver=None, sender=None, dispatch_uid=None)
```

`Signal.disconnect()`用来断开信号的接收器，和`Signal.connect()`中的参数相同。如果接收器成功断开，返回True，否则返回False。