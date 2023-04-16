## 第十四章  Flask

#### 14.1 基本安装和使用

- `pip install Flask `
- `pycharm`直接下载安装，安装后的模块有：
  - `Flask ` 主要模块
  - `Jinja2`  模板语言  便于跟后端view交互，`temlplates`诞生于`jinja2`
  - `Markupsafe`  传递模板到前端渲染标签，另外防止攻击
  - `werkzeug` 承载Flask服务，类似`django`中的`wsgi`

- 使用：

  1. `pycharm`创建新的环境` Pure Python`

  2. 在新的环境中安装Flask 模块

  3. 包下新建文件，开始使用

     ```python
     from flask import Flask
     app = Flask(__name__)
     app.run()
     
     app.debug = True  #调试模式 不用每次手动刷新
     ```

#### 14.2 Flask中的Response 

1. `HttpResponse`

   ```python
   @app.route('/index')
   def index():
       return 'hello world i am Flask'
   ```

2. `render` 响应模板

   ```python
   @app.route('/index')
   def index():
       return render_template('index.html')
   ```

3. `redirect`

   - 重定向本质是 往响应体中添加响应行 location 

   ```python
   @app.route('/index')
   def index():
       return redirect('/index')
   ```

4. `send_file`  

   - 浏览器特性：返回文件内容，可以识别的比如`mp3,mp4,jpg`等（具体查看可以查看 响应体中的 `content-type`） 不能识别的会被直接下载
   - `content-type=audio`时，加载音频内容的时候，采用流媒体加载，即服务器一次回给30秒的视频，等到时间到继续发请求。目的是防止加载过慢，服务器负载大。

   ```python
   @app.route('/file')
   def get_file():
       return send_file('app.py')
   ```

 5. `jsonify`

    - 作用： 返回标准格式的`Json`字符串，先序列化`json`的字典，content-type中会变为`aplication/json`, `content-length`为字符长度

      ```python
      content-type:application/json
      content-length: 14 (算换行符)
      ```

    - 在Flask 1.1.1 版本中，可以直接返回字典格式，无需`Jsonify`。

    ```python
    @app.route('/json')
    def get_json():
        return {'xk': 'sb'}
    	#return jsonify({'xk': sb}) 
    ```

#### 14.3 Flask中的request

1. flask中的request是一个公共变量，在视图函数中无需进行传递

2. 基本方法：
   1. `request.method`  获取请求方式

   2. `request.form`  获取` FormData`中的数据，也就是所谓的form标签

      - 格式是 `immutablemuldict` 可以使用字典的所有提取方式
      - 也可以通过`to_dict（）`将其转换成普通字典

   3. `request.args`

      - 获取URL中的数据
      - 可以通过`to_dict（）`将其转换成普通字典

   4. `request.json`

      - 请求中 `Content-Type：application/json `请求体中的数据 被序列化到 `request.json `中 以字典的形式存放

   5. `request.data`

      - 请求中 Content-Type 中不包含 Form 或 `FormData` 保留请求体中的原始数据 b""  是一个错误的保存

   6. `request.files`  获取Form中的文件

      - `form`中仍然需要加 `enctype=multipart/form-data`

      ```python
      files = request.files.get('my_files')
      file_name = files.filename
      files.save(filename) 
      #以文件本命保存文件
      import os
      file_path = os.path.join(storage,file_name)
      files.save(file_path)   #保存到指定文件夹
      ```

   7. `request.path ` 请求路径 路由地址

   8. `request.url`  访问请求的完整路径包括` url`参数

   9. `request.host`  主机位 `127.0.0.1:5000 `

   10. `request.cookies`

   11. `request.values` 

       - 获取`url `和 `FormData `中的数据 敏感地带
       - 若参数相同，url的参数会替代form中的，一般只用于查看数据去向

#### 14.4 jinja2

1. 后端给前端数据时 ，在flask中

   ```python
   @app.route('/')
   def home():
       return render_template('stu.html', stu=列表/字典)
   ```

2. 对于字典在前端取值

   ```html
   stu.name   /  stu['name']  /stu.get('name')   都可以
   <-- 若stu是个大字典 -->
   {% for student in stu.items() %} 也可以
   ```

3. 若要传函数

   ```python
   @app.template_global()   #类似于django的 simple_tag
   def ab(a,b):
       return a+b
   @app.route('/')
   def home():
       return render_template('stu.html')
   
   <-->前端<-->
   {{ ab(1,4) }}
   ```

4. 前端直接写函数

   ```html
   {% macro my_input(na,ty) %}
   <input type='{{ ty }}' name='{{ na }}'>
   {% endmacro %}
   
   {{ my_input('username','text')}}  调用函数
   ```

5. 脚本创建标签传给后台

   ```python
   @app.template_global()
   def my_input(ty,na):
       s = f'<input type='{{ ty }}' name='{{ na }}'>'
       return Markup(s) #禁止前端转义
   
   <-->前端<-->
   {{ my_input(submit, username) }}
   
   ```

#### 14.5 Flask中的session

- 基于请求上下文管理机制
- 要想使用flask的session，必须设置` app.secret_key`， 作用是加盐
- flask中的session 没有专门的服务器的数据库来存放每个用户的session，其主要工作机制是：
  - 用户的cookie中保存着一段自己的类session信息
  - 服务器甚至更高规格保存着secret_key
  - 当用户登录后，服务器会将自己的secret_key和用户的登录状态所保存的session键值对进行一个加密（按位运算算法），并且序列化后返还给用户。
  - 当用户再次登录后，凭借之前服务器给的session, 交给服务器，服务器通过自己保存的secret_key和客户给的session 进行按位运算，将结果给到客户。
  - 实质上，服务器不在通过数据库保存用户的session键值对。而是保存的加密方式。只有将所谓的密文发送给服务器，服务器可以解析得出session键值对，此时解析后的键值对存放在服务器内存中。这种机制叫 交由客户端保管机制。

- 使用

  - 设置

  ```python
  session["user"] = 'xk'
  ```

  - 获取

  ```python
  res = session.get('user')
  ```

  

