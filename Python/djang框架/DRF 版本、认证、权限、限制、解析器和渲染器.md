[TOC]



# 一.DRF之版本控制

## 1.为什么要有版本控制?

API版本控制允许我们在不同的客户端之间更改行为(同一个接口的不同版本会返回不同的数据). DRF提供了许多不同的版本控制方案.

可能会有一些客户端因为某些原因不再维护了, 但是我们后端的接口还要不断的更新迭代, 这个时候通过版本控制返回不同的内容就是一种不错的解决方案.

## 2.DRF提供的版本控制方案

DRF提供了五种版本控制方案, 如下:

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214223577-449251934.png)

## 3.版本的使用

### 3.1全局配置

1. `settings.py`文件中进行全局配置

除非明确设置, 否则`DEFAULT_VERSIONING_CLASS`值为None, 此例中的`request.version`将会始终返回None.

```python
REST_FRAMEWORK = {
    # 配置默认使用的版本控制方案: URLPathVersioning
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning',
    'DEFAULT_VERSION': 'v1',  # 默认的版本
    'ALLOWED_VERSIONS': ['v1', 'v2'],  # 有效的版本
    'VERSION_PARAM': 'version',  # 版本的参数名与URL conf中一致
}
```

1. `urls.py`文件中:

```python
from django.conf.urls import url
from django.contrib import admin
from bms import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),

    url(r'^(?P<version>[v1|v2]+)/book/$',   # 版本的参数名与URL conf中一致
        views.BookViewSet.as_view(actions={'get': 'list', 'post': 'create'})),
    url(r'^(?P<version>[v1|v2]+)/book/(?P<pk>\d+)$',
        views.BookViewSet.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),

]
```

1. `bms/views.py`文件中:

我们在中可以通过访问`request.version`来获取当前请求的具体版本, 然后根据不同的版本来返回不同的内容.

- 只要在`settings.py`中配置了版本信息, 在视图(`bms/views.py`)中就能通过`request.version`获取当前版本
- `get_serilaizer_class`方法可以根据不同版本返回不同的序列化类
- `get_queryset`方法可以根据不同的版本返回不同的数据控制

思考: 为什么可以直接用`request.version`拿到版本号? --> 这就是我们看源码的目的

```
from bms import models
from bms.modelserializers import BookModelSerializer
from rest_framework.viewsets import ModelViewSet


class BookViewSet(ModelViewSet):
    queryset = models.Book.objects.all()
    serializer_class = BookModelSerializer

    def get_serializer_class(self):
        """不同版本 使用不同的序列化类"""
        if self.request.version == 'v1':
            return BookModelSerializer1
        return self.serializer_class

    def get_queryset(self):
        """不同的版本可以 返回不同的数据控制"""
        if self.request.version == 'v1':
            return models.Book.objects.all()[:3]
        return self.queryset.all()
```

### 3.2局部配置(使用较少)

我们可以在一个单独的视图上设置版本控制方案. 通常, 我们==不需要这样做==, 因为在全局范围内使用一个版本控制方案更有意义. 如果我们确实需要这样做, 请使用`versioning_class`属性.

1. 导入版本控制方案:`from rest_framework.versioning import 版本控制方案`
2. `versioning_class=版本控制方案`
3. 定义 `get_queryset`方法 或 `get_serializer_class`方法

==注意==: 版本控制方案有五种

```
# AcceptHeaderVersioning
# -- 将版本信息放在请求头中

URLPathVersioning
# -- 将版本信息放在URL中,如: 127.0.0.1:8000/v1/book

NamespaceVersioning
# -- 通过namespace来区分版本

HostNameVersioning
# -- 通过主机名来区分版本

QueryParameterVersioning
# -- 通过URL查询参数来区分版本 如: 127.0.0.1:8000/authors/?version=1
```

`my_app/views.py`文件中:

```
# 第一步
from rest_framework.versioning import URLPathVersioning
class AuthorViewSet(ModelViewSet):
    queryset = models.Author.objects.all()
    serializer_class = AuthorModelSerializer
    
    # 第二步
    versioning_class = URLPathVersioning

    # 第三步
    def get_queryset(self):
        """不同的版本可以 返回不同的数据控制量"""
        pass
    
    # 第三步
    def get_serializer_class(self):
        """不同版本 使用不同的序列化类"""
        pass
```

`urls.py`文件中:

```
from django.conf.urls import url
from bms import views
urlpatterns = [
    url(r'^(?P<version>[v1|v2]+)/authors/$',
        views.AuthorViewSet.as_view(actions={'get': 'list', 'post': 'create'})),
    url(r'^(?P<version>[v1|v2]+)/authors/(?P<pk>\d+)$',
        views.AuthorViewSet.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),
]
```

# 二.DRF之认证

身份验证是将==传入请求==与==一组标识凭据(如请求来自的用户或其签名的令牌)==相关联的机制. 然后 权限 和 限制 组件决定是否拒绝这个请求.

简单来说:

- 认证 -- 确定了你是谁
- 权限 -- 确定你能不能访问某个接口
- 限制 -- 确定你访问某个接口的频率

**认证的目的**: 告诉服务端你是谁

**思考**: 有个问题, 我们的Django和Vue项目是分离的, 它们很可能是建立在两个不同的服务器上的, 这种前后端分离的情况我们该怎样存cookie和session呢? 对于这种情况, 我们一般是通过Vue发ajax请求来把数据保存到cookie/session中的. 还有一种解决办法, 当前端请求到来时, 前端发送过来一个==token==值给后端, 后端通过查询这个token值(数据库中匹配)就可以确定你是谁了.

**总结**: 我们通过token值来确定前端来访问的用户是谁.

## 1.内置的认证

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214250229-689042602.png)

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119215142560-1613801912.png)

## 2.步骤

1.新创建一个app: `auth_demo`

2.在`settings.py`中注册`auth_demo`这个app

3.`auth_demo/models.py`中的表结构设计:

```
from django.db import models

# 用户表
class UserInfo(models.Model):
    name = models.CharField(max_length=32)
    pwd = models.CharField(max_length=32)
    vip = models.BooleanField(default=False)
    token = models.CharField(max_length=128, null=True, blank=True)
```

4.二级路由

根目录下的`urls.py`:

```
from django.conf.urls import url, include

urlpatterns = [
    url(r'^users/', include('auth_demo.urls')),
]
```

`auth_demo/urls.py`:

```
from django.conf.urls import url
from auth_demo import views

urlpatterns = [
    url(r'reg/$', views.RegView.as_view()),  # 注册
    url(r'login/$', views.LoginView.as_view()),  # 登录
    url(r'test_auth/$', views.TestAuthView.as_view()),  # 测试登录认证
]
```

5.视图函数 `auth_demo/views.py`:

`注册`:

```
from rest_framework.views import APIView
from rest_framework.response import Response
from auth_demo import models


class RegView(APIView):
    """只支持注册用户"""

    def post(self, request):
        # 1.获取用户注册的数据
        name = request.data.get('name')
        pwd = request.data.get('pwd')
        re_pwd = request.data.get('re_pwd')
        if name and pwd:
            # 2.判断密码和确认密码是否一致
            if pwd == re_pwd:
                # 3.创建用户
                models.UserInfo.objects.create(name=name, pwd=pwd)
                # 4.返回响应
                return Response('注册成功')
            else:
                return Response('两次密码不一致')
        else:
            return Response('无效的参数')
```

`登录`:

```
class LoginView(APIView):
    """只支持用户登录"""

    def post(self, request):
        # 1.通过request.data获取前端提交的数据
        name = request.data.get('name')
        pwd = request.data.get('pwd')
        if name and pwd:
            # 2.从数据库中进行筛选匹配
            user_obj = models.UserInfo.objects.filter(name=name, pwd=pwd).first()
            if user_obj:
                # 3.登录成功,生成token(时间戳 + Mac地址)
                import uuid
                token = uuid.uuid1().hex
                # 4.把token保存到用户表中
                user_obj.token = token
                user_obj.save()
                # 5.返回响应(包括状态码和token值)
                return Response({'error_no': 0, 'token': token})
            else:
                # 3.用户名或密码错误,登录失败
                return Response({'error_no': 1, 'error': '用户名或密码错误'})
        else:
            return Response('无效的参数')
```

6.自定义认证类 `auth_demo/auth.py`:

```
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed
from auth_demo import models


class MyAuth(BaseAuthentication):

    def authenticate(self, request):
        # 1.通过request.query_params获取前端的url参数
        token = request.query_params.get('token')
        if token:
            # 2.如果请求的url中携带了token参数
            user_obj = models.UserInfo.objects.filter(token=token).filter()
            if user_obj:
                # 3.token是有效的
                return user_obj, token  # 必须返回一个元组: (user_obj, token) --> (request.user, request.auth)
            else:
                raise AuthenticationFailed('无效的token')
        else:
            raise AuthenticationFailed('请求的URL中必须携带token参数')
```

7.局部认证 配置: `auth_demo/views.py`

注意: ==局部配置的优先级高于全局配置==

```
from auth_demo.auth import MyAuth

# 登录之后才能看到的数据接口
class TestAuthView(APIView):
    authentication_classes = [MyAuth, ]  # 配置局部认证, 全局认证在settings.py文件中配置

    def get(self, request):
        print(request.user.name)    # request.user 用户对象
        print(request.auth)         # reequest.auth 设置的token值
        return Response('这个视图里面的数据只有登录以后才能看到!')
```

8.全局配置: `settings.py`

```
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': ['auth_demo.auth.MyAuth', ],  # 认证 全局配置
}
```

# 三.DRF之权限

## 1.自定义一个权限类

`auth_demo/permissions.py`:

```
"""
自定义一个权限组件
"""
from rest_framework.permissions import BasePermission
class MyPermission(BasePermission): # 继承BasePermission
    message = '只有VIP才能访问'

    def has_permission(self, request, view):    # 必须实现has_permission方法
        # 只有通过权限验证的用户才能访问has_permission方法
        if not request.auth:    # request.auth --> token值
            return False
        # request.user --> 当前通过token认证的用户(UserInfo表中的用户对象)
        if request.user.vip:
            # 是VIP就通过
            return True
        else:
            # 不是VIP就拒绝
            return False
```

## 2.权限 局部配置

`auth_demo/views.py`:

```
from auth_demo.auth import MyAuth
from auth_demo.permissions import MyPermission


# 登录之后才能看到的数据接口
class TestAuthView(APIView):
    authentication_classes = [MyAuth, ]  # 配置局部认证, 全局认证在settings.py文件中配置
    permission_classes = [MyPermission, ]   # 配置局部权限, 全局权限在settings.py文件中配置

    def get(self, request):
        print(request.user.name)
        print(request.auth)
        return Response('这个视图里面的数据只有登录以后才能看到!')
```

## 3.权限 全局配置

`settings.py`:

```
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': ['auth_demo.auth.MyAuth', ],
    'DEFAULT_PERMISSION_CLASSES': ['auth_demo.permissions.MyPermission', ]
}
```

# 四.DRF之限制

## 1.使用自定义限制类

DRF内置了基本的限制类，首先我们自己动手写一个限制类，熟悉下限制组件的执行过程。

### 1.1自定义一个限制类

`auth_demo/throttle.py`:

```
import time

# 访问记录
VISIT_RECORD = {}
class MyThrottle(object):

    def __init__(self):
        self.history = None

    def allow_request(self, request, view):
        # 1.拿到当前请求的ip作为VISIT_RECORD的key
        ip = request.META.get('REMOTE_ADDR')
        # 2.拿到当前请求的时间戳
        now = time.time()
        # 3.如果是请求是第一次来访问
        if ip not in VISIT_RECORD:
            # {ip:[]}
            VISIT_RECORD[ip] = []
            return True
        # 4.把当前请求的访问记录拿出来保存到一个变量(访问历史)中
        history = VISIT_RECORD[ip]
        self.history = history
        # 5.循环访问历史,把超过10秒钟的请求时间去掉
        while history and now - history[-1] > 10:
            history.pop()
        # 6.此时,history中只保存了最近10秒钟的访问记录
        if len(history) >= 3:
            # (1)history中存放了3条及以上的历史记录,拒绝访问
            return False
        else:
            # (2)history中的历史记录不到3条,存储当前历史记录
            self.history.insert(0, now)
            return True

    def wait(self):
        """告诉客户端还需要等待多久"""
        now = time.time()
        return self.history[-1] + 10 - now
```

### 1.2限制 局部配置

`auth_demo/views.py`:

```
from auth_demo.throttle import MyThrottle


# 登录之后才能看到的数据接口
class TestView(APIView):
    throttle_classes = [MyThrottle, ]  # 配置局部限制, 全局限制在settings.py文件中配置

    def get(self, request):
        return Response('你成功了!这个视图里面的数据只有登录以后才能看到!')
```

### 1.3限制 全局配置

`settings.py`:

```
# 在settings.py中设置rest framework相关配置项
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': ['auth_demo.auth.MyAuth', ],  # 认证 全局配置
    'DEFAULT_PERMISSION_CLASSES': ['auth_demo.permissions.MyPermission', ],  # 权限 全局配置
    'DEFAULT_THROTTLE_CLASSES': ['auth_demo.throttle.MyThrottle'],  # 限制 全局配置
}
```

## 2.使用内置限制类

### 2.1定义内置限制类

`auth_demo/throttle.py`:

```
#使用内置限制类

from rest_framework.throttling import SimpleRateThrottle
class VisitThrottle(SimpleRateThrottle):
    scope = "throttle"

    def get_cache_key(self, request, view):
        return self.get_ident(request)
```

### 2.2全局配置

```
REST_FRAMEWORK = {
    # 内置限制类的全局配置
    "DEFAULT_THROTTLE_RATES": {
        "throttle": "5/m",  # 这里的key要与throttle.py文件中的scope="throttle"相对应
    },
}
```

# 五.DRF之分页

## 1.为什么要使用分页

我们的数据表中可能会有成千上万条数据, 当我们访问某张表的所有数据时,我们不大可能需要一次把所有数据都展示出来, 因为数据量很大, 对服务端的内存压力比较大并且网络传输过程中耗时也会比较大.

通常我们会希望一部分一部分去请求数据, 也就是我们常说的一页一页获取数据并展示出来.

## 2.DRF使用分页器

### 2.1分页模式

REST framework中提供了三种分页模式:

```
from rest_framework.pagination import PageNumberPagination, LimitOffsetPagination, CursorPagination
```

### 2.2全局配置

```
REST_FRAMEWORK = {
    # 默认使用的分页类
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    # 默认每页的数据个数
    'PAGE_SIZE': 100
}
```

### 2.3局部配置

我们可以在视图类中进行局部配置

```
class PublisherViewSet(ModelViewSet):
    queryset = models.Publisher.objects.all()
    serializer_class = PublisherModelSerializer
    pagination_class = PageNumberPagination  # 注意不是列表（只能有一个分页模式）
```

## 3.DRF内置分页器

### 3.1`PageNumberPagination`

按页码数分页, 第n页, 每页显示m条数据.

例如: `http:127.0.0.1:8000/v1/book/?page=2&size=1`

#### 分页器

```
# bms/pagination.py

from rest_framework.pagination import PageNumberPagination
class MyPageNumber(PageNumberPagination):
    page_size = 2  # 每页显示多少条
    page_size_query_param = 'size'  # URL中每页显示条数的参数
    page_query_param = 'page'  # URL中页码的参数
    max_page_size = None  # 最大页码数限制
```

#### 视图

```
# bms/views.py

from bms.pagination import MyPageNumber
class BookViewSet(ModelViewSet):
    queryset = models.Book.objects.all().order_by('id')
    serializer_class = BookModelSerializer
    """
    普通分页器
    """
    pagination_class = MyPageNumber
```

### 3.2`LimitOffsetPagination`

分页, 在n位置, 向后查看m条数据.

例如: `127.0.0.1:8000/v1/book/?offset=2&limit=2`

#### 分页器

```
# bms/pagination.py

from rest_framework.pagination import LimitOffsetPagination
class MyLimitOffset(LimitOffsetPagination):
    default_limit = 1
    limit_query_param = 'limit'
    offset_query_param = 'offset'
    max_limit = 999
```

#### 视图

```
# bms/views.py

from bms.pagination import MyLimitOffset
class BookViewSet(ModelViewSet):
    queryset = models.Book.objects.all().order_by('id')
    serializer_class = BookModelSerializer
    """
    offset分页器
    """
    pagination_class = MyLimitOffset
```

### 3.3`CursorPagination`

加密分页, 把上一页和下一页的id值记住.

#### 分页器

```
# bms/pagination.py

from rest_framework.pagination import CursorPagination
class MyCursorPagination(CursorPagination):
    cursor_query_param = 'cursor'
    page_size = 1
    ordering = '-id'  # 重写要排序的字段
```

#### 视图

```
# bms/views.py

from bms.pagination import MyCursorPagination
class BookViewSet(ModelViewSet):
    queryset = models.Book.objects.all().order_by('id')
    serializer_class = BookModelSerializer
    """
    加密分页器
    """
    pagination_class = MyCursorPagination
```

# 六.解析器和渲染器

[参考资料](https://www.cnblogs.com/liwenzhou/p/10267985.html)

略.

# 七.对DRF中的`request`对象的相关总结

## 1.查看源码

1.`APIView`类

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214640919-984109625.png)

2.1.1`initialize_request`方法

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214443267-1195601520.png)

2.1.2`Request`类

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214507609-1850572750.png)

2.2.1`initial`方法

![img](https://img2018.cnblogs.com/blog/1464512/201901/1464512-20190119214517143-215536463.png)

## 2.总结

- `request.data` -- 前端post提交的数据
- `request.query_params` -- 前端页面的url参数
- `request.user` -- 通过认证的用户对象
- `request.auth` -- 前端发过来的token值
- `request.version` -- 版本号(如: v1, v2)
- `requst.versioning_scheme` -- 版本控制方案(5个)

# 八.版本,认证,权限,限制,分页 -- 源码查看方法

```
from rest_framework.versioning import *      # 查看 版本 源码
from rest_framework.authentication import *  # 查看 认证 源码
from rest_framework.permissions import *     # 查看 权限 源码
from rest_framework.throttling import *      # 查看 限制 源码
from rest_framework.pagination import *      # 查看 分页 源码

from django.core.handlers.wsgi import WSGIRequest   # 查看Django自己的request 源码

from rest_framework import settings # 查看settings.py文件中的配置项(版本,认证,权限,等等)
```

# 九.补充知识

## 1.`issubset()`

**描述:** `issubset()`方法用于判断集合的所有元素是否都包含在指定集合中, 如果是则返回True, 否则返回False.

**语法:**

```
set.issubset(set)
```

**参数:**

- `set` -- 必需, 要比较查找的集合

**返回值:** 返回布尔值, 如果都包含返回True, 否则返回False.

**实例说明:** 判断集合x的所有元素是否都包含在集合u中.

```
x = {"a", "b", "c"}
y = {"f", "e", "d", "c", "b", "a"}
z = x.issubset(y)
print(z)

# 执行结果:
# True
x = {"a", "b", "c"}
y = {"f", "e", "d", "c", "b"}
z = x.issubset(y)
print(z)

# 执行结果:
# False
```

## 2.语法糖`setter`,`getter`,`deleter`

**实例说明:**

- 例1:

```
class Person:
    def __init__(self, name):
        self.name = name

p1 = Person('王乃卉')
print(p1.name)

# 执行结果:
# 王乃卉
```

- 例2:

```
class Person:

    def __init__(self, name):
        self.name = name

    @property   # getter -- 获取属性
    def age(self):
        print('get age called')
        return self._age

    @age.setter # setter -- 设置属性
    def age(self, value):
        print('set age called')
        if not isinstance(value, int):
            raise TypeError('Excepted an int')
        self._age = value


p2 = Person('王力宏')  # 实例化
p2.age = 19     # 设置属性
print(p2.age)   # 获取属性

##执行结果:
# set age called
# get age called
# 19
```

- 例3:

```
class Person:

    def __init__(self, age):
        self.age = age

    @property   # getter -- 获取属性
    def age(self):
        print('get age called')
        return self._age

    @age.setter # setter -- 设置属性
    def age(self, value):
        print('set age called')
        if not isinstance(value, int):
            raise TypeError('Excepted an int')
        self._age = value


p3 = Person(19)     # 实例化,设置属性
print(p3.age)       # 获取属性

p3.age = 22         # 设置属性
print(p3.age)       # 获取属性

##执行结果:
# set age called
# get age called
# 19
# set age called
# get age called
# 22
```

- 例4:

```
class Person:
    def __init__(self):
        self.__name = None

    @property       # 访问属性
    def name(self):
        return self.__name

    @name.setter    # 设置属性
    def name(self, value):
        self.__name = value

    @name.deleter   # 删除属性
    def name(self):
        del self.__name

p = Person()
print(p.name)     # 访问属性 --> None
p.name = '王乃卉'  # 设置属性
print(p.name)     # 访问属性 --> 王乃卉
del p.name        # 删除属性
#print(p.name)    # 访问属性 --> 报错: 对象没有该属性
```

## 3.ORM之`update_or_create()`

```
# 检查是否有这条记录,有则更新(需要defaults参数,字典类型),无则新增
models.UserToken.objects.update_or_create(user=user_obj, defaults={
    "token": access_token
})
```

## 4.`assert`断言

根据[Python 官方文档解释](https://docs.python.org/3/reference/simple_stmts.html#assert) : `"Assert statements are a convenient way to insert debugging assertions into a program".`

语法:

```
assert condition
```

用来让程序测试这个`condition`, 如果`condition`为False, 则raise一个`AssertionError`出来. 逻辑上等同于:

```
if not condition:
    raise AssertionError('error_message')
```

**实例说明:**

```
>>> assert 1==1
>>> assert 1==0
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AssertionError

>>> assert True
>>> assert False
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AssertionError

>>> assert 1<2
>>> assert 1>2
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AssertionError
```

## 5.while循环测试

对比以下两个例子并思考为什么执行结果会不同.

- 例1:

```
lst1 = []
while lst1[-1] and lst1:
    print('这里是lst1')

# 执行结果:
# IndexError: list index out of range
```

- 例2:

```
lst2 = []
while lst2 and lst2[-1]:
    print('这里是lst2')

# 由于lst2为空,所以不执行while循环
```

