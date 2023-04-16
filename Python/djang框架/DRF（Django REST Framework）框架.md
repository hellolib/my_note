# DRF（Django REST Framework）框架

[TOC]

## 一.DRF中的Request

在Django REST Framework中内置的Request类扩展了Django中的Request类, 实现了很多方便的功能 -- 如请求数据解析和认证等.

比如, 区别于Django中的request: 从`request.GET`中获取URL参数, 从`request.POST`中去取某些情况下的POST数据(前端提交过来的数据).

在APIView中封装的request, 就实现了请求数据的解析:

- 对于GET请求的参数, APIView通过`request.query_params`来获取
- 对于POST请求、PUT请求的数据, APIView通过`request.data`来获取

## 二.前戏: 关于面向对象的继承

```
# 讲一个葫芦娃的故事

class Wa1(object):
    name = "红娃"

    def f1(self):
        print("力大无穷！")


class Wa2(object):
    name = '橙娃'

    def f2(self):
        print('千里眼顺风耳！')


class Wa3(object):
    name = '黄娃'

    def f3(self):
        print('钢筋铁骨！')


class Wa4(object):
    name = '绿娃'

    def f4(self):
        print("会喷火！")


class Wa5(object):
    name = '青蛙'

    def f5(self):
        print("会喷水！")


class Jishuwa(Wa1, Wa3, Wa5):
    name = '奇数娃'

    def ff(self):
        print("我是{}, 我会:".format(self.name))
        self.f1()
        self.f3()
        self.f5()


class Oushuwa(Wa2, Wa4):
    name = '偶数娃'

    def ff(self):
        print("我是{}, 我会:".format(self.name))
        self.f2()
        self.f4()


jsw = Jishuwa()
jsw.ff()
osw = Oushuwa()
osw.ff()


# 直接定义一个基数娃
class Taowa(Wa1, Wa3, Wa5):
    name = '套娃'

    def ff(self):
        print("我是{}, 我会:".format(self.name))
        self.f1()
        self.f3()
        self.f5()


class Wawa(Taowa):
    pass


print("=" * 120)
a = Wawa()
a.ff()
```

## 三.初级版本

### 1. `settings.py`文件 -- 注册app

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'bms.apps.BmsConfig',
    'rest_framework',   # 注册app
]
```

### 2. `models.py`文件 -- 创建表

```
from django.db import models

# 出版社表
class Publisher(models.Model):
    name = models.CharField(max_length=32)

    def __str__(self):
        return self.name

# 书籍表
class Book(models.Model):
    title = models.CharField(max_length=32)
    publisher = models.ForeignKey(to='Publisher', to_field='id', on_delete=models.CASCADE)

    def __str__(self):
        return self.title
# cd到当前项目目录
# 执行数据库迁移指令
python manage.py makemigrations
python manage.py migrate
```

### 3. `admin.py`文件

```
from django.contrib import admin
from bms import models  # bms是我们的app

admin.site.register(models.Publisher)
admin.site.register(models.Book)
# 创建超级用户
# cd到当前项目目录
python manage.py createsuperuser
# 启动Django项目
python manage.py runserver 127.0.0.1:8000
# 浏览器地址栏输入 127.0.0.1:8000
# 输入账号和密码,进入admin页面,对数据库中的表 添加或修改相关数据
```

### 4. 根目录下`urls.py` -- 路由匹配

```
from django.conf.urls import url
from django.contrib import admin
from bms import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    
    url(r'^book/$', views.BookListView.as_view()),
    url(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view()),
]
```

### 5. `bms/views.py` -- 视图函数

```python
from rest_framework.response import Response
from rest_framework.views import APIView
from bms import models
from bms.modelserializers import BookModelSerializer


class BookListView(APIView):
    def get(self, request):
        # 1.从数据库查询出所有书籍对象
        queryset = models.Book.objects.all()
        # 2.使用modelserializer对获取的对象进行序列化
        ser_obj = BookModelSerializer(queryset, many=True)
        return Response(ser_obj.data)

    def post(self, request):
        # 1.获取前端提交过来的数据 --> request.data
        # 2.对数据进行有效性校验
        ser_obj = BookModelSerializer(data=request.data)
        if ser_obj.is_valid():
            ser_obj.save()
            return Response('添加成功!')
        else:
            return Response(ser_obj.errors)


class BookDetailView(APIView):
    def get(self, request, pk):  # get获取具体某本书的信息
        # 1.根据pk去数据库中查询具体的那本书籍对象
        book_obj = models.Book.objects.filter(pk=pk).first()
        if book_obj:
            # 2.将书籍对象 序列化成 json格式的数据
            ser_obj = BookModelSerializer(book_obj)
            # 3.返回响应
            return Response(ser_obj.data)
        else:
            return Response('无效的书籍id')

    def put(self, request, pk):  # put修改具体某本书的信息
        # 1.根据pk去查询具体的那本书籍对象
        book_obj = models.Book.objects.filter(pk=pk).first()
        if book_obj:
            # 2.获取用户发送过来的数据并修改数据
            ser_obj = BookModelSerializer(instance=book_obj, data=request.data, partial=True)
            if ser_obj.is_valid():
                # 3.保存并返回修改后的数据
                ser_obj.save()
                return Response(ser_obj.data)
            else:
                return Response(ser_obj.errors)
        else:
            return Response('无效的书籍id')

    def delete(self, request, pk):  # delete删除具体某一本书籍对象
        # 1.根据pk去查询具体的那本书籍对象
        book_obj = models.Book.objects.filter(pk=pk).first()
        if book_obj:
            # 2.删除该书籍对象
            book_obj.delete()
            return Response('删除成功')
        else:
            return Response('无效的书籍id')
```

### 6. `bms/modelserializers.py` -- 自定义序列化工具

```python
from rest_framework import serializers
from bms import models


class PublisherSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    name = serializers.CharField()


class BookModelSerializer(serializers.ModelSerializer):
    publisher_info = serializers.SerializerMethodField(read_only=True)

    def get_publisher_info(self, book_obj):
        return PublisherSerializer(book_obj.publisher).data

    class Meta:
        model = models.Book
        fields = '__all__'
        extra_kwargs = {
            'publisher': {'write_only': True},
        }


class PublisherModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Publisher
        fields = '__all__'
```

## 四.进化版: 使用自定义混合类和自定义通用类

提取出`views.py`文件中函数`BookListView`和`BookDetailView`代码中的重复部分, 并将这些重复部分封装为**通用类(Generic)**和**混合类(Mixin)**, 利用Python强大的多继承功能, 将代码进一步优化. 充分体现Python语言的"优雅"和"简洁".

**注意:** 混合类Mixin不能单独实例化, 需要与其他的类搭配使用.

`bms/views.py`:

```python
from rest_framework.response import Response
from rest_framework.views import APIView
from bms import models
from bms.modelserializers import BookModelSerializer, PublisherModelSerializer


# 通用功能
class GenericView(APIView):
    queryset = None
    serializer_class = None

    def get_queryset(self, request, *args, **kwargs):
        # 再一次调用all()方法: 让每次请求来的时候都重新查一次数据
        return self.queryset.all()

    def get_obj(self, request, pk, *args, **kwargs):
        return self.get_queryset(request, *args, **kwargs).filter(pk=pk).first()


# get展示(全部)资源
class ListMixin(object):
    def get(self, request):
        queryset = self.get_queryset(request)
        ser_obj = self.serializer_class(queryset, many=True)
        return Response(ser_obj.data)


# post添加资源
class CreateMixin(object):
    def post(self, request):
        ser_obj = self.serializer_class(data=request.data)
        if ser_obj.is_valid():
            ser_obj.save()
            return Response('添加成功!')
        else:
            return Response(ser_obj.errors)


# get展示(部分)资源
class RetrieveMixin(object):
    def get(self, request, pk, *args, **kwargs):
        obj = self.get_obj(request, pk, *args, **kwargs)
        if obj:
            ser_obj = self.serializer_class(obj)
            return Response(ser_obj.data)
        else:
            return Response('无效的id!')


# put更新(修改)资源
class UpdateMixin(object):
    def put(self, request, pk, *args, **kwargs):
        obj = self.get_obj(request, pk, *args, **kwargs)
        if obj:
            ser_obj = self.serializer_class(instance=obj, data=request.data, partial=True)
            if ser_obj.is_valid():
                ser_obj.save()
                return Response(ser_obj.data)
            else:
                return Response(ser_obj.errors)
        else:
            return Response('无效的id!')


# delete删除资源
class DestroyMixin(object):
    def delete(self, request, pk, *args, **kwargs):
        obj = self.get_obj(request, pk, *args, **kwargs)
        if obj:
            obj.delete()
            return Response('删除成功!')
        else:
            return Response('无效的id!')


class BookListView(GenericView, ListMixin, CreateMixin):
    queryset = models.Book.objects.all()
    serializer_class = BookModelSerializer


class BookDetailView(GenericView, RetrieveMixin, UpdateMixin, DestroyMixin):
    queryset = models.Book.objects.all()
    serializer_class = BookModelSerializer


class PublisherListView(GenericView, ListMixin, CreateMixin):
    queryset = models.Publisher.objects.all()
    serializer_class = PublisherModelSerializer


class PublisherDetailView(GenericView, RetrieveMixin, UpdateMixin, DestroyMixin):
    queryset = models.Publisher.objects.all()
    serializer_class = PublisherModelSerializer
```

`urls.py`:

```python
from django.conf.urls import url
from django.contrib import admin
from bms import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    
    url(r'^book/$', views.BookListView.as_view()),
    url(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view()),
    url(r'^publisher/$', views.PublisherListView.as_view()),
    url(r'^publisher/(?P<pk>\d+)$', views.PublisherDetailView.as_view()),
]
```

## 五.超级进化版: 使用`GenericViewSet`通用类

`GenericViewSet`是`rest_framework`这个app中已经封装好了的一个类:

```
from rest_framework.viewsets import GenericViewSet
```

需要注意的是, 继承了`GenericViewSet`以后, `GenericViewSet`这个类已经帮我们封装好了`get_queryset()`和`get_object()`这两个方法, 它们不需要接收参数, 我们直接调用即可.

`bms/views.py`:

```
from rest_framework.response import Response
from bms import models
from bms.modelserializers import BookModelSerializer, PublisherModelSerializer
from rest_framework.viewsets import GenericViewSet  # 引入GenericViewSet通用类


# get展示(全部)资源
class ListMixin(object):
    def list(self, request, *args, **kwargs):
        queryset = self.get_queryset()
        ser_obj = self.serializer_class(queryset, many=True)
        return Response(ser_obj.data)


# post添加资源
class CreateMixin(object):
    def create(self, request, *args, **kwargs):
        ser_obj = self.serializer_class(data=request.data)
        if ser_obj.is_valid():
            ser_obj.save()
            return Response('添加成功!')
        else:
            return Response(ser_obj.errors)


# get展示(部分)资源
class RetrieveMixin(object):
    def retrieve(self, request, pk, *args, **kwargs):
        obj = self.get_object()
        if obj:
            ser_obj = self.serializer_class(obj)
            return Response(ser_obj.data)
        else:
            return Response('无效的id!')


# put更新(修改)资源
class UpdateMixin(object):
    def update(self, request, pk, *args, **kwargs):
        obj = self.get_object()
        if obj:
            ser_obj = self.serializer_class(instance=obj, data=request.data, partial=True)
            if ser_obj.is_valid():
                ser_obj.save()
                return Response(ser_obj.data)
            else:
                return Response(ser_obj.errors)
        else:
            return Response('无效的id!')


# delete删除资源
class DestroyMixin(object):
    def destroy(self, request, pk, *args, **kwargs):
        obj = self.get_object()
        if obj:
            obj.delete()
            return Response('删除成功!')
        else:
            return Response('无效的id!')


class BookViewSet(GenericViewSet, ListMixin, CreateMixin, RetrieveMixin, UpdateMixin, DestroyMixin):
    queryset = models.Book.objects.all()
    serializer_class = BookModelSerializer


class PublisherViewSet(GenericViewSet, ListMixin, CreateMixin, RetrieveMixin, UpdateMixin, DestroyMixin):
    queryset = models.Publisher.objects.all()
    serializer_class = PublisherModelSerializer
```

`urls.py`:

```
from django.conf.urls import url
from django.contrib import admin
from bms import views


urlpatterns = [
    url(r'^admin/', admin.site.urls),
    
    url(r'^book/$', views.BookViewSet.as_view(actions={'get': 'list', 'post': 'create'})),
    url(r'^book/(?P<pk>\d+)$', views.BookViewSet.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),
    url(r'^publisher/$', views.PublisherViewSet.as_view(actions={'get': 'list', 'post': 'create'})),
    url(r'^publisher/(?P<pk>\d+)$', views.PublisherViewSet.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),
]
```

## 六.究极进化版: 使用`rest_framework`帮我们封装好的通用类和混合类

`bms/views.py`:

```
from bms import models
from bms.modelserializers import BookModelSerializer, PublisherModelSerializer
from rest_framework.viewsets import ModelViewSet


class BookViewSet(ModelViewSet):
    queryset = models.Book.objects.all()
    serializer_class = BookModelSerializer


class PublisherViewSet(ModelViewSet):
    queryset = models.Publisher.objects.all()
    serializer_class = PublisherModelSerializer
```

## 七.终极进化版: 使用`rest_framework`帮我们封装好的路由`DefaultRouter`

```
from rest_framework.routers import DefaultRouter
from bms import views

urlpatterns = []

router = DefaultRouter()
router.register('book', views.BookViewSet)
router.register('publisher', views.PublisherViewSet)

# 重写urlpatterns
urlpatterns += router.urls
```