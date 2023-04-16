## 1.Flask重点总结

```python
#Flask 安装依赖包及作用
- jinja2 模板语言  (flask依赖包)
- markupsafe   防止css攻击  (flask依赖包)
- werkzeug  --wkz 类似于django中的wsgi,承载服务  (flask依赖包)
```

### 1.1Flask启动

```python
# 三行启动
from flask import Flask
app = Flask(__name__)
app.run("0.0.0.0",9527)  #监听地址,监听端口
```

### 1.2 Response

1. return  "字符串"    -->httpresponse
2. return  render_template('html文件',变量='字符串')     -->默认存储位置是remplates,返回模板,依赖markupsafe包
3. return redirect("/login")    -->重定向   在响应头中加入-location:url地址,然后跳转

- 特殊的Response

4. return  send_file("文件名称或者文件路径+文件名称")   -->打开文件,返回文件你内容,自动识别文件类型,在响应头中加入content-Type:文件类型

5. return jsonify({k:v})   -->返回标准格式的json字符串,在响应投中加入content-Type:appcation/json
   
   - 在Flask 1.1.1版本中,直接返回dict时,本质上是在运行jsonify
   
   - 在Flask1.1.1中,可以直接返回字典,flask默认先把字典序列化之后再返回,在content-type中加入application/json

### 1.3 FLask Request

- request.from  获取FormData中的数据(类似Djnago中的request.POST)
  - to_dict() 转化为类似字典类型,可以通过get和[key]取值,[key],**当key不存在时,出现KeyError错误**
- request.args  获取url中的数据(类似Djnago中的request.GET)
  - to_dict() 转化为类似字典类型,可以通过get和[key]取值,[key],**当key不存在时,出现KeyError错误
- request.files  获取Fome中上传文件:
- **request.json**  
  - 如果请求中存在 Content-Type：application/json ,那么请求体中的数据 被序列化到 request.json 中,并且以 以字典的形式存放
- **request.data**
  - 如果请求中 存在Content-Type 中不包含 Form 或 FormData ,保留请求体中的原始数据,是base类型 b"  ".存放

- request.method  #请求方法
- request.host  #主机位: 127.0.0.1:5000
- request.url   #完整的url,包括参数
- request.path  #请求路径,路由地址
- request.cookies   #字典获取浏览器请求时带上的cookie
- request.files  # request文件,返回filestorage,.save保存文件
- request.heads  获取请求头

### 1.4 Flask session

- 交由客户端保管机制,客户端保存的是一串加密字符串,保存在服务器的内存中

- 设置session

  ```python
  1.在Flask中设置密钥 
  app.secret_key = "@#^%&*&@^&%^#*@"
  或者:
  app.config['SECRET_KEY'] = "@#^%&*&@^&%^#*@"
  2.设置session
  session["user"]="123"
  3.获取session
  session.get('user')
  
  
  # 交由客户端保管机制
  1.登陆成功后设置session,Flask根据密钥和要设置的session键值对经过位运算,生成session,保存到内存中,需要的话就通过get获取,不需要关闭时删除,并在浏览器中设置键值对--session:string
  2.在发出请求时,根据浏览器中的--session:string和Flask中secret_key,反序列化就得到了session键值对
  ```

### 1.5 Flask 路由 

```python
from flask import Flask  
app = Flask(__name__)  

@app.route('/',methods=("POST","GET"),endpoint='别名',defaults={"count":20} ) 
def home(count): 			
count = request.args.get('count',count)  #指定页数就优先,如果没有取默认值

app.run()
```

- rule= '/'   第一个时url,

- methods= ["GET","POST",..]   支持的请求方式(get查询,post增加,put更新,delete删除,options跨域)

- endpoint 别名   * 同一个项目中别名不能重复,默认值是视图函数名,

  ```
  Mapping,别名.
  反向解析  url = url_for('别名')
  ```

- strict_slashes  = True  严格遵守路由匹配

-  defaluts  默认参数

- redirect_to  ='/'   永久重定向,不管url是什么都会跳转到'/'

  - 应用场景: 地址更换时,点击原来地址跳转到新地址

  ```python
  #添加路由时不一定用装饰器,可以使用 
  app.add_url_rule(rule,  # 路由地址
                   view_func  #视图函数
                  )
  ```

### 1.6 动态参数路由

- 可以分页,获取文件,解决分类,解决正则路由问题

```python
from flask import Flask  
app = Flask(__name__)  

@app.route('/home/<page>')  #page默认类型是字符串接收 
def home(page):   # 需要传参接收			
    pass  

@app.route('/home/<page>_<id>_<id2>')  #默认类型是字符串接收 
def home(page,id,id2):   # 需要传参接收			
    pass 

app.run() 
```

### 1.7 Flask初始化配置##重要!!!

```python
from flask import Flask  
app = Flask(
    __name__,
    template_folder = 'templates',   # 更改母版存放路径,默认值是templates  ##重要!!!
    static_folder = 'static',  # 指定静态文件保存目录,默认值是static    "家"  ##重要!!! 
    static_url_path = "/static",  # 静态文件访问路径,默认值是 /+static_folder  "回家的路"  ##重要!!!
    
)  

#原理
@app.route('/<filename>', )
def func(filename):
    filepath = os.path.join('img', filename)  # img就是家
    return send_file(filepath)  				# filepath就是访问路径


if __name__ == '__main__':
	app.run()
```

### 1.8 Flask实例配置

- 基本配置

```python
from flask import Flask  
app = Flask(
    __name__,
    template_folder = 'templates',   # 更改母版存放路径,默认值是templates  ##重要!!!
    static_folder = 'static',  # 指定静态文件保存目录,默认值是static    "家"  ##重要!!! 
    static_url_path = "/static",  # 静态文件访问路径,默认值是 /+static_folder  "回家的路"  ##重要!!!
    
)  

# 实例化配置
app.debug = True # 修改代码自动重启
app.secret_key = '$%^^$'   #设置session值需要改密匙
app.session_cookie_name = 'session'  # 设置的session名称 ,默认是session
app.permanent_session_lifetime=  # session生命周期,以秒计时,默认31天

# 另外一种更改配置方式
app.config['DEBUG']= True  # 这种方式速度更快

#app.config   Flask配置
#app.defalut_config     flask默认配置

if __name__ == '__main__':
	app.run()
```

- **settings文件配置**(重要!!!),快速切换工作模式

```python
# settinigs.py文件代码

class DebugConfig(object):
    """线上开发环境"""
    DEBUG = True
    SECRET_KEY = "#$%^&*($#$%^&*%$#$%^&*^%$#$%"
    PERMANENT_SESSION_LIFETIME = 3600
    SESSION_COOKIE_NAME = "I am Not Session"


class TestConfig(object):
    """测试环境"""
    TESTING = True
    SECRET_KEY = hashlib.md5(f"{time.time()}#$%^&*($#$%^&*%$#$%^&*^%$#$%{time.time()}".encode("utf8")).hexdigest()
    PERMANENT_SESSION_LIFETIME = 360000
    SESSION_COOKIE_NAME = "#$%^&*($#$%^&*%$#$%^&*^%$#$%"   # session名字
    
    
    
#配置生效
1.导入配置文件
from settings import DebugConfig,TestConfig
2.环境生效
app.config.from_object(DebugConfig)  # 线上环境
app.config.from_object(TestConfig)	# test环境,需要的时候只需要启用DebugConfig,TestConfig其中一条
```

### 1.9 Flask蓝图 Blueprint

- Blueprint,类似普通的Flask实例,不能被run的Flask实例,不存在config

1. 创建蓝图bp_users.py文件,名字可以更改

   ```python
   from flask import Blueprint
   
   bp = Blueprint('bp01', __name__,url_prefix='url前缀')  # 'bp01'第一个参数是唯一标识,整个环境不能重复!  url_prefix='url前缀',当存在多个蓝图url冲突时,在地址栏输入'url前缀',就可以访问指定的蓝图文件
   
   
   @bp.route('/user',endpoint='user')
   def user():
       return '我是蓝图函数01'
   ```

2. 建立关系

   ```python
   # 在项目的app.run文件中
   from app01 import bp  #导入蓝图
   app.register_blueprint(bp) # 注册蓝图
   ```

3. 访问指定路径

   - 在蓝图中反向解析时,需要注意书写格式:

     ```python
     url = url_for('蓝图标识.装饰器别名')
     ```

### 1.10特殊装饰器

`类似于django中的中间件`

- @app.before_request   
  - 在请求进入视图函数之前,顺序执行,做出处理
  - 类似dganjo中的request中间件
  - 执行规律django中的request中间件一样
- @app.after_request
  - 在视图函数之后执行,倒序执行,做出处理
  - 类似django中的response中间件
  - 与django中间件不同的是:after_request不管什么情况,只要有响应都会倒序全部执行
- @app.errorhandler(错误码)
  - 错误监听装饰器
  - 错误码只能是4(请求错误)或5(服务器错误)开头的
  - 可以重定义错误界面

```python
@app.before_request
def is_login():
    """
    校验登录状态
    :return:
    """
    path = request.path
    if path != '/login':
        if not session.get('is_login'):
            return redirect('/login')
    return None


@app.errorhandler(404)
def error(error_msg):  # 形参必须添加
    """
    校验登录状态
    :return:
    """
    return '没找到页面'
```

### 1.11CBV

- 基本格式

  ```python
  from flask import view,Flask
  app = Flask(__name__)
  
  class Login(views.MethodView):    #继承MethodView,使类变成视图函数
      def get(self,*args,**kwargs):
          pass
      def post(self,*args,**kwargs):
          pass
  app.add.url_rule('/login',
                  endpoint='login',  # 如果endpoint不定义的话名称就是view_func的name,必须唯一
                  view_func=Login.as_view(name='loginlogin'),  # name就是就是view_func的名称
                  )    
      
  app.run()
  ```

## 2.第三方组件Flask-Session

- setdefault()  字典的方法

  ```python
  dict.setdefault(key, default=None)
  #参数
  key -- 查找的键值。
  default -- 键不存在时，设置的默认键值。
  #返回值
  如果 key 在 字典中，返回对应的值。如果不在字典中，则插入 key 及设置的默认值 default，并返回 default ，default 默认值为 None。
  ```

  

- 安装Flask-session包

  - app.session_interface  就是默认的session接口,Flask利用session_interface选择session的存放位置和存放机制.

  ```python
  from flask import Flask, request, session
  from flask_session import Session  #导入Session
  from redis import Redis  # 导入redis
  
  app = Flask(__name__)
  app.secret_key = '$%^&*%$'  #  flask_session使用pickle转化,密钥可以不使用
  app.config['SESSION_TYPE'] = 'redis'  # 设置session存放机制,,浏览器中存的就是session_id,session存在redis中
  app.config['SESSION_REDIS'] = Redis(host='192.168.12.10', port=6379, db=10)
  Session(app)  # 使普通sesson变成flask_session
  
  # app.session_interface    #Flask利用app.session_interface 选择session存放位置和存放机制
  
  @app.route('/set')
  def sets():
      session['key'] = 'QWER'
      return 'set'
  
  @app.route('/get')
  def gets():
      return session.get('key')
  
  app.run()
  ```







## 3.Flask请求上下文管理

### 3.1 偏函数

- partial   使用该方式可以生成一个新函数

  ```python
  from functools import partial
   
  def mod( n, m ):
    return n % m
   
  mod_by_100 = partial( mod, 100 )  # 100传给n
   
  print mod( 100, 7 )  # 2
  print mod_by_100( 7 )  # 2
  ```

### 3.2 线程安全

```python
import time
from threading import local

class Foo(local): 
# 继承local,保证线程安全,也保证了处理速度,threading.local()这个方法的特点用来保存一个全局变量，但是这个全局变量只有在当前线程才能访问，
    num = 0
    
foo = Foo()
def add(i):
    foo.num =i
    time.sleep(0.5)
    print(foo.num)

from threading import Thread
for i in range(20):
    task = Thread(target=add,args=(i,))
    task.start()
    
```

### 3.3 请求上文 

app.run()

### 3.4 请求下文

request.method

- callable(local)  判断local是不是可执行对象