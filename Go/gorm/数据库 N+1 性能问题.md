# ORM 引发的数据库 N+1 性能问题

> 每一句话都是重点

## 1. 背景描述

最近在使用 Django 时，发现当调用 api 后，在数据库同一个进程下的事务中，出现了大量的数据库查询语句。调查后发现，是由于 Django ORM 的机制所引起。

Django Object-Relational Mapper（ORM）作为 Django 比较受欢迎的特性，在开发中被大量使用。我们可以通过它和数据库进行交互，实现 DDL 和 DML 操作.

具体来说，就是使用 [QuerySet](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet) 对象来检索数据， 而 QuerySet 本质上是通过在预先定义好的 model 中的 [Manager](https://docs.djangoproject.com/en/3.1/topics/db/managers/#django.db.models.Manager) 和数据库进行交互。

Manager 是 Django model 提供数据库查询的一个接口，在每个 Model 中都至少存在一个 Manager 对象。但今天要介绍的主角是 QuerySet ，它并不是关键。

为了更清晰的表述问题，假设在数据库有如下的表：

device 表，表示当前网络中纳管的物理设备。

interface 表，表示物理设备拥有的接口。

interface_extension 表，和 interface 表是一对一关系，由于 interface 属性过多，用于存储一些不太常用的接口属性。

```python
class Device(models.Model):
    name = models.CharField(max_length=100, unique=True)  # 添加设备时的设备名
    hostname = models.CharField(max_length=100, null=True)  # 从设备中获取的hostname
    ip_address = models.CharField(max_length=100, null=True)  # 设备管理IP

class Interface(models.Model):
    device = models.ForeignKey(Device, on_delete=models.PROTECT, null=False，related_name='interfaces')) # 属于哪台设备
    name = models.CharField(max_length=100)  # 端口名
    collect_status = models.CharField(max_length=30, default='active')
    class Meta:
        unique_together = ("device", "name")  # 联合主键
        
class InterfaceExtension(models.Model):
    interface = models.OneToOneField(
        Interface, on_delete=models.PROTECT, null=False, related_name='ex_info')
        
    endpoint_device_id = models.ForeignKey( # 绑定了的终端设备
        Device, db_column='endpoint_device_id',
        on_delete=models.PROTECT, null=True, blank=True)
        
    endpoint_interface_id = models.ForeignKey(
        Interface, db_column='endpoint_interface_id', on_delete=models.PROTECT, # 绑定了的终端设备的接口
        null=True, blank=True)
```

简单说一下之间的关联关系，一个设备拥有多个接口，一个接口拥有一个拓展属性。

在接口的拓展属性中，可以绑定另一台设备上的接口，所以在 interface_extension 还有两个参考外键。

为了更好的分析 ORM 执行 SQL 的过程，需要将执行的 SQL 记录下来，可以通过如下的方式：

- 在 django settings 中打开 sql log 的日志
- 在 MySQL 中打开记录 sql log 的日志

django 中，在 `settings.py` 中配置如下内容, 就可以在控制台上看到 SQL 执行过程：

```python
DEBUG = True

import logging
l = logging.getLogger('django.db.backends')
l.setLevel(logging.DEBUG)
l.addHandler(logging.StreamHandler())

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'filters': {
        'require_debug_false': {
            '()': 'django.utils.log.RequireDebugFalse'
        }
    },
    'handlers': {
        'mail_admins': {
            'level': 'ERROR',
            'filters': ['require_debug_false'],
            'class': 'django.utils.log.AdminEmailHandler'
        },'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db': {
            'level': 'DEBUG',
            'handlers': ['console'],
        },
    }
}
```

或者直接在 MySQL 中配置：

```sh
# 查看记录 SQL 的功能是否打开，默认是关闭的：
SHOW VARIABLES LIKE "general_log%";

# 将记录功能打开，具体的 log 路径会通过上面的命令显示出来。
SET GLOBAL general_log = 'ON';
```

## 2. QuerySet

假如要通过 QuerySet 来查询，所有接口的所属设备的名称:

```python
interfaces = Interface.objects.filter()[:5] # hit once database

for interface in interfaces: 
    print('interface_name: ', interface.name,
          'device_name: ', interface.device.name) # hit database again
```

**上面第一句取前 5 条 interface 记录，对应的 raw sql 就是 `select * from interface limit 5;` 没有任何问题。**

**但下面取接口所属的设备名时，就会出现反复调用数据库情况：当遍历到一个接口，就会通过获取的 device_id 去数据库查询 device_name. 对应的 raw sql 类似于：`select name from device where id = {}`.**

**也就是说，假如有 10 万个接口，就会执行 10 万次查询，性能的消耗可想而知。算上之前查找所有接口的一次查询，合称为 N + 1 次查询问题。**

**解决方式也很简单，如果使用原生 SQL，通常有两种解决方式：**

- 在第一次查询接口时，使用 join，将 interface 和 device 关联起来。这样仅会执行一次数据库调用。
- 或者在查询接口后，通过代码逻辑，将所需要的 device_id 以集合的形式收集起来，然后通过 in 语句来查询。类似于 `SELECT name FROM device WHERE id in (....)`. 这样做仅会执行两次 SQL。

具体选择哪种，就要结合具体的场景，比如有无索引，表的大小具体分析了。

回到 QuerySet，那么如何让 QuerySet 解决这个问题呢，同样也有两种解决方法，使用 QuerySet 中提供的 `select_related()` 或者 `prefetch_related()` 方法。

## 3. 问题解决

- 回到 QuerySet，那么如何让 QuerySet 解决这个问题呢，同样也有两种解决方法，使用 QuerySet 中提供的 `select_related()` 或者 `prefetch_related()` 方法。

### 3.1  select_related

- 在调用 `select_related()` 方法时，Queryset 会将所属 Model 的外键关系，一起查询。相当于 raw sql 中的 `join` . 一次将所有数据同时查询出来。`select_related()` 主要的应用场景是：某个 model 中关联了外键（多对一），或者有 1 对 1 的关联关系情况。

- 还拿上面的查找接口的设备名称举例的话：

```python
interfaces = Interface.objects.select_related('device').filter()[:5] # hit once database

for interface in interfaces:
    print('interface_name: ', interface.name,
         'device_name: ', interface.device.name) # don't need to hit database again 
```

- 上面的查询 SQL 就类似于：`SELECT xx FROMinterface INNER JOIN device ON interface.device_id = device.id limit5`，注意这里是 inner join 是因为是非空外键。

- `select_related()` 还支持一个 model 中关联了多个外键的情况：如拓展接口，查询绑定的设备名称和接口名称：

```python
ex_interfaces = InterfaceExtension.objects.select_related(
    'endpoint_device_id', 'endpoint_interface_id').filter()[:5] 

# or

ex_interfaces = InterfaceExtension.objects.select_related(
    'endpoint_device_id').select_related('endpoint_interface_id').filter()[:5] 
```

- 上面的 SQL 类似于：
  - 这里由于是可空外键，所以是 left join.

```sql
SELECT XXX FROM interface_extension LEFT OUTER JOIN device ON (interface_extension.endpoint_device_id=device.id) 
LEFT OUTER JOIN interface ON (interface_extension.endpoint_interface_id=interface.id)
LIMIT 5
```

>  如果想要清空 QuerySet 的外键关系，可以通过：`queryset.select_related(None)` 来清空。

### 3.2 prefetch_related

>**prefetch_related** 和 **select_related** 一样都是为了避免大量查询关系时的数据库调用。只不过为了避免多表 join 后产生的巨大结果集以及效率问题， 所以 **select_related** 比较偏向于外键（多对一）和一对一的关系。
>
>而 **prefetch_related** 的实现方式则类似于之前 raw sql 的第二种，分开查询之间的关系，然后通过 python 代码，将其组合在一起。所以 **prefetch_related** 可以很好的支持一对多或者多对多的关系。

- 还是拿查询所有接口的设备名称举例：

```python
interfaces = Interface.objects.prefetch_related('device').filter()[:5] # hit twice database

for interface in interfaces:
    print('interface_name: ', interface.name,
         'device_name: ', interface.device.name) # don't need to hit database again
```

- 换成 **prefetch_related** 后，sql 的执行逻辑变成这样：

  >1. "SELECT * FROM interface "
  >2. "SELECT * FROM device where device_id in (.....)"
  >3. 然后通过 python 代码将之间的关系组合起来。

- 如果查询所有设备具有哪些接口也是一样：

```python
devices = Device.objects.prefetch_related('interfaces').filter()[:5] # hit twice database
for device in devices:
    print('device_name: ', device.name,
          'interface_list: ', device.interfaces.all())
```

- 执行逻辑也是：

  >1. "SELECT * FROM device"
  >2. "SELECT * FROM interface where device_id in (.....)"
  >3. 然后通过 python 代码将之间的关系组合起来。

- 如果换成多对多的关系，在第二步会变为 join 后在 in，具体可以直接尝试。

  但有一点需要注意，当使用的 QuerySet 有新的逻辑查询时， prefetch_related 的结果不会生效，还是会去查询数据库：

  如在查询所有设备具有哪些接口上，增加一个条件，接口的状态是 up 的接口

```python
devices = Device.objects.prefetch_related('interfaces').filter()[:5] # hit twice database
for device in devices:
    print('device_name: ', device.name,
         'interfaces:', device.interfaces.filter(collect_status='active')) # hit dababase repeatly
```

- 执行逻辑变成：

  >1. "SELECT * FROM device"
  >2. "SELECT * FROM interface where device_id in (.....)"
  >3. 一直重复 device 的数量次： "SELECT * FROM interface where device_id = xx and collect_status='up';"
  >4. 最后通过 python 组合到一起。

- 原因在于：之前的 prefetch_related 查询，并不包含判断 collect_status 的状态。所以对于 QuerySet 来说，这是一个新的查询。所以会重新执行。

- 可以利用 [Prefetch 对象](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.Prefetch) 进一步控制并解决上面的问题：

```python
devices = Device.objects.prefetch_related(
    Prefetch('interfaces', queryset=Interface.objects.filter(collect_status='active'))
    ).filter()[:5] # hit twice database
for device in devices:
    print('device_name: ', device.name, 'interfaces:', device.interfaces) 
```

- 执行逻辑变成：

  >1. "SELECT * FROM device"
  >2. "SELECT * FROM interface where device_id in (.....) and collect_status = 'up';"
  >3. 最后通过 python 组合到一起。

- 可以通过 Prefetch 对象的 `to_attr`，来改变之间关联关系的名称：

```python
devices = Device.objects.prefetch_related(
    Prefetch('interfaces', queryset=Interface.objects.filter(collect_status='active'), to_attr='actived_interfaces')
    ).filter()[:5] # hit twice database
for device in devices:
    print('device_name: ', device.name, 'interfaces:', device.actived_interfaces) 
```

- 可以看到通过 Prefetch，可以实现控制关联那些有关系的对象。

- 最后，对于一些关联结构较为复杂的情况，可以将 prefetch_related 和 select_related 组合到一起，从而控制查询数据库的逻辑。

  比如，想要查询全部接口的信息，及其设备名称，以及拓展接口中绑定了对端设备和接口的信息。

```python
queryset = Interface.objects.select_related('ex_info').prefetch_related(
            'ex_info__endpoint_device_id', 'ex_info__endpoint_interface_id')
```

- 执行逻辑如下：

  >1. `SELECT XXX FROM interface LEFT OUTER JOIN interface_extension ON (interface.id=interface_extension .interface_id)`
  >2. `SELECT XXX FROM device where id in ()`
  >3. `SELECT XXX FROM interface where id in ()`
  >4. 最后通过 python 组合到一起。

第一步, 由于 interface 和 interface_extension 是 1 对 1 的关系，所以使用 select_related 将其关联起来。

第二三步：虽然 interface_extension 和 endpoint_device_id 和 endpoint_interface_id 是外键关系，如果继续使用 select_related 则会进行 4 张表连续 join，所以将其换成 prefetch_related. 对于 interface_extension 外键关联的属性使用 in 查询，因为interface_extension 表的属性并不是经常使用的。

## 4. 总结

- Django N +1 问题产生的原因，解决的方法就是通过调用 QuerySet 的 select_related 或 prefetch_related 方法。

- 对于 select_related 来说，应用场景主要在外键和一对一的关系中。对应到原生的 SQL 类似于 JOIN 操作。

- 对于 prefetch_related 来说，应用场景主要在多对一和多对多的关系中。对应到原生的 SQL 类似于 IN 操作。

- 通过 Prefetch 对象，可以控制 select_related 和 prefetch_related 和那些有关系的对象做关联。

- 最后，在每个 QuerySet 可以通过组合 select_related 和 prefetch_related 的方式，更改查询数据库的逻辑。