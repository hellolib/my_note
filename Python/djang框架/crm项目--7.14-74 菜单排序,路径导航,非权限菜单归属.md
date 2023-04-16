## 1.一级菜单排序表结构 

- 在一级菜单的数据表中添加了权重字段,根据权重大小定一次菜单的显示顺序

- ```python
  weight = models.IntegerField(default=1)
  ```

### 1.1权限的数据结构

- permission_list = []

```python
permission_list = [{'url': i['permissions__url'],
                    'title': i['permissions__title'],
                  },
                   {'url': i['permissions__url'],
                    'title': i['permissions__title'],
                  }]
```

### 1.2菜单的数据结构 

- menu_dict = {}

```python
{   
   1 : {
        'title' : '财务管理',
        'icon' : 'fa-cny',
        'children' :  [ 
            { 'title':'缴费列表' ,'url':'/payment/list/'},
            { 'title':'报销列表' ,'url':'/baoxiao/list/'},
         ]

    } ,
    2 : {
        'title' : '客户管理',
        'icon' : 'fa-user-o',
        'children' :  [ 
            { 'title':'客户列表' ,'url':'/customer/list/'},
         ]
    } ,
}



#该数据结构的构造方式
ret = {}

for i in data:
    menu_id = i.get('permissions__menu_id')
    if not menu_id:
        continue

    if menu_id not in ret:
        ret[menu_id] = {
            'title': i.get('permissions__menu__title'),
            'icon': i.get('permissions__menu__icon'),
            'children': [
                {'title': i.get('permissions__title'), 'url': i.get('permissions__url')}
            ]
        }
    else:
        ret[menu_id]['children'].append({'title': i.get('permissions__title'), 'url': i.get('permissions__url')})

print(ret)
```



## 2.非菜单权限归属表结构 

- 在权限表中添加了自关联,记录子权限归属父权限id(二级菜单id)

  ```python
  parent = models.ForeignKey('self', blank=True, null=True)  # 父级菜单id(二级菜单id)
  ```

### 2.1权限的数据结构 

- permission_list = []

```python
permission_list = [{'url': i['permissions__url'],
                    'id': i['permissions__id'],
                     'title': i['permissions__title'],
                    'pid': i['permissions__parent_id'], },
                  {'url': i['permissions__url'],
                    'id': i['permissions__id'],
                     'title': i['permissions__title'],
                    'pid': i['permissions__parent_id'], }]
```

### 2.2菜单的数据结构 

- menu_dict = {}

```python
#   数据结构

{
    1: {
        'title': i.get('permissions__menu__title'),
        'icon': i.get('permissions__menu__icon'),
        'weight': i.get('permissions__menu__weight'),
        'children': [
            {'title': i.get('permissions__title'),
             'url': i.get('permissions__url'),
             'id': i.get('permissions__id')}
        ]
    },
    2: {
        'title': i.get('permissions__menu__title'),
        'icon': i.get('permissions__menu__icon'),
        'weight': i.get('permissions__menu__weight'),
        'children': [
            {'title': i.get('permissions__title'),
             'url': i.get('permissions__url'),
             'id': i.get('permissions__id')}
        ]
    },
}
```

## 3.路径导航

### 2.1权限的数据结构 

- permission_dict={}

```python
permission_dict[i['permissions__id']] = {'url': i['permissions__url'],
                                         'id': i['permissions__id'],
                                         'title': i['permissions__title'],
                                         'pid': i['permissions__parent_id'], }



# 数据结构

{
    i['permissions__id']:{'url': i['permissions__url'],
                          'id': i['permissions__id'],
                          'title': i['permissions__title'],
                          'pid': i['permissions__parent_id'], }
}
```

### 2.2菜单的数据结构 

- menu_dict = {}

```python
#   数据结构

{
    1: {
        'title': i.get('permissions__menu__title'),
        'icon': i.get('permissions__menu__icon'),
        'weight': i.get('permissions__menu__weight'),
        'children': [
            {'title': i.get('permissions__title'),
             'url': i.get('permissions__url'),
             'id': i.get('permissions__id')}
        ]
    },
    2: {
        'title': i.get('permissions__menu__title'),
        'icon': i.get('permissions__menu__icon'),
        'weight': i.get('permissions__menu__weight'),
        'children': [
            {'title': i.get('permissions__title'),
             'url': i.get('permissions__url'),
             'id': i.get('permissions__id')}
        ]
    },
}
```

