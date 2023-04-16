# rabbitmq消息队列

## 1.rabbitmq的安装使用

```python

1.yum安装即可

yum  install erlang  rabbitmq-server   -y 


2.启动rabbitmq服务端 

systemctl start rabbitmq-server  
	

3.配置rabbitmq,创建管理用户以及后台管理页面

sudo rabbitmqctl add_user pengpeng   123  

4.给新用户,设置管理员角色
sudo rabbitmqctl set_user_tags pengpeng   administrator  


5.给与这个用户,对所有的队列,可读可写
语法:set_permissions [-p <vhostpath>] <user> <conf> <write> <read>

sudo rabbitmqctl set_permissions -p "/" pengpeng  ".*" ".*" ".*"


6.添加rabbtimq管理界面
rabbitmq-plugins enable rabbitmq_management

7.访问mq的管理界面
http://123.206.16.61:15672/
服务器id:15672  


8.登录rabbitmq服务端


9.练习rabbitmq的消息生产消费


生产者代码.py 
	#!/usr/bin/env python
	import pika
	# 创建凭证，使用rabbitmq用户密码登录
	# 去邮局取邮件，必须得验证身份
	credentials = pika.PlainCredentials("pengpeng","123")
	# 新建连接，这里localhost可以更换为服务器ip
	# 找到这个邮局，等于连接上服务器
	connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
	# 创建频道
	# 建造一个大邮箱，隶属于这家邮局的邮箱，就是个连接
	channel = connection.channel()
	# 声明一个队列，用于接收消息，队列名字叫“水许传”
	channel.queue_declare(queue='水续传')
	# 注意在rabbitmq中，消息想要发送给队列，必须经过交换(exchange)，初学可以使用空字符串交换(exchange='')，它允许我们精确的指定发送给哪个队列(routing_key=''),参数body值发>送的数据
	channel.basic_publish(exchange='',
						  routing_key='水续传',
						  body='大郎 起来喝药了')
	print("已经发送了消息")
	# 程序退出前，确保刷新网络缓冲以及消息发送给rabbitmq，需要关闭本次连接
	connection.close()
~                     

消费者.py 

	import pika
	# 建立与rabbitmq的连接
	credentials = pika.PlainCredentials("pengpeng","123")
	connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
	channel = connection.channel()


	channel.queue_declare(queue="水续传")

	def callbak(ch,method,properties,body):
		print("消费者接收到了任务：%r"%body.decode("utf8"))
		
		
	# 有消息来临，立即执行callbak，没有消息则夯住，等待消息
	# 老百姓开始去邮箱取邮件啦，队列名字是水许传
	channel.basic_consume(callbak,queue="水续传",no_ack=True)


	# 开始消费，接收消息
	channel.start_consuming()  

10.单生产者,单消费者

执行服务端代码,生成队列,写入消息
执行消费者代码,从队列中取走消息 


11.单生产者,多消费者 ,默认是轮询消费机制

12.消息丢列之确认机制,保证消息正确被处理   rabbitmq的ack确认机制
 
生产者代码不变 
	#!/usr/bin/env python
	import pika
	# 创建凭证，使用rabbitmq用户密码登录
	# 去邮局取邮件，必须得验证身份
	credentials = pika.PlainCredentials("pengpeng","123")
	# 新建连接，这里localhost可以更换为服务器ip
	# 找到这个邮局，等于连接上服务器
	connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
	# 创建频道
	# 建造一个大邮箱，隶属于这家邮局的邮箱，就是个连接
	channel = connection.channel()

	# 新建一个hello队列，用于接收消息
	# 这个邮箱可以收发各个班级的邮件，通过
	channel.queue_declare(queue='西游记')
	# 注意在rabbitmq中，消息想要发送给队列，必须经过交换(exchange)，初学可以使用空字符串交换(exchange='')，它允许我们精确的指定发送给哪个队列(routing_key=''),参数body值发送的数据
	channel.basic_publish(exchange='',
						  routing_key='西游记',
						  body='大师兄,师傅被妖怪抓走了')
	print("已经发送了消息")
	# 程序退出前，确保刷新网络缓冲以及消息发送给rabbitmq，需要关闭本次连接
	connection.close()

消费者代码如下.py  

	import pika

	credentials = pika.PlainCredentials("pengpeng","123")
	connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
	channel = connection.channel()

	# 声明一个队列(创建一个队列)
	channel.queue_declare(queue='西游记')

	def callback(ch, method, properties, body):
		print("消费者接受到了任务: %r" % body.decode("utf-8"))
		int('asdfasdf')
		# 我告诉rabbitmq服务端，我已经取走了消息
		# 回复方式在这,告诉服务端,我正确消费了消息,你可以标记清除了
		ch.basic_ack(delivery_tag=method.delivery_tag)
		
		
	# 关闭no_ack，代表给与服务端ack回复，确认给与回复
	channel.basic_consume(callback,queue='西游记',no_ack=False)

	channel.start_consuming()

```

## 2.消息和队列持久化 

```python
生产者代码.py 

	import pika
	# 无密码
	# connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61'))
	# 有密码

	credentials = pika.PlainCredentials("pengpeng","123")
	connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
	channel = connection.channel()

	# 声明一个队列(创建一个队列)
	# 默认此队列不支持持久化，如果服务挂掉，数据丢失
	# durable=True 开启持久化，必须新开启一个队列，原本的队列已经不支持持久化了
	'''
	实现rabbitmq持久化条件
	 delivery_mode=2
	使用durable=True声明queue是持久化
	 
	'''
	channel.queue_declare(queue='python',durable=True)  #这里实现队列创建的时候,就是持久化的

	channel.basic_publish(exchange='',
						  routing_key='python', # 消息队列名称
						  body='life is short,i use python ',
						  # 支持数据持久化
						  properties=pika.BasicProperties(
							  delivery_mode=2,#代表消息是持久的  2
						  )
						  )
	connection.close()

消费者代码.py 

import pika
credentials = pika.PlainCredentials("pengpeng","123")
connection = pika.BlockingConnection(pika.ConnectionParameters('123.206.16.61',credentials=credentials))
channel = connection.channel()
# 确保队列持久化
channel.queue_declare(queue='python',durable=True)

'''
必须确保给与服务端消息回复，代表我已经消费了数据，否则数据一直持久化，不会消失
'''
def callback(ch, method, properties, body):
	print("成功取出了消息 >>: %r" % body.decode("utf-8"))
	# 模拟代码报错
	# int('asdfasdf')    # 此处报错，没有给予回复，保证客户端挂掉，数据不丢失
   
	# 告诉服务端，我已经取走了数据，否则数据一直存在
	ch.basic_ack(delivery_tag=method.delivery_tag)


	
# 关闭no_ack，代表给与回复确认
channel.basic_consume(callback,queue='python',no_ack=False)
channel.start_consuming()

```

