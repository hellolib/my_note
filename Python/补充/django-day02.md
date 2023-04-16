### django-debug-toolbar

<https://www.cnblogs.com/maple-shaw/articles/7808910.html>

### 缓存

配置

```
# 内存
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
        'LOCATION': 'unique-snowflake',
        'TIMEOUT': 300,  # 缓存超时时间（默认300，None表示永不过期，0表示立即过期）
        'OPTIONS': {
            'MAX_ENTRIES': 100,  # 最大缓存个数（默认300）
            'CULL_FREQUENCY': 3,  # 缓存到达最大个数之后，剔除缓存个数的比例，即：1/CULL_FREQUENCY（默认3）
        },
    }
}
```

应用

缓存视图

```Python
from django.views.decorators.cache import cache_page


@cache_page(5)
def student_list(request, *args, **kwargs):
    students = models.Student.objects.all()
    print('students')
    return render(request, 'student_list.html', {'students': students})
```

全栈缓存

```
MIDDLEWARE = [
    'django.middleware.cache.UpdateCacheMiddleware',
		....
    'django.middleware.cache.FetchFromCacheMiddleware',
]
```

![1568689511987](asset/1568689511987.png)

局部缓存

```
{% load cache %}

{% cache 5 'xxx' %}

    缓存
    {{ now }}

{% endcache %}
```



缓存使用redis

<https://django-redis-chs.readthedocs.io/zh_CN/latest/>

下载

```
pip install django-redis
```

```
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

```
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

### 信号

内置的信号

```
from django.core.signals import request_finished
from django.core.signals import request_started
from django.core.signals import got_request_exception

from django.db.models.signals import class_prepared
from django.db.models.signals import pre_init, post_init
from django.db.models.signals import pre_save, post_save
from django.db.models.signals import pre_delete, post_delete
from django.db.models.signals import m2m_changed
from django.db.models.signals import pre_migrate, post_migrate

from django.test.signals import setting_changed
from django.test.signals import template_rendered

from django.db.backends.signals import connection_created
```

注册信号

```
from django.db.models.signals import pre_save, post_save


def callback(sender, **kwargs):
    print("xxoo_callback")
    print(sender, kwargs)


post_save.connect(callback)


from django.dispatch import receiver


@receiver(post_save)
def callback1(sender, **kwargs):
    print("xxoo_callback1111111")
    print(sender, kwargs)

```

### orm性能相关

1. 尽量不查对象,能用values()

2. select_related('classes')     连表查询    多对一   一对一

3. prefetch_related('classes')    子查询    多对一   多对多

4. only('name')   指定某些字段     defer  指定排除某些字段  

   queryset 特性

### 多个数据库

配置

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    },
    'db2': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db2.sqlite3'),
    },
}
```

迁移其他的数据库

python manage.py migrate --database db2

读写分离

手动

```
models.Student.objects.using('db2').all()

obj = models.Student.objects.using('db2').get(name='zhazha')
obj.name = 'star'
obj.save(using='default')
```

自动

settings配置:

```
DATABASE_ROUTERS = ['myrouter.Router']
```

```
class Router:
    """
    读写分离
    
    """

    def db_for_write(self, model, **kwargs):
        return 'db2'

    def db_for_read(self, model, **kwargs):
        return 'default'
```

一主多从

```
class Router:
    """
    一主多从
    """

    def db_for_write(self, model, **kwargs):
        return 'db1'

    def db_for_read(self, model, **kwargs):
        return random.choices['db2', 'db3', 'db4']
```

分库分表

```Python
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

### django执行原生SQL的方法

```Python
# 1. extra

ret = models.Student.objects.all().extra(where=['id > %s'], params=['1'], order_by=['-id'])
# print(ret)
# for i in ret:
#     print(i)


# 2. raw

ret = models.Student.objects.raw('select * from main.app01_classes where id <= 2')
print(ret)
for i in ret:
    print(i.name)

# 3 connection
from django.db import connection, connections

# cursor = connection.cursor()
cursor = connections['db2'].cursor()
cursor.execute("""SELECT * from main.app01_classes where id > %s""", [1])
row = cursor.fetchall()
print(row)
```

### crm+权限

crm

客户关系管理系统 

功能:

 - 销售
    - 客户的信息
      	- 公户和私户
   - 跟进记录的管理
   - 报名表的管理 
   - 缴费记录的管理
- 班主任
  - 班级管理
  - 课程记录的管理
  - 学生学习记录的管理

公户和私户

避免抢单的情况

有没有现成的CRM系统?  

<http://www.xuebangsoft.com/pro_boss.html>   学邦

自己开发  可定制化程度比较高

项目用了多少张表?

- crm  (10张)  
- 6张表(4个model)

权限

如何实现权限控制?

- 在web开发中,url地址当做权限  
- 表结构的设计
- 登录成功后保存用户的权限  + 中间件 校验用户的权限

技术点

- 登录成功后保存权限信息

  - 查询  跨表查询   去空   去重
  - session
    - json的序列化
    - 字典的key 是数字  序列化之后是字符串

-  中间件

  - url     request.path_info

  - request.current_men_id = None   # 当前展示的二级菜单id

  - request.breadcrumb_list = [  { url:'/index/','title':'首页'  } ]

  - 白名单

    - settings   
    - re  正则匹配

  - 登录状态的校验

  - 免认证的校验

  - 权限的校验

    - 从session中获取权限的信息
    - for 循环进行校验
      - 判断 有没有 pid
        - 有pid 当前权限是子权限
          - request.current_men_id = pid
          - request.breadcrumb_list.append({ p_url  p_title   })  #  从权限字典中找父权限信息
          - request.breadcrumb_list.append({ url  title   })
        - 没有pid 当前权限是父权限  二级菜单
          - request.current_men_id = id
          - request.breadcrumb_list.append({ url  title   })

    

  - 最后返回HttpResponse('没有权限')

- 动态生成菜单

  - inclusion_tag

  - 排序   有序字典   sorted 

  - ```
    m['class'] = 'active'   # 选中
    i['class'] = ''   # 一级菜单展开
    ```

- 路径导航
  
  - inclusion_tag
- 权限控制到按钮级别
  - filter
  - if    name   in  permision_dict

表结构

```Python
# 简单权限控制
class Permission(models.Model):
    url = models.CharField(max_length=64, verbose_name='权限')


class Role(models.Model):
    name = models.CharField(max_length=32, verbose_name='角色')
    permissions = models.ManyToManyField('Permission')


class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='用户名')
    pwd = models.CharField(max_length=32, verbose_name='密码')
    roles = models.ManyToManyField('Role')
    
    
    
# 生成一级菜单
class Permission(models.Model):
    url = models.CharField(max_length=64, verbose_name='权限')
    title = models.CharField(max_length=32, verbose_name='标题')
    icon = models.CharField(max_length=32, verbose_name='图标')
    is_menu = models.BooleanField(default=False, verbose_name='是否是菜单')


class Role(models.Model):
    name = models.CharField(max_length=32, verbose_name='角色')
    permissions = models.ManyToManyField('Permission')


class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='用户名')
    pwd = models.CharField(max_length=32, verbose_name='密码')
    roles = models.ManyToManyField('Role')

    
    
# 生成二级菜单
class Menu(models.Model):
    """一级菜单"""
    title = models.CharField(max_length=32, verbose_name='标题')
    icon = models.CharField(max_length=32, verbose_name='标题')
    weight = models.IntegerField(default=1)


class Permission(models.Model):
    url = models.CharField(max_length=64, verbose_name='权限')
    title = models.CharField(max_length=32, verbose_name='标题')
    menu = models.ForeignKey(Menu, null=True, blank=True)  # menu_id 


class Role(models.Model):
    name = models.CharField(max_length=32, verbose_name='角色')
    permissions = models.ManyToManyField('Permission')


class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='用户名')
    pwd = models.CharField(max_length=32, verbose_name='密码')
    roles = models.ManyToManyField('Role')

    
#  非菜单权限归属
class Menu(models.Model):
    """一级菜单"""
    title = models.CharField(max_length=32, verbose_name='标题')
    icon = models.CharField(max_length=32, verbose_name='标题')


class Permission(models.Model):
    """
    有parent_id  当前的权限就是子权限
    有menu_id     当前的权限就是父权限  二级菜单 
    """
    url = models.CharField(max_length=64, verbose_name='权限')
    title = models.CharField(max_length=32, verbose_name='标题')
    menu = models.ForeignKey(Menu, null=True, blank=True)  # menu_id
    parent = models.ForeignKey('self', null=True, blank=True)  # parent_id


class Role(models.Model):
    name = models.CharField(max_length=32, verbose_name='角色')
    permissions = models.ManyToManyField('Permission')


class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='用户名')
    pwd = models.CharField(max_length=32, verbose_name='密码')
    roles = models.ManyToManyField('Role')
    
    
#  权限控制到按钮级别
class Menu(models.Model):
    """一级菜单"""
    title = models.CharField(max_length=32, verbose_name='标题')
    icon = models.CharField(max_length=32, verbose_name='标题')


class Permission(models.Model):
    """
    有parent_id  当前的权限就是子权限
    有menu_id     当前的权限就是父权限  二级菜单
    """
    url = models.CharField(max_length=64, verbose_name='权限')
    title = models.CharField(max_length=32, verbose_name='标题')
    name = models.CharField(max_length=32, unique=True, verbose_name='URL的别名')
    menu = models.ForeignKey(Menu, null=True, blank=True)  # menu_id
    parent = models.ForeignKey('self', null=True, blank=True)  # parent_id


class Role(models.Model):
    name = models.CharField(max_length=32, verbose_name='角色')
    permissions = models.ManyToManyField('Permission')


class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='用户名')
    pwd = models.CharField(max_length=32, verbose_name='密码')
    roles = models.ManyToManyField('Role')

```

数据结构

```python
# 简单权限控制
permissions = [ {'permissions__url':'xxxxxx'} ,{'permissions__url':'xxxxxx'}   ]

# 生成一级菜单
permission_list = [ {'url':'xxxxx'}  ]
menu_list= [{'url','title' ,'icon'}]

# 生成二级菜单
permission_list = [ {'url':'xxxxx'}  ]
menu_dict ={
    一级菜单的id : {  
			title : 'xxx',
        	icon:'xxxx',
        	weight:1,
        	chilidern: [
                { url   title  }
			]
    	}	
}

# 非菜单权限归属
permission_list = [ {'url':'xxxxx','pid','id'}  ]
menu_dict ={
    一级菜单的id : {  
			title : 'xxx',
        	icon:'xxxx',
        	weight:1,
        	chilidern: [
                { url   title  id  }
			]
       }	
}


# 路径导航
permission_dict = { id :  {'url':'xxxxx','pid','id','title'}  }
menu_dict ={
    一级菜单的id : {  
			title : 'xxx',
        	icon:'xxxx',
        	weight:1,
        	chilidern: [
                { url   title  id  }
			]
       }	
}


# 权限控制到按钮级别
permission_dict = { name :{'url':'xxxxx','pid','id','title','p_name'}  }
menu_dict ={
    一级菜单的id : {  
			title : 'xxx',
        	icon:'xxxx',
        	weight:1,
        	chilidern: [
                { url   title  id  }
			]
       }	
}
```

用户的权限变更后,不重新登录,怎么应用最新的权限?

1. 用户表加字段  session_key

2. 查询用户的session数据  把最新的权限保存进去

```Python
from django.contrib.sessions.backends.db import SessionStore
from django.contrib.sessions.models import Session  # session表
ret = Session.objects.filter(session_key='ld5w252hf6b6pe72y2b0cmc9yh6kdyen').first()
print(ret.session_data)

ret = request.session.decode(ret.session_data)   # 解密数据
print(ret,type(ret))
ret = request.session.encode(ret)   # 加密数据
print(ret,type(ret))
```

权限控制到按钮级别,怎么控制到行级别?



你在开发中遇到什么印象深刻的问题?

- json序列化
- 表结构的变换
- 功能上的取舍

