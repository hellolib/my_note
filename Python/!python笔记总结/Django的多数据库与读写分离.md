# Django的多数据库与读写分离

## 1.多个数据库

- settings.py

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

- 迁移其他的数据库

`python manage.py migrate --database db2`

## 2.读写分离

### 2.1手动指定

```
models.Student.objects.using('db2').all()

obj = models.Student.objects.using('db2').get(name='zhazha')
obj.name = 'star'
obj.save(using='default')
```

### 2.4自动选择

- settings配置:

```
DATABASE_ROUTERS = ['myrouter.Router']
```

- 创建 router.py

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

