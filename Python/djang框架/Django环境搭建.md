# Django环境搭建

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

python  manage.py runserver  0.0.0.0:80  # 0.0.0.0:80

- pycharm

点绿色三角

配置  ip  端口

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

#### 5.1.创建app

- 命令行

python manage.py startapp  app名称

- pycharm工具

tools ——》 run manage.py task ——》 输入命令  

#### 5.2.注册app

```
INSTALLED_APPS = [
	... 
	 # 'app01',
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
    url(r'^login/', views.login),
    url(r'^orm_test/', views.orm_test),
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

## 8.form表单

- form标签的属性  action=‘’  提交数据的地址  method='post'   提交方式

- 所有的input标签要有name属性   有的标签还需要定义value

- 要有input  type=submit  或者 button按钮

## 9.get和post请求的区别

get    获取到一个页面

提交的数据暴露在URL上的

传递参数 <http://127.0.0.1:8000/index/?id=2&name=alex> 

获取数据  request.GET

post  提交数据

数据隐藏在请求体

获取数据  request.POST

## 10.django使用MySQL数据库的流程：

#### 10.1.创建一个MySQL数据库；

#### 10.2.在settings中配置数据库

​	ENGINE :  mysql

​	NAME  ： 数据库的名称

​	HOST  :  IP   ‘127.0.0.1’

​	PORT： 端口  3306

​	USER :  用户名  ’root'

​	PASSWORD:  '123123'

#### 10.3.使用pymsql的模块连接MySQL数据库

在与settings同级的目录下的init文件夹下写：

import pymysql

pymysql.install_as_MySQLdb()

#### 10.4.创建表 ——》 在app下的models.py 中写类：

```python
from django.db import models

class User(models.Model):
    username = models.CharField(max_length=32)   # username varchar(32)
    password = models.CharField(max_length=32)   # username varchar(32)
```

#### 10.5.执行数据库迁移的命令

python manage.py makemigrations  # 记录下models.py文件的变更记录

python manage.py migrate  # 同步models.py的变更记录

## 11.ORM

对应关系

类     ——》   表

对象  ——》  数据行（记录）

属性  ——》  字段

ORM能做什么操作：

1.操作表

2.操作数据行

ORM具体的操作：

```python
from app01 import models 
# 查找所有的数据
models.User.objects.all()  # ——》  queryset  对象列表
# 查找一条数据
obj = models.User.objects.get(name='alex')  # 没有或者多个 就报错
obj.name   ——》 'alex'

# 查询满足条件的所有数据
models.User.objects.filter(name='alex')  # queryset  对象列表
```

