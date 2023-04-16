# Django基础环境搭建

## 1.下载安装

- 命令行

pip install django==1.11.21

pip install django==1.11.21 -i 源

- pycharm

setting ——》 解释器 ——》 点+号 ——》 输入Django ——》 选择版本 ——》 下载安装

## 2.创建项目

- 命令行

切换一个存放项目的目录下

django-admin startproject 项目名

- pycharm

file  ——》 new project ——》 左侧选择django ——》输入django项目的路径 ——》 选择解释器 ——》 输入一个app名称  ——》 创建

## 3.启动

- 命令行

cd到项目的根目录下 manage.py

python  manage.py runserver   # 127.0.0.1:8000

python  manage.py runserver  80  # 127.0.0.1:80

python  manage.py runserver  0.0.0.0:80  # 0.0.0.0:80  局域网访问,如果在pycharm中运行,需要讲settings中ALLOWED_HOSTS = ['*']

- pycharm

点绿色三角

配置  ip  端口

```python
右键运行manage.py,然后修改manage.py配置,在parametes中输入   0.0.0.0:80, #局域网访问
```

## 4.settings配置

数据库

静态文件

STATIC_URL ='/static/'

STATICFILES_DIRS = [

​	os.path.join(BASE_DIR, 'static1'),
​	os.path.join(BASE_DIR, 'static'),
​	os.path.join(BASE_DIR, 'static2'),

]

TEMPLATES   模板

```
'DIRS': [os.path.join(BASE_DIR, 'templates')]
```

MIDDLEWARE  中间件

```
'django.middleware.csrf.CsrfViewMiddleware'    注释掉之后就可以提交POST请求(绕过cs验证)
```

## 5.app

5.1.创建app

- 命令行

python manage.py startapp  app名称

- pycharm工具

tools ——》 run manage.py task ——》 输入命令  

5.2.注册app

```
INSTALLED_APPS = [
    'app01.apps.App01Config',  # 推荐写法

]
```

## 6.urls.py

写urll路径和函数的对应关系

```python
from django.conf.urls import url
from app01 import views

urlpatterns = [
    url(r'^index/', views.index),
]
```

## 7.views.py

写函数

```python
def login(request):
    
request.method   ——》 请求方式 GET  POST 
request.POST     ——》 form表单提交POST请求的数据  {}  request.POST['xxx'] request.POST.get('xxx',)

返回值
from django.shortcuts import HttpResponse, render, redirect
HttpResponse   —— 》 字符串 
render(request,'模板的文件名')  ——》 返回一个HTML页面 
redirect('重定向的地址')   ——》 重定向  /  响应头  Location：‘地址’  
```

## 8.get和post请求的区别

- get    获取到一个页面
  - 提交的数据暴露在URL上的
  - 传递参数 <http://127.0.0.1:8000/index/?id=2&name=alex> 

  - 获取数据  request.GET

- post  提交数据

  - 数据隐藏在请求体
  - 获取数据  request.POST

## 9.django使用MySQL数据库的流程：

9.1.创建一个**MySQL**数据库；

9.2.在settings中配置数据库

​	ENGINE :  mysql

​	NAME  ： 数据库的名称

​	HOST  :  IP   ‘127.0.0.1’

​	PORT： 端口  3306

​	USER :  用户名  ’root'

​	PASSWORD:  '123123'

9.3.使用pymsql的模块连接MySQL数据库

在与settings同级的目录下的init文件夹下写：

import pymysql

pymysql.install_as_MySQLdb()

9.4.创建表 ——》 在app下的models.py 中写类：

9.5.执行数据库迁移的命令

python manage.py makemigrations  # 记录下models.py文件的变更记录

python manage.py migrate  # 同步models.py的变更记录







# Django开发环境配置(pycharm)

**1.打开pycharm**

**2.虚拟环境配置**

- settings-->Project Interpreter-->Add-->Virtualenv Environment -->

  New environment 

  - 选择路径  D:\my_project\oldboy\env
  - 选择解释器 解释器名字需要时python.exe
  - 选择make available to all projects

**3.给虚拟环境添加package  --Django 1.11.22**

**4.新建Django项目**

- New project -->Django项目
-  选择项目路径 Location -->选择  existing interpreter  --> 选择新建的虚拟环境解释器 --> more settings (创建app名称:web  存储前端功能)

**5.更改项目settings中设置**

- 新建本地settings  --local_settings.py

  ```python
  import os
  
  # Build paths inside the project like this: os.path.join(BASE_DIR, ...)
  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  
  DEBUG = True
  
  ALLOWED_HOSTS = []
  
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': os.path.join(BASE_DIR, 'local_db.sqlite3'),
      }
  }
  ```

- 修改项目settings文件

  ```python
  DEBUG = False
  
  ALLOWED_HOSTS = ['*']
  
  try:
      from .local_settings import *
  except ImportError:
      pass
  ```

**6.git代码管理**

- git init 初始化代码仓库

- 添加忽略文件

  - github.com-->搜索gitignore-->control+f 搜索python-->Python.gitignore, 添加.gitignore文件到项目文件夹

  - 修改Python.gitignore文件

    ```python
    # 添加
    .idea/
    local_db.sqlite3
    ```

- git status 检查变更
- git add . 添加变化
- git commit -m '初始化项目'
- 创建远程仓库,线上托管代码
  - 创建仓库,名称
  - 选择开源和私有

- git config --global user.name "刘赛赛"
- git config --global user.email "17660626526@163.com"
- git remote add origin https://gitee.com/big_ox/oldboyedu.git
- git push -u origin master

**7.创建后台管理app**

- python manage.py startapp backend
- 注册app

**8.创建一个专门写models的app**

- python manage.py startapp repository     # *repository知识库*
- 注册app

**9.在项目中创建static文件夹**

- 配置settings

  ```python
  STATIC_URL = '/static/'
  STATICFILES_DIRS = [
      os.path.join(BASE_DIR, 'static')
  ]
  ```

  

