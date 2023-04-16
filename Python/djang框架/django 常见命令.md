# django 常见命令

### 1.django的命令

**下载安装**

- pip install django==1.11.21 -i 源

**创建项目**

- django-admin startproject 项目名称

**启动**

- cd 项目的根目录下  manage.py

- python manage.py runserver   #  127.0.0.1:8000

- python manage.py runserver 80   #  127.0.0.1:80

- python manage.py runserver 0.0.0.0:80   #  0.0.0.0:80

**创建APP**

- python manag.py  startapp app名称

**数据库迁移的命令**

- python manage.py makemigrations   # 记录models.py的变更记录

- python manage.py migrate   # 迁移  将变更记录同步到数据库中

### 2.django的配置

**静态文件**

STATIC_URL = '/static/'    

STATICFILES_DIRS=[

​	os.path.join(BASE_DIR,‘static’)，

​	os.path.join(BASE_DIR,‘static1’)，

]



**数据库  DATABASES**

ENGINE  引擎 

NAME     数据库的名称

HOST   IP    '127.0.0.1'  

PORT   端口号   3306 

USER  用户名

PASSWORD 密码



**app注册**

INSTALLED_APPS

'app01'

'app01.apps.App01Config'



**中间件**  

注释  'django.middleware.csrf.CsrfViewMiddleware'    可以提交POST请求



**TEMPLATES** 

'DIRS': [os.path.join(BASE_DIR, 'templates')]

### 3.django使用MySQL数据库的流程

**1.创建一个MySQL数据库；**

**2.在settings中配置数据库；**

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'day53',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': 'root',
        'PASSWORD': '123'
    }
}
```

**3.让django使用pymysql连接MySQL数据库**

在与settings同级目录下的init 中写：

```python
import pymysql
pymysql.install_as_MySQLdb()
```

**4.创建表  在app下的models.py中写类（继承models.Model）**

```python
from django.db import models

class User(models.Model):   # app名称_user
    #单表创建字段
    data=models.DataField() #设置时间
    pid = models.AutoField(primary_key=True)  # 自动生成序号
    username = models.CharField(max_length=32)   # username varchar(32) 设置普通的字段
    password = models.CharField(max_length=32)   # password varchar(32)
    
    #一对多创建字段
    pub=models.Foreignkey('要关联的表名' ,on_delete= models.CASCASDE) #设置外键 on_delete级联删除on_delete  在django 2.0 版本之后是必填的参数    1.11  之前的可以不填 
    
    #多对多创建字段
    books=models.ManyToManyField('要关联的表') #多表关联,会自动生成第三张表,然后根据另外两张表的表明生成'表名_id'字段
    
```

**5.执行数据库迁移的命令**

python manage.py makemigrations   # 记录models.py的变更记录

python manage.py migrate   # 迁移  将变更记录同步到数据库中

### 4.get和post

**get  获取到一个资源**

发get的途径：

**1.在浏览器的地址栏种输入URL，回车**

**2.a标签**

**3.form表单   不指定 method    method=‘get’**

传递参数     url路径?id=1&name=alex

- django种获取url上的参数    request.GET   {'id':1,'name':'alex'}  
  - request.GET.get(key)     request.GET[key] 单选获取,返回字符串
  - request.GET.getlist(key)    多选框获取列表
  - F=request.FILES.get('f')  根据文件名获取上传文件对象
    - F.chunks() 获取文件

**post   提交数据.**

**form表单   method=‘post’**   

**参数不暴露在URL    在请求体中**  

**django中获取post的数据  request.POST** 

### 5.ORM 

**对应关系**

类     —— 》    数据表

对象   ——》  数据行（记录）

属性  ——》   字段

from app01 import models 

```python
#orm创建数据库表格


#一对多
mofels.Foreignkey('要关联的表名' ,on_delete=) 设置外键
'''
- on_delete 的参数
  - 默认是models.CASCASDE 级联删除
  - models.SET()  / models.SET_DEFAULT  / models.SET_NULL
- on_delete  在django2.0 版本之后是必填的参数    1.11  之前的	可以不填 
'''
#多对多
models.ManyToManyField('要关联的表')

- 多表关联,会自动生成第三张表,然后根据另外两张表的表明生成'表名_id'字段
```

- datafield
  - 在数据库中创建一个时间对象

**查：**

models.Publisher.objects.all()   ——》  查询所有的数据    queryset  对象列表  

models.Publisher.objects.get(name='xxxx')     ——》 对象      获取不到或者获取到多个就报错

models.Publisher.objects.filter(name='xxxx')   ——》 获取满足条件的所有的对象   queryset  对象列表   

- 一对多查询

  ```python
  all_books = models.Book.objects.all()
  
  for book in all_books:
      print(book.title)
      print(book.pub,type(book.pub))   #  ——> 所关联的出版社对象
      print(book.pub.pk)  #  查id 多一次查询
      print(book.pub_id)  # 直接在book表中查出的ID
      print(book.pub.name)
      print("*"*32)
  ```

- 多对多查询

  ```python
  for author in all_authors:
      print(author)
      print(author.pk)
      print(author.name)
      print(author.books,type(author.books)) # author.books是关系管理对象
      print(author.books.all(),type(author.books.all()))  #获取所有book相关的对象,返回的是一个对象列表
      print('*'*30)
  ```

**增：**

models.Publisher.objects.create(name='xxx')   ——》  新插入数据库的对象

obj = models.Publisher(name='xxx')  ——》 存在在内存中的对象

obj.save()      ——》   提交到数据库中  新增

**删：**

obj = models.Publisher.objects.get(pk=1)

obj.delete()    #单个对象删除

obj_list = models.Publisher.objects.filter(pk=1)

obj_list.delete()    #多个对象批量删除

**改：**

- obj = models.Publisher.objects.get(pk=1)

- obj.name  = 'new name'      ——》 在内存中修改对象的属性

- obj.save()      ——》   提交数据   保存到数据库中
- author_obj.books.set(books)  # 多对多关系时,每次直接重新设置

