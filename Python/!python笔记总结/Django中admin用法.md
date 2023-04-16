# Django Admin管理工具

Django提供了基于web的管理工具。

Django自动管理工具是django.contrib的一部分。你可以在项目的settings.py中的INSTALLED_APPS看到它：

```
INSTALLED_APPS = （' django.contrib.admin ' ，
     ' django.contrib.auth ' ，
     ' django.contrib.contenttypes ' ，
     ' django.contrib.sessions ' ，
     ' django.contrib.messages ' ，
     ' django.contrib.staticfiles ' ，
 ）
```

django.contrib中是一套庞大的功能集，它是Django的基本代码的组成部分。

## 激活管理工具

通常我们在生成项目时会在urls.py中自动设置好，我们只需去掉注释即可。

配置项如下所示：



来自django的＃urls.py 。conf 。网址从django 导入网址。的contrib 进口管理员urlpatterns的= [ URL （[R ' ^管理员/ ' ，管理员。站点。网址），  ]                

当这一切都配置好后，Django管理工具就可以运行了。

------

## 使用管理工具

启动开发服务器，然后在浏览器中访问http://127.0.0.1:8000/admin/,

通过命令**python manage.py createsuperuser**来创建超级用户，如下所示：



```
#python manage.py createsuperuser 用户名（留空以使用'root' ）：admin
 电子邮件地址：admin @ runoob 。com
 密码：密码（再次）：超级用户创建成功。[ root @ solar HelloWorld ]＃
  
 
```

之后输入用户名密码登录;

为了让管理界面管理某个数据模型，我们需要先注册该数据模型到admin。比如，我们之前在TestModel中已经创建了模型测试。修改TestModel / admin.py：

```
from django.contrib import admin
from TestModel.models import Test
 
# Register your models here.
admin.site.register(Test)
```



刷新后即可见到Testmodel数据表';





