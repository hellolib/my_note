# Django 数据库系统

## 一  常见数据库连接配置

### 1. mysql

- settings

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'day54',
          'HOST':'127.0.0.1',
          'PORT':3306,
          'USER':'root',
          'PASSWORD':'123'
      }
  }
  # ps:键必须都是大写
  ```

- init

  ```python
  方式1:在你的项目文件夹下面的__init__.py
  方式2:也可以在你的应用文件夹下面的__init__.py
  # 固定写法
  import pymysql
  pymysql.install_as_MySQLdb()  # 告诉django用pymysql代替mysqldb连接数据库
  ```

### 2. pg

- settings

  模块依赖`pip install psycopg2-binary==2.9.1`

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.postgresql_psycopg2',
          'NAME': 'webserver',
          'USER': 'webserver',
          'PASSWORD': 'Webserver123!',
          'HOST': '10.0.23.109',
          'PORT': 5432,
          'OPTIONS': {  # 该参数是为了指定你的 schema 
              'options': '-c search_path=scbd'
          }
      },
  }
  ```

### 3. sqllite

- settings 默认

  ```python
      'default': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
      },
  ```

  

## 二 多数据库配置

### 1. orm 性能优化

1. 尽量不查对象,能用values()

2. select_related('classes')     连表查询    多对一   一对一

3. prefetch_related('classes')    子查询    多对一   多对多

4. only('name')   指定某些字段     defer  指定排除某些字段  


### 2. 配置多个数据库连接

- settings 配置

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
      },
      'db2': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': os.path.join(BASE_DIR, 'db2.sqlite3'),
      },
      'db3': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': os.path.join(BASE_DIR, 'db3.sqlite3'),
      },
  }
  ```

- 迁移指定的数据库

  ```python
  python manage.py migrate --database db2
  ```

### 3. django 执行原生sql

- extra

  ```python
  #解释：结果集修改器，一种提供额外查询参数的机制
  #说明：依赖model模型
  #用在where后:
  Book.objects.filter(publisher_id="1").extra(where=["title='python学习1'"])　　　　
  #用在select后　　
  Book.objects.filter(publisher_id="1").extra(select={"count":"select count(*) from hello_book"})
  ```

  ```python
  ret = models.Student.objects.all().extra(where=['id > %s'], params=['1'], order_by=['-id'])
  # print(ret)
  # for i in ret:
  #     print(i)
  ```

  

- row

  ```python
  ret = models.Student.objects.raw('select * from main.app01_classes where id <= 2')
  print(ret)
  for i in ret:
      print(i.name)
  ```

- connection

  ```python
  
  from django.db import connection, connections
  
  # cursor = connection.cursor()
  cursor = connections['db2'].cursor()
  cursor.execute("""SELECT * from main.app01_classes where id > %s""", [1])
  row = cursor.fetchall()
  print(row)
  ```

  ```python
  from django.db import connection
  cursor = connection.cursor()
  #插入
  cursor.execute("insert into hello_author(name) values('xiaol')")
  #更新
  cursor.execute("update hello_author set name='xiaol' where id=1")
  #删除
  cursor.execute("delete from hello_author where name='xiaol'")
  #查询
  cursor.execute("select * from hello_author")
  #返回一行
  raw = cursor.fetchone()
  print(raw)
  # #返回所有
  # cursor.fetchall()
  ```

  

## 三 读写分离

### 1. 手动

```python
models.Student.objects.using('db2').all()

obj = models.Student.objects.using('db2').get(name='zhazha')
obj.name = 'star'
obj.save(using='default')
```

### 2. 自动

- settings配置:

  ```python
  DATABASE_ROUTERS = ['myrouter.Router']
  ```

- 自定义Router类

  ```python
  class Router:
      """
      读写分离
      """
  
      def db_for_write(self, model, **kwargs):
          return 'db2'
  
      def db_for_read(self, model, **kwargs):
          return 'default'
  ```

- 一主多从

  ```python
  class Router:
      """
      一主多从
      """
  
      def db_for_write(self, model, **kwargs):
          return 'db1'
  
      def db_for_read(self, model, **kwargs):
          return random.choices['db2', 'db3', 'db4']
  ```

- 分库分表

  ```python
  class Router:
      """
      分库分表
  
      app01  model   db1
      app02  model   db2
      """
  
      def db_for_write(self, model, **kwargs):
          app_name = model._meta.app_label
          if app_name == 'app01':
              return 'db1'
          elif app_name == 'app02':
              return 'db2'
  
      def db_for_read(self, model, **kwargs):
          app_name = model._meta.app_label
          if app_name == 'app01':
              return 'db1'
          elif app_name == 'app02':
              return 'db2'
  ```

  

  