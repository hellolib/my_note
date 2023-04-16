# django  

**web框架(socket服务端)**

- socket收发消息

- 根据不同的路径返回不同内容

- 动态的页面(模板渲染) jinja2 

  

在浏览器中输入地址,之后会发生什么样的事情?



http协议

请求应答的标准

请求的格式(request)

'请求方式 URL的路径 HTTP/1.1\r\n

k1: v1\r\n

k2: v2\r\n

\r\n

请求体(请求数据)'            GET请求没有请求体 

响应的格式(response)

'HTTP/1.1 状态码 状态描述\r\n

k1: v1\r\n

k2: v2\r\n

\r\n

响应体(响应数据)'    



状态码

1xx    请求已经接受,进一步处理

2xx    200   OK 

3xx    重定向  301  302

4xx    请求的错误   404   403  402

5xx    服务器的错误   500  502      



头信息

content-type   json    

user-agent  

cookie   

set-cookie

host 

Location:地址 



## 路由

### 正则表达式

r''    ^   $  [a-zA-Z0-9]{4}   \d    \w   .  ?   +  *   

### 分组和命名分组

```
url(r'^edit_customer/(\d+)/', consultant.customer_change, name='edit_customer'),
```

分组   将捕获的参数按照  位置传参  传递给视图函数

```
url(r'^edit_customer/(?P<cid>\d+)/', consultant.customer_change, name='edit_customer'),
```

命名分组   将捕获的参数按照  关键字传参  传递给视图函数

### url的命名和反向解析

静态路由

```
url(r'^login/', auth.login, name='login'),
```

模板:

{% url 'login' %}     _>     '/app01/login/'

py文件:

```
from django.urls import reverse
reverse('login')    _>   '/app01/login/'
```

分组

```python
url(r'^edit_customer/(\d+)/', consultant.customer_change, name='edit_customer'),
```

模板:

{% url 'edit_customer' pk %}     _>    '/app01/edit_customer/1/'    

py文件:

```
from django.urls import reverse
reverse('edit_customer',args=(2,))    _>    '/app01/edit_customer/2/'    
```

命名分组

```python
url(r'^edit_customer/(?P<pk>\d+)/', consultant.customer_change, name='edit_customer'),
```

模板:

{% url 'edit_customer' 1 %}     _>    '/app01/edit_customer/1/'    

{% url 'edit_customer' pk=1 %}     _>    '/app01/edit_customer/1/'    

py文件:

```
from django.urls import reverse
reverse('edit_customer',args=(2,))    _>    '/app01/edit_customer/2/'  
reverse('edit_customer',kwargs={'pk':'2'})    _>    '/app01/edit_customer/2/'  
```

### 路由分发

```
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^crm/', include('crm.urls')),
    url(r'^rbac/', include('rbac.urls')),
]
```

### namespace

{% url 'app01:edit_customer' 1 %}  

reverse('app01:edit_customer',args=(2,)) 

## 视图

view 

MVC

M: model  

V: 视图   HTML 

C: 控制器 controller 

MTV

M: model  orm

T: template  模板 

V: 视图 业务逻辑 

FBV   CBV 

```python
# FBV
def xxxx(request,*args,**kwargs):
    # 业务逻辑
    return  response 

# CBV
from django.views  import View
class Xxx(View):
    
    def get(self,request,*args,**kwargs):
        # 业务逻辑
    	return  response 
        
    def post(self,request,*args,**kwargs):
        # 业务逻辑
    	return  response 
    
urls.py 
 url(r'^xxx/', Xxx.as_view()),
```

CBV装饰器的加法:

```
from django.utils.decorators import method_decorator
```

1. 加在方法上

   @method_decorator(timer)

   def get(self,request,*args,**kwargs):

2. 加在dispatch方法上

   @method_decorator(timer)

   def dispatch(self,request,*args,**kwargs):



​		@method_decorator(timer,name='dispatch')

​		class Xxx(View):

3. 加在类的上:

   @method_decorator(timer,name='post')

   @method_decorator(timer,name='get')

   class Xxx(View):

其他:

```
from django.views.decorators.csrf import csrf_exempt,csrf_protect
csrf_exempt只能加在dispatch上
```

request对象

```python
#属性
request.method   #  请求方式  GET POST DELETE OPTIONS PUT TARCE HEAD PATCH
request.GET      #  url上携带的参数    url  ?key=v1
request.POST     # post请求发送的数据  application/x-www-form-urlencoded
request.body     # 请求体  
request.FILES    # 上传的文件  enctype="multipart/form-data"
request.COOKIES  #   普通的cookie   请求头cookie
request.session  # session 
request.META    # 头的信息   xxx  _>  XXX   HTTP_   - _> _ 
request.path_info  # 路径  不包含ip和端口 也不包含参数  /login/

# 方法
request.get_full_path()  # 完整的路径  不包含ip和端口 包含参数  /login/?next=/xxxxx/
request.is_ajax()
request.get_host()  
```

response对象

HttpResponse('字符串')     返回字符串

render(request,'模板的路径',{})       返回一个完整的页面  

redirect('重定向的地址')       重定向    Location:地址 

JsonResponse({})      JsonResponse([],safe=False)       

TemplateResponse(request,'模板的路径',{})

## 模板

{{  变量  }}   {%  %}   tags 

render(request,'模板的路径',{'k1':'v1'})  

{{ k1 }}   . 索引  .属性 .方法  .字典的key

### 过滤器 filter 

{{  k1|filter_name }}  {{  k1|filter_name:'xxx' }}

内置的过滤器

safe    mark_safe   from django.utils.safestring import mark_safe

default   add   date:'Y-m-d  H:i:s'    filesizeformat

### 标签

for 

{%  for i in list %}

​		{{ i }}   {{  forloop.counter }}

{% endfor %}

{%  for i in list %}

​		{{ i }}   {{  forloop.counter }}

{% empty %}

​	xxxx

{% endfor %}



{%  if  条件 %}

​	xx

{% elif  条件 %}

   xxx

{% else %}

  xx

{% endif %}

if  不支持连续判断     不支持算数运算   

with 

{% with  xxxx  as  sss %}



{%  endwith %}



{% csrf_token %}     csrfmiddlewaretoken    64

母版和继承

{%  extends  'base.html' %}

组件 

{% include 'nav.html' %} 

静态文件

{%  load  static %}

{% static   '相对路径' %}  

{% get_static_prefix  %}

### 自定义filter simple_tag  inclusion_tag

1. 在app下创建一个名为templatetags的python包  # templatetags 不能错 

2. 在包内创建Python文件   my_tags.py

3. 在py文件中写代码:

   ```python
   from django import template
   register= template.Library()   # template不能错 
   ```

4. 写函数 + 加装饰器

   ```python
   @register.filter
   def  xxx(value,arg):
       return  xxxxx 
   
   @register.simple_tag
   def xxx2(*args,**kwargs):
       return  xxxxx 
   
   @register.inclusion_tag('模板')
   def xxx3(*args,**kwargs):
       return  {'num':range(num)} 
   
   # 模板 
   {% for i in num  %}
   	{{ i }}
   {% endfor %}
   ```

5. 使用

   ```HTML
   {% load my_tags %}
   
   {{ k1|xxx:'xxx' }}   # 可以使用在tag中
   
   {%  xxx2 v1 v2 k1=v1 k2=v2  %}
   
   {% xxx3  num=10 %}
   ```

## ORM

### 对象关系映射

对应关系

类     _>    表

对象  _>   数据行(记录)

属性  _>    字段

### 1.必知必会13条

返回对象列表的

```python
all()     # 返回所有的数据
filter()  # 返回所有符合条件的所有数据
exclude()  # 返回所有不满足条件的数据
order_by()  # 排序
reverse()   # 倒叙
distinct()  # 去重
values()   # 获取字段的名和值  [{},{}]
values_list()   # 获取字段值  [(),()]
```

返回对象

### 

```python
get()   # 获取一个满足条件的对象  没有或者多个都报错
first()  # 获取第一个元素
last()  # 获取最后一个元素
```

返回布尔值

exists()  # 是否存在

返回数字

count()   #  计数

### 2.单表的双下滑线

filter(name='xxxx')

id__gt=

id__gte=

id__lt=

id__lte=

id__range = [1,6]

id_in= [1,4,5,6]

name__contains = 'alex'

name__icontains = 'alex'

__startswith=

__endswith=

__isnull = True

### 3.外键

描述一对多的关系

```Python
class Publisher(models.Model):
    name = models.CharField(max_length=32, verbose_name="名称")


class Book(models.Model):
    name = models.CharField(max_length=32, verbose_name="书名")
    pub = models.ForeignKey(to='Publisher')
```

对象的查询

book_obj.pub   _>   所关联的对象   book_obj.pub.name

book_obj.pub_id    

pub_obj.book_set  _>  关系管理对象

pub_obj.book_set.all()   _> 所有的书籍对象

指定related_name='books'后

pub_obj.books.all()   _> 所有的书籍对象

字段的查询

models.Book.objects.filter(pub__name='xxxx')

不指定related_name

models.Publisher.objects.filter(book__name='xxxx')

指定related_name='books'后

models.Publisher.objects.filter(books__name='xxxx')

指定related_query_name='book'后

models.Publisher.objects.filter(book__name='xxxx')

### 4.多对多

```python
class Book(models.Model):
    name = models.CharField(max_length=32, verbose_name="书名")
    pub = models.ForeignKey(to='Publisher', related_name='books', related_query_name='book')


class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name="姓名")
    books = models.ManyToManyField(to='Book', )  # 生成一张表
```

关系管理对象的方法

set   设置关系    [  对象,对象 ]   [id,id]

add  添加关系   add()   对象,对象     id,id 

remove  添加关系

clear()   情况所有的关系

create()   新增一个对象并且同当前的对象设置关系

ORM进阶操作

### 分组和聚合

```
from django.db.models import Max, Min, Count, Avg, Sum
# 聚合
Book.objects.filter().aggregate(max=Max('price'))    #  {'max':xxxx}

# 分组
Author.objects.annotate(count=Count('books'))    #  对象列表   [对象,]

Book.objects.values('author').annotate(Count('id'))   # 对象列表  [{},{}]
```

### F 和 Q

```python
from django.db.models import F,Q 
# F
Book.objects.filter(sale__gt=F('stock'))  # 比较两列数据
Book.objects.update(sale=F('sale') * 2 + 100)

# Q  条件
|  或
&  与
~  非
Book.objects.filter(~Q(id__gt=10)|Q(id__lt=5))


q = Q()
q.connector = 'OR'
q.children.append(Q(('name__contains', 10)))
q.children.append(Q(('xxxx__contains', 10)))
```

### 事务

```sql

begin;

xxxx
xxx

commit;
```

```python
try:
    with transaction.atomic():
        """ 
        一系列操作
        """
        Book.objects.create(name='xxxx')
        Book.objects.create(name='xxxx')
        Book.objects.create(name='xxxx')
        Book.objects.create(name='xxxx')

except Exception:
    pass
```

### 行级锁

```Python
try:
    with transaction.atomic():
        """ 
        一系列操作
        """
        query_set = Book.objects.filter(id__gt=5).select_for_update()
        query_set

except Exception:
    pass
```

## cookie session

### cookie

为什么有cookie?

http协议是无状态的,每次的请求是相互独立的,没办法保存状态.

cookie是保存在浏览器上一组组键值对.

特点:

​	    1.由服务器让浏览器进行设置

  		2. 浏览器有权进行读取或者保存
       3. 下次访问时自动携带相应的cookie

django中的操作

 1. 设置

    response.set_cookie(key,value,max-age=10,)        #  set-cookie

    response.set_signed_cookie(key,value,salt='xxx')

	2. 获取

    request.COOKIES {  }   request.COOKIES[KEY]   request.COOKIES.get()    # cookie

	3. 删除cookie

    response.delete_cookie(key)     #  set-cookie

### session

为什么要有session?

cookie保存浏览器本地  不安全

http协议或者浏览器对cookie的大小有限制 

session是保存在服务器上一组组键值对,必须依赖cookie.

django中操作session

1. 设置

   request.session[key] = value

2. 获取

   request.session[key]  request.session.get(key)

3. 删除

   del request.session[key]

   request.session.pop(key)

   request.session.delete()    #删除所有的数据    不删除cookie

   request.session.flush()      #删除所有的数据    删除cookie

4. 其他

   request.session.set_expiry(100)

   request.session.clear_expired()   #  清除已过期的session数据

```
SESSION_SAVE_EVERY_REQUEST = True
SESSION_EXPIRE_AT_BROWSER_CLOSE = True
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
存储的地方: 数据库(默认) 缓存  缓存+数据库 文件 加密cookie

```

## ajax

ajax是js 的技术,发请求.

jq 

```HTML

$.ajax({
	url:'地址',
	type:'post',
	data:{
		xxxx:'xxx',
		xxx':'qqqqq',
	},
	success:function(res){
			
	},
})
```

上传文件

```html

var  formdata = new FormData()

formdata.append('key','v1')
formdata.append('f1',$('#f1')[0].files[0])


$.ajax({
	url:'地址',
	type:'post',
	conentType:false,    #  不需要处理content-Type的请求头
	procssData:false,    #  不需要处理数据的编码  
	data:formdata,
	success:function(res){	
	},
})
```

通过django中csrf的校验

前提: 必须有cookie     csrftoken        

 1. 使用 {% csrf_token %}

 2. ```
    from django.views.decorators.csrf import ensure_csrf_cookie
    ```

1.data中添加csrfmiddlewaretoken的键值对

2.请求头中添加x-csrftoken 

3.引入文件

## 中间件

django的中间件是一个类,处理django的请求和响应的框架级别的钩子.

5个方法  4个特点

```
from django.utils.deprecation import MiddlewareMixin
class Jasdk(MiddlewareMixin):
```

```Python
def process_request(self, request):
执行时间: 路由匹配之前
执行顺序: 按照注册的顺序  顺序执行 
返回值:
	None  正常的流程
	HttpResponse 当前中间件之后的中间件的process_request 路由匹配 视图函数都不执行,直接执行当前中间件的process_response方法,倒叙执行之前的中间件中的process_response方法
```

```Python
def process_response(self, request,response):
执行时间: 视图函数之后
执行顺序: 按照注册的顺序  倒叙执行 
返回值:
	HttpResponse 必须返回
```

```Python
def process_view(self, request, view_func, view_args, view_kwargs):
执行时间: 路由匹配之后,视图函数之前
执行顺序: 按照注册的顺序  顺序执行 
返回值:
	None  正常的流程
	HttpResponse 当前中间件之后的中间件的process_view,视图函数都不执行,直接执行最后一个中间件的process_response方法,倒叙执行之前的中间件中的process_response方法
```
```Python
def process_exception(self, request, exception):
执行时间: 视图函数出错
执行顺序: 按照注册的顺序  倒叙执行 
返回值:
	None  当前中间件没有处理异常,交由下一个中间件处理,如果所有都不处理,django处理错误
	HttpResponse 当前中间件之前的中间件的process_exception不执行,直接执行最后一个中间件的process_response方法,倒叙执行之前的中间件中的process_response方法
```
```Python
def process_template_response(self, request,response):
执行时间: 视图函数返回的是一个template_response或者对象有一个render方法
执行顺序: 按照注册的顺序  倒叙执行 
返回值:
	HttpResponse 必须返回对象   可以处理模板,数据
     response.template_name = '模板的文件名'
     response.context_data = {}
```

## form  modelform  modelformset

form  

定义:

```
from django import forms
from django.core.exceptions import ValidationError
from django.core.validators import RegexValidator

def sd(value):
    # 通过校验规则 什么都不干
    # 不通过校验规则 抛出异常  ValidationError('xxxxxx)


class Form(forms.Form):
    name = forms.CharField(
        label='姓名',
        widget=forms.TextInput(attrs={'class':'form-control'}),
        error_messages ={'required':'此字段是必填的',},
        validators=[]
    )
    age = forms.IntegerField()
    gender = forms.MultipleChoiceField(choices=(('1', '男'), ('2', '女')))


    def clean_age(self):
        self.cleaned_data
        # 通过校验规则  返回当前字段的值
        # 不通过校验规则 抛出异常  ValidationError('xxxxxx)

    def clean(self):
        self.cleaned_data
        # 通过校验规则  返回所有字段的值

        # 不通过校验规则
        #  self.add_error('xxx','xxxxxx')
        # 抛出异常  ValidationError('xxxxxx)   _> __all__
```

py文件:

```
def add_customer(request):
    # form_obj 没有数据
    form_obj = CustomerForm()
    if request.method == 'POST':
        # form_obj 包含提交的数据
        form_obj = CustomerForm(request.POST)
        if form_obj.is_valid():   # 对数据进行校验
            # 校验成功
            form_obj.save()
            return redirect(reverse('customer_list'))

    return render(request, 'consultant/add_customer.html', {'form_obj': form_obj})
```



```
{% for field in form_obj %}

    <div class="form-group  {% if field.errors %}has-error{% endif %}">
        <label   {% if not field.field.required %} style="color: #777777" {% endif %} for="{{ field.id_for_label }}"
                                               class="col-sm-2 control-label">{{ field.label }}</label>
        <div class="col-sm-8">
            {{ field }}
            <span class="help-block">{{ field.errors.0 }}</span>
        </div>
    </div>
{% endfor %}

{{ form_obj.errors }}
{{ form_obj.non_field_errors }}
```

modelform  

```
class Form(forms.ModelForm):
    
    
    class Meta:
        model = models.Customer
        fields = "__all__"   # ['']
        exclude = []
        
        labels ={}
        error_messages={}
        widgets ={}
        
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        for filed in self.fields.values():
            filed.widget.attrs['class'] = 'form-control'
```



```Python
def customer_change(request, pk=None, ):
    obj = models.Customer.objects.filter(pk=pk).first()

    form_obj = CustomerForm(instance=obj)
    if request.method == 'POST':
        form_obj = CustomerForm(data=request.POST, instance=obj)
        if form_obj.is_valid():
            form_obj.save()
            return redirect(reverse('customer_list'))
    title = '编辑客户' if pk else '新增客户'
    return render(request, 'form.html', {'form_obj': form_obj, 'title': title})
```

modelformset





