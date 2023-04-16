## 1. CMDB实现思路

- auto_client  
  - 脚本采集数据
- server_api 
  - Djnago + DRF (提供api)

### 1.1 agent 方式

1. 每台服务器装一个agent
2. 每天定时启动这个脚本
3. 采集完成信息后发送一个机器 保存信息

- 代码结构
  - agent
  - api

### 1.2 ssh 方式

1. 中控机上装agent脚本
  2. 每天定时启动，自动采集资产
  3. 通过ssh远程（paramiko）连接上主机，在被控机上执行命令，获取到硬件的信息
  4. 对数据进行处理，通过requests模块发送到api
  5. api接受数据保存到api

### 1.3 salt方式

1. 中控机上装agent脚本(中控机装salt-master,被控机装salt-minion)

  2. 每天定时启动，自动采集资产
  3. 通过saltstack远程连接上主机，在被控机上执行命令，获取到硬件的信息
  4. 对数据进行处理，通过requests模块发送到api
  5. api接受数据保存到api

- 实现手段: 通过saltstack实现主从控制

- saltstack 安装配置

  - salt IP

  ```shell
  # 1.安装yum源 在主从机器都要执行命令
  sudo yum install https://repo.saltstack.com/py3/redhat/salt-py3-repo-latest.el7.noarch.rpm -y
  # 2.控制端
  ## 2.1 安装
  yum install salt-master -y
  ## 2.2 配置
  vi  /etc/salt/master
  interface: 0.0.0.0
  ## 2.3 启动
  systemctl start salt-master
  # 3.被控制端
  ## 3.1 安装
  yum install salt-minion -y
  ## 3.2 配置
  vi  /etc/salt/minion
  ## 3.3 启动
  master: 10.0.0.128
  systemctl start salt-minion
  # 4.控制端授权
  salt-key -L  查看所有授权
  salt-key -A  接受所有的授权
  
  # 5.如果出现 没有ip或主机名的时候关闭防火墙
  systemctl stop firewalld
  systemctl disable firewalld
  
  # 6.执行命令
  salt '10.0.0.129' cmd.run 'ls'
  salt '*' cmd.run 'ls'
  ```

  - salt hostname

  ```python
  #1. 被控制端设置主机名
  #2. master 删除key   停止
  salt-key -d  ip 
  systemctl stop salt-master
  #3. minion 删除 minion_id 重启
  rm -rf /etc/salt/minion_id 
  systemctl restart salt-minion
  #4. master 启动 接受key
  systemctl start salt-master
  salt-key -a  key   
  salt-key -A 
  ```

- 其他插曲

```python
import traceback

def disk():
    int('saaa')

def run():
    try:
        disk()
    except Exception:
        ret = traceback.format_exc()
        print(ret)  // 输出完整的错误信息
        print(type(ret))  // str
run()
```



## 2. auto_client 目录结构

- bin    可执行文件
- log    日志
- conf  配置 
- lib  公共库  
- src  逻辑的代码

## 3.硬件信息的唯一标识

- 方案一：
  1. 物理机 :sn号

  2. 虚拟机 :通过虚拟技术的接口

- 方案二(本项目采用)：根据主机名判断机器设备唯一
  - 流程：

    - 在cert文件中保存上主机名

    - 新的机器: 没有文件 ->告诉API 新增的操作->  响应正常->  保存主机名到cert文件中

    - 老的机器: 读取文件->拿到老的主机名->  使用老主机名和新采集的主机名对比

      ```python
      相等    没有修改主机名   告知api 更新资产信息
      不相等  修改l主机名   告知api 更新资产信息  + 更新主机名
      ```

      


## --. 项目知识点 

### --.1 接口类的实现

- **类的约束**(推荐使用)

  ```python
  import abc
  
  class Person(metaclass=abc.ABCMeta):
      @abc.abstractmethod
      def talk(self):
          print('xx')
  
  class China(Person):
      def talk(self):
          print(124)
      # pass
  
  p = China()  # 类China中必须有talk方法,不然会报错
  ```

- **继承  + 抛出异常**(实现python的约束)

  ```python
  class Person():
  
      def talk(self):
          raise NotImplementedError('talk()  must be Implemented')
  
  class Chinese(Person):
      pass
      # def talk(self):
      #     print('中国话')
  
  p = Chinese()  # 类China中必须有talk方法,不然父类的talk就会主动抛出异常
  p.talk()  # 可以实例化但是不能调用该方法
  ```

- 添加根目录至path

  ```python
  ret = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  sys.path.insert(0, ret)  # 添加项目根目录至path目录,不然程序只能在项目中运行
  
  os.environ['USER_SETTINGS'] = 'conf.settings'  # 一个环境变量.当作字典使用,值是字符串,根据反射取操作包
  ```

- importlib 将一个文件封装为module

  ```python
  import importlib
  settings_module = importlib.import_module(path)
  #settings_module就是自制的包,通过.方法可以取属性或者方法
  ```

  

### --.2 反射的应用

- 通过反射来实现不同插件插槽功能,避免了大量的if else使代码更简洁

  ```python
  import importlib
  
  def get_class(class_path_str):
      """
      获取导入的字符串
      :return:
      """
      module_str, cls_str = class_path_str.rsplit('.', maxsplit=1)  # 切割出路径和类名字符串
      module = importlib.import_module(module_str)  # 封装module
      cls = getattr(module, cls_str)  # 获取类
      return cls
  ```

  

- 类似Django的setting文件(通过单例模式使用一个类整理系统设置和用户个人设置)

  - python的`__init__`文件自动形成单例模式

  ```python
  # 在init文件中定义 配置
  import os
  import importlib
  from . import globle_settings
  
  
  class LazySettings(object):
      def __init__(self):
          # 默认配置
          for j in dir(globle_settings):
              if j.isupper():
                  gv = getattr(globle_settings, j)
                  setattr(self, j, gv)
          # 用户自定义配置
          path = os.environ.get('USER_SETTINGS')
          settings_module = importlib.import_module(path)  # 封装模块(导入用户的自定义配置)
          for i in dir(settings_module):  # 查看对象所有的属性
              if i.isupper():  # 判断大写
                  uv = getattr(settings_module, i)
                  setattr(self, i, uv)  # 给本对象设值
  
  
  settings = LazySettings()
  ```

### --.3 线程池

- 线程池执行任务,提高检测速度

  ```python
  import time
  from concurrent.futures import ThreadPoolExecutor
  pool = ThreadPoolExecutor(100)
  def func(num, qw):
      print(num, qw)
      time.sleep(1)
  for i in range(100):
      pool.submit(func, 1, 2)
  ```

### --.4 使用DRF

- CBV + APIView
- APIVIew
  - Respones 可以序列化列表,变成一json串

### --.5 类方法`__dict__`

- 使用类方法`__dict__`方法,把一个对象的属性列为字典

### --.6 logging模块封装

```python
import logging

class Logger(object):
    """自己封装logging模块"""
    def __init__(self, name, log_file_path, log_level=logging.DEBUG):
        """
        :param name: 名字(拼接在日志文件的内容里)
        :param log_file_path: 文件路径
        :param log_level: 日志等级
        """
        file_handler = logging.FileHandler(log_file_path, 'a', encoding='utf-8')
        fmt = logging.Formatter(fmt="%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s")
        file_handler.setFormatter(fmt)

        self.logger = logging.Logger(name, level=log_level)
        self.logger.addHandler(file_handler)

    def debug(self, msg):
        """debug"""
        self.logger.debug(msg)

    def error(self, msg):
        """error"""
        self.logger.error(msg)

    def info(self, msg):
        """info"""
        self.logger.info(msg)


logger = Logger('test', './log.log')

```

