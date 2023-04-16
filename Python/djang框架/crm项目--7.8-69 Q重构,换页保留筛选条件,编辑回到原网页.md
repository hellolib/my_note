## 1.Q的重构

```python
from django.db.models import Q


def search(request, field_list):
    """
    Q对象的重构
    Q(Q(qq__contains=query) | Q(name__contains=query) | Q(phone__contains=query))
    :return:
    """
    # 首先获取前端form表单传来的关键字
    query = request.GET.get('query', '')
    # 实例化Q
    q = Q()
    # 把Q的选择变为OR
    q.connector = 'OR'
    # 循环要匹配的额字段列表
    for field in field_list:
    # 循环加入列表children [qq__contains=query,name__contains=query,phone__contains=query]
    q.children.append(Q(('{}__contains'.format(field), query)))
    # 返回q,q的样式:Q(Q(qq__contains=query) | Q(name__contains=query) | Q(phone__contains=query))
    return q

```

## 2.分页保留搜索条件

```python
request.GET   <class 'django.http.request.QueryDict'>   'query': ['13']  
request.GET.urlencode()    ——》  query=13&page=1
request.GET._mutable = True  # 可编辑
request.GET.copy()   # 深拷贝 可编辑
QueryDict(mutable=True)   # 可编辑
```

具体使用详见my_models--分页pagination

## 3.编辑后跳转到原界面

```python
# 使用自定义的simple_tag
from django import template
from django.urls import reverse
from django.http.request import QueryDict

register = template.Library()


@register.simple_tag
def reverse_url(request, name, *args, **kwargs):
    """
    编辑返回原界面
    :param request:  request对象
    :param name:   url别名
    :param args:
    :param kwargs:
    :return:  返回原url路径
    """
    # 获取当前的访问路径
    old_url = request.get_full_path()
    # 要访问的url
    url = reverse(name, args=args, kwargs=kwargs)
    # 定义querydict
    q_dict = QueryDict(mutable=True)
    q_dict['old_url'] = old_url
    # 拼接路径,把原路径存到url中
    return_url = '{}?{}'.format(url, q_dict.urlencode())
    return return_url
```

