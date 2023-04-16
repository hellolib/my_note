# Django 缓存系统

## Django 缓存设置方法

```python
# django cache settings
CACHES = {
    'default': {
        # 'BACKEND': 'django.core.cache.backends.dummy.DummyCache', # 开发环境不缓存
        "BACKEND": "django_redis.cache.RedisCache",  # 使用django-redis的缓存
        "LOCATION": "redis://{}:6379/0".format(REDIS_HOST),  # redis数据库的位置
        'TIMEOUT': 3000,  # 缓存超时时间（默认300，None表示永不过期，0表示立即过期）
        'OPTIONS': {
            'MAX_ENTRIES': 300,  # 最大缓存个数（默认300）
            'CULL_FREQUENCY': 3,  # 缓存到达最大个数之后，剔除缓存个数的比例，即：1/CULL_FREQUENCY（默认3）
        },
        'KEY_PREFIX': 'sc_api_server_cache',  # 缓存key的前缀（默认空）
        'VERSION': 1,  # 缓存key的版本（默认1）
        # 'KEY_FUNCTION':                # 生成key的函数（默认函数会生成为：【前缀:版本:key】）
    }
}
```

### 参数说明

- BACKEND

  ```python
  # 选项后端缓存选项，根据不同的内容进行选择性填写，对应指定路径
  
  'django.core.cache.backends.db.DatabaseCache' # 数据库缓存
  'django.core.cache.backends.dummy.DummyCache' # 开发调试缓存
  'django.core.cache.backends.filebased.FileBasedCache' # 文件缓存
  'django.core.cache.backends.locmem.LocMemCache' # 内存缓存
  'django.core.cache.backends.memcached.MemcachedCache' # Memcache缓存（python-memcached模块）
  'django.core.cache.backends.memcached.PyLibMCCache' # Memcache缓存（pylibmc模块）
  "django_redis.cache.RedisCache" # Redis缓存
  
  ```

- **KEY_FUNCTION**

  ```python
  # 一个字符串，其中包含一个函数的点路径，该函数定义了如何将前缀，版本和密钥组合成最终缓存密钥
  def make_key(key, key_prefix, version):
      return ':'.join([key_prefix, str(version), key])
  
  ```

- **KEY_PREFIX**

  ```python
  # 默认值：（ ' ' 空字符串）
  # 一个字符串，它将自动包含（默认为前置）到Django服务器使用的所有缓存键中
  
  ```

- **LOCATION**

  ```python
  # 默认值：（ ' ' 空字符串）
  # 使用的缓存位置
  # 这可能是文件系统缓存的目录，内存缓存服务器的主机和端口，或者是本地内存缓存的标识名
  ```

- OPTIONS

  ```python
  # 默认值： None
  # 额外的参数传递给缓存后端。可用参数因您的缓存后端而异
  
  ```

- **TIMEOUT**

  ```python
  # 默认值： 300
  # 缓存条目被视为过时之前的秒数
  # 如果此设置的值为None，则缓存条目将不会过期
  ```

- **VERSION**

  ```python
  # 默认值： 1
  # Django服务器生成的缓存键的默认版本号
  
  ```

- CACHE_MIDDLEWARE_ALIAS

  ```python
  # 默认值： 'default'
  # 用于缓存中间件的缓存连接。
  
  ```

- CACHE_MIDDLEWARE_KEY_PREFIX

  ```python
  # 默认值：（ ' ' 空字符串）
  # 一个字符串，该字符串将作为由缓存中间件生成的缓存键的前缀
  # 该前缀与KEY_PREFIX设置结合在一起 ；它不会替代它
  
  ```

- **CACHE_MIDDLEWARE_SECONDS**

  ```python
  # 默认值：（ ' ' 空字符串）
  # 缓存中间件页面的默认秒数
  
  ```

  



### 1. Memcached

- Memcached是Django原生支持的缓存系统，速度快，效率高。

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
        'LOCATION': '127.0.0.1:11211',
    }
}

```

### 2. 数据库缓存

- 我们使用缓存的很大原因就是要减少数据库的操作，如果将缓存又存到数据库，岂不是脱…

```python


CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    }
}
```

### 3. redis缓存

- 推荐使用

```python
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

### 4.文件系统缓存

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
        'LOCATION': '/var/tmp/django_cache',
    }
}

```

### 5.基于本地内存的缓存

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
        'LOCATION': 'unique-snowflake',
    }
}
```

## 项目应用

### 1.全部项目缓存

```python
# 使用中间件，经过一系列的认证等操作
# 如果内容在缓存中存在，则使用FetchFromCacheMiddleware获取内容并返回给用户
# 当返回给用户之前，判断缓存中是否已经存在
# 如果不存在则UpdateCacheMiddleware会将缓存保存至缓存，从而实现全站缓存

MIDDLEWARE = [
    'django.middleware.cache.UpdateCacheMiddleware',
    # 其他中间件...
    'django.middleware.cache.FetchFromCacheMiddleware',
]
CACHE_MIDDLEWARE_ALIAS = ""
CACHE_MIDDLEWARE_SECONDS = ""
CACHE_MIDDLEWARE_KEY_PREFIX = ""

```

### 2.单独视图缓存

```python
# 方式一
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)
def my_view(request):
	......


# 方式二
from django.views.decorators.cache import cache_page

urlpatterns = [
    url(r'^foo/([0-9]{1,2})/$', cache_page(60 * 15)(my_view)),
]


# 方式三
from django.views.decorators.cache import cache_page
@cache_page(10)   #缓存10秒
def cache(request):
    import time
    time=time.time()
    return render(request,'cache.html',{'time':time,})

```

```python
# 前端模板，引入TemplateTag
{% load cache %}

# 前端模板，使用缓存
{% cache 5000 缓存key %}
    缓存内容
{% endcache %}

```



## Django缓存的实施方案

### 方案1:  cache_page 缓存

- 用法
  - 在视图添加装饰器 @cache_page(过期时间s)

- 优点: 
  - 简单粗暴 

- 缺点:
  - 无法细粒度缓存,无法针对性存储
  - 无法按照角色进行缓存(管理员或者访客)
  - 更新或者删除成本过高, 出现新旧数据不一致, 因为无法获取到django系统设置缓存的key,无法获取cache key 更新难度复杂

- 适用场景:
  - 简单缓存, 无角色认证, 



### 方案2  cache.set 局部缓存

- 用法 cache.set/get

- 优点: 
  - 灵活 
  -  存储成本最优 
  - 删除成本低

- 缺点:  
  - 代码实现成本高

- 底层方法

  ```python
  from django.core.cache import cache
  
  设置：cache.set(键,值,有效时间)
  获取：cache.get(键)
  删除：cache.delete(键)
  清空：cache.clear() 
  ```

  

### 方案3  cache.set + 装饰器

- 推荐使用

- 实现逻辑:

  - 自我实现带参数的装饰器进行缓存
  - 复用 cache.set 方法
  - 根据角色或者其他场景自定义缓存key
  - 自定义过期时间

  `每次数据库更新根据缓存key进行清理缓存`

- 实现方式:

  ```python
  def cache_set(expire_time):
    def _cache_set(func):
      def inner(request, *args,**kwargs):
        # 区分场景
        # 根据场景生产正确的cache key
        # 判断是不是有缓存
        	# 有缓存直接返回缓存
          # 无缓存执行视图->获取结果-> 设置缓存-> 返回结果
      return func(request, *args,**kwargs)
    return inner
  return _cache_set
  ```


- 底层方法

  ```python
  from django.core.cache import cache
  
  设置：cache.set(键,值,有效时间)
  获取：cache.get(键)
  删除：cache.delete(键)
  清空：cache.clear() 
  ```

  