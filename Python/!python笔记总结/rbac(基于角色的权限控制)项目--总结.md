# rbac项目--  基于角色的权限控制--面试!!

## 1.如何实现权限控制？

- url代表权限

  

**1.1简单权限控制--表结构**

`简单权限控制,三个model,五张表`

**权限表**permission

- url   权限   url的地址 正则表达式  ^$ 
- title   标题

**角色表**role

- name  角色名称
- permissions   多对多   关联权限表    

**用户表**user

- username  用户名
- password  密码
- roles  多对都  关联角色

**角色和权限的关系表**

**用户和角色的关系表**



**1.2 一级菜单--表结构**

`一级菜单: 在permission表中增加is_menu字段,区别该权限是否是菜单`

**权限表**

- url   权限   url的地址 正则表达式  ^$ 

- title   标题

- is_menu   是否是菜单 

- icon 图标   

  

**1.3 二级菜单--表结构**

`二级菜单: 实现二级菜单功能时,需要增加第六张表menu,在权限表中加入外键,关联menu表,有menu_id   当前的权限是二级菜单  没有menu_id   当前的权限是普通的权限 `

**菜单表**

- title  一级菜单的名称
- icon  图标

**权限表**

- url   权限   url的地址 正则表达式  ^$ 

- title   标题

- menu   外键  关联菜单表    有menu_id   当前的权限是二级菜单  没有menu_id   当前的权限是普通的权限 

  

**1.4 二级菜单  对一级菜单进行排序--表结构**

`要对一级菜单排序,在menu中加入weight权重字段`

**菜单表**

- title  一级菜单的名称

- icon  图标

- weight  整型

  

**1.5 非菜单 权限归属**

`权限归属,在menu表中加入parent外键,自关联权限表, 有parent_id当前的权限是子权限,   没有parent_id 当前的权限是父权限 ,即二级菜单`

**权限表**

- url   权限   url的地址 正则表达式  ^$ 
- title   标题
- menu   外键-关联菜单表    有menu_id   当前的权限是二级菜单  没有menu_id   当前的权限是普通的权限 
- parent  外键-关联权限表-自关联,    有parent_id    当前的权限是子权限   没有parent_id    当前的权限是父权限 二级菜单



### --表结构:

- **menu 菜单表**

  - title 标题
  - icon 图标
  - weight 权限

- **permission 权限表**

  - url  权限,url路径,正则表达式 ^$
  - title 标题
  - name url的别名, 唯一
  - menu 外键,--关联菜单表,blank=True,null=True , **(二级菜单使用)**
    - 存在menu_id,当前的权限是二级菜单,
    - 没有menu_id当前的权限是普通权限
  - parent 自关联, **(非菜单权限归属使用)**
    - 有menu_id 的就是子权限
    - 没有parent_id就是父权限

  - *is_menu 布尔值,一级菜单使用的*
  - *icon,一级菜单使用的*

- **role 角色表**
  - name 角色的名称,
  - permission 多对多,关联权限表
- **user用户表**
  - username 用户名
  - password 密码
  - roles    多对多 关联角色表
- **role_permission角色与权限的关系表**
- **user_role 用户与角色的关系表**

### --数据结构(流程+技术点)

##### 1. 简单的权限控制 

- 登陆成功后保存权限信息到session中

  - **权限数据结构**

    ```python
    permission_list = [{url},]
    
    # url就是权限
    ```

- 中间件-

  `-(校验成功,return None,校验失败继续执行)`

  - 获取当前访问的url路径
  - 白名单校验
  - 登录状态的校验
  - 免认证的地址校验
  - 权限的校验
    - 从sission中获取权限
    - 循环权限,正则匹配

- 模板
  
  - 母版和继承

##### 2. 动态生成一级菜单 

- 登录成功保存用户权限到sission中

  - **权限数据结构**

    ```python
    permission_list=[{url},]
    
    ```

  -  **菜单数据结构**

    ```python
    menu_list=[{url:,title:,icon:,},]
    
    #url : 一级菜单的url
    #title: 一级菜单的 title
    ```

- 中间件

  `校验成功,return None,校验失败继续执行`

  - 获取当前访问的url路径
  - 白名单校验
  - 登录状态校验
  - 免认证路径的校验
  - 权限的校验
    - 从session中获取权限
    - 循环权限,正则匹配

- 母版

  - inclusion_tag
  - 动态生成一级菜单
    - 定义inclusion_tag
    - 一层for循环men_list,动态生成一级菜单

##### 3. 动态生成二级菜单

- 登录成功后保存用户权限到session

  - **权限列表数据结构**

    ```
    permission_list ={{url:,},}
    ```

  - **菜单字典数据结构**

    ```python
    menu_dict={一级菜单ID:{  # 一级菜单排序的时候使用一级菜单的id
        title:
        icon:
        children:[{
            url:
            title:
        },]
    },} 
    # 构造数据结构,有menu_id就是二级菜单,menu_id就是一级菜单的id
    ```

- 中间件

  - 获取当前访问的url路径
  - 白名单校验
  - 登录状态校验
  - 免认证路径的校验
  - 权限校验
    - 从session中获取权限
    - 循环权限,正则匹配

- 模板

  - 模板和继承

  - 动态生成二级菜单

    - 自定义inclusion_tag

    - 两层for循环menu_dict.values()

##### 4. 动态生成二级菜单(一级菜单排序) 

- 登录成功获取用户权限保存到sission中

  - **权限列表数据结构**

    ```
    permission_list= [{url},]
    ```

  - **菜单字典数据结构**

    ```python
    menu_dict = {一级菜单的id:{  #根据weight权重对一级菜单id即字典的键排序
    	title:
    	icon:
    	weight:    # 根据weight权重排序
    	children:[{
    		url:
    		title:
    	},]
    },}
    ```

- 中间件

  - 中间件
    - 获取当前访问的url路径
    - 白名单校验
    - 登录状态校验
    - 免认证路径的校验
    - 权限校验
      - 从session中获取权限
      - 循环权限,正则匹配

- 模板

  - 母版和继承

  - 生成二级菜单并对一级菜单排序

    - 自定义inclusion_tag
    - sorted对menu_diact字典排序,添加到有序字典od

    - 两层for循环,返回有序字典od.values(),

##### 5. 动态生成二级菜单(二级菜单默认选中,展开) 

- 登录成功后保存用户权限到session

  - **权限列表数据结构**

    ```python
    permission_list =  [{url}]
    ```

  - **菜单字典的数据结构**

    ```python
    menu_dict= {一级菜单id:{
    	title:
    	icon:
    	weight:
    	children:[{
    		url:
            title:
    	},]
    },}
    ```

- 中间件

  - 获取当前访问的url地址路径
  - 白名单校验
  - 登录状态检验
  - 免认证地址校验
  - 权限校验
    - 从session中获取权限
    - 循环权限,正则匹配

- 模板
  - 母版和继承
  - 动态生成二级菜单,并排序,展开
    - 自定义inclusion_tag
    - 获取当前url
    - sorted对menu_diact字典排序,添加到有序字典od
    - 循环一级菜单,加入class='hide'
    - 循环二级菜单,正则匹配url,如果匹配成功
      - 二级菜单加class='active'
      - 一级菜单class=''
    - 两层for循环,返回有序字典od.values()

##### 6. 动态生成二级菜单(非菜单权限归属,子权限选中二级菜单展开)

- 登录成功后保存用户权限到session

  - **权限列表数据结构**

    ```python
    permission_list = [{
    	url:
    	id:  # 当是二级菜单时没有pid,只有id,封装到
    	pid:  # 判断该权限是一个二级菜单还是子权限
    },]
    ```

  - **菜单字典数据结构**

    ```python
    menu_dict = {一级菜单id:{
    	title:
    	icon:
    	weight:
    	children:[{
    		url:
    		title:
    		id:  #判断request.current_menu_id与二级菜单中的id是否相等
    	},]
    },}
    ```

- 中间件

  - 获取当前访问的url路径
  - 白名单校验
  - request.current_menu_id  = None   *---当访问index免认证地址时,保证全部二级菜单闭合*
  - 登录状态的校验
  - 免认证的校验
  - 权限的校验
    - 从sission中获取取消权限
    - 循环权限,正则匹配
    - 获取权限列表中的id和pid
      - 当pid不存在时,当前权限是一个二级菜单,把id封装到request中,request.current_menu_id=id  *--current 当前--*
      - 当pid存在时,当前权限是一个二级菜单,把pid,即耳机菜单的id封装到requst中,request.current_menu_id=pid

- 模板

  - 母版与继承

  - 生成二级菜单,子权限选中二级菜单展开

    - 自定义inclussion_tag
    - 获取当前url
    - sorted 对一级菜单排序,添加到有序字典od
    - 循环一级菜单,加入class='hide'
    - 循环二级菜单,判断request.current_menu_id与二级菜单中的id是否相等
      - 相等时给该二级菜单加入class='active',移除一级菜单中的hide类,class=''

    - 两层for循环,返回od.values()

##### 7. 路径导航

- 登录成功保存用户权限到sission中

  - **权限字典数据结构**

    ```python
    permission_dict ={权限id:{   # 可以根据子权限的pid获取父(二级菜单)权限的字典
    	url:,
    	id:,
    	pid:,  
        title:,
    	},} 
    ```

  - **菜单字典数据结构**

    ```python
    menu_dict = {一级菜单id:{
    	title:
    	icon:
    	weight:
    	children:[{
    		url:
    		title:
    		id:  # 判断request.current_menu_id与二级菜单中的id是否相等
    	},]
    },}
    ```

- 中间件
  - 获取当前访问的url路径
  - 白名单校验
  - request.current_menu_id=None   *---当访问index免认证地址时,保证全部二级菜单闭合*
  - request.breadcrumb_list =[{'title':'主页','url':'/index/'},]   ---添加免认证信息到路径导航
  - 登录状态校验
  - 免认证地址校验
  - 权限校验
    - 从sission中获取权限
    - 循环权限permissions_dict.vlaues,正则匹配
    - 获取id和pid
      - 如果pid不存在是一个二级菜单,
        - request.current_menu_id = id
        - 封装该二级菜单信息--request.breadcurmb_list.append({'title':i['title'],'url':i['url']})
      - 如果pid存在,是一个子权限
        - 父权限p_permissions = permissions_dict[str('pid')]
        - request.current_menu_id = pid
        - 封装该二级菜单信息--request.breadcurmb_list.append({'title':p_permissions['title'],'url':p_permissions['url']})
        - 封装子权限菜单信息--request.breadcurmb_list.append({'title':i['title'],'url':i['url']})

- 模板

  - 母版和继承

  - 生成二级菜单,子权限选中二级菜单展开

    - 自定义inclussion_tag
    - 获取当前url
    - sorted 对一级菜单排序,添加到有序字典od
    - 循环一级菜单,加入class='hide'
    - 循环二级菜单,判断request.current_menu_id与二级菜单中的id是否相等
      - 相等时给该二级菜单加入class='active',移除一级菜单中的hide类,class=''

    - 两层for循环,返回od.values()

  - 路径导航
    - 自定义inclusion_tag
    - 获取breadcurmb_list= request.breadcrumb_list
    - 一层for循环 request.breadcrumb_list 

##### 8. 权限控制到按钮级别

- 登录成功之后保存用户权限到session

  - 权限字典数据结构

    ```python
    permisssions_dict={url别名name:{ # 以name作为字典的键,可以方便判断模版中传入的name是不是在权限表中
    	url:
        id:
        pid:
        title:
        pname:  # 获取父权限(二级菜单)信息,去做路径导航信息
    },}
    ```

  - 菜单字典数据结构

    ```
    menu_dict = {一级菜单id:{
    	title:
    	icon:
    	weight:
    	children:[{
    		url:
    		title:
    		id:
    	},]
    },}
    ```

- 中间件

  - 获取当前访问的url路径

  - 白名单校验
  - request.current_menu_id=None
  - request.breadcrumb_list=[{'tittle':'主页','url':'/index/'},]
  - 登录状态校验
    - 从sission中获取权限
    - 循环权限permissions_dict.vlaues,正则匹配
    - 获取id和pid
      - 如果pid不存在,当前url就是二级菜单
        - request.current_menu_id = id
        - request.breadcrumb_list.append({'title':i['title'],'url':i['url']})
      - 如果pid存在,当前url就是子权限
        - 获取父权限(二级菜单)信息  p_permissions = permissions_dict[i['pname']]
        - request.current_menu_id = pid
        - request.breadcrumb_list.append({'title':p_permissions['title'],'url':p_permissions['url']})
        - request.breadcrumb_list.append({'title':i['title'],'url':i['url']})

- 模板

  - 母版和继承

  - 生成二级菜单/一级菜单排序/非菜单权限归属,子权限选中二级菜单展开

    - 自定义inclussion_tag
    - 获取当前url
    - sorted 对一级菜单排序,添加到有序字典od
    - 循环一级菜单,加入class='hide'
    - 循环二级菜单,判断request.current_menu_id与二级菜单中的id是否相等
      - 相等时给该二级菜单加入class='active',移除一级菜单中的hide类,class=''

    - 两层for循环,返回od.values()

  - 路径导航

    - 自定义inclusion_tag
    - 获取breadcurmb_list= request.breadcrumb_list
    - 一层for循环 request.breadcrumb_list 

  - 权限控制到按钮

    - 自定义filter --has_permission
    - 判断前端传过来的name in permission_dict,  
      - 存在返回True
      - 不存在返回False
    - 在html文件中作判断{% if   request|has_permission: name   %}    { % endif %}

    

## ~~2. 流程+技术点~~

- ~~完成的django生命周期~~

~~![1563265113582](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1563265113582.png)~~

### ~~2.1.中间件校验~~

1. ~~**获取当前的访问的url地址**~~

2. ~~**白名单校验**~~

   - ~~re~~ 
   - ~~settings~~

3. ~~request.current_menu_id = None~~

4. ~~request.breadcrumb_list = [  { titel 首页  url :index } ]~~

5. ~~**验证登录状态**~~

   ​	~~没有登录跳转到登录页面~~

6. ~~**免认证地址的校验**~~

   - ~~re~~
   - ~~settings~~

7. ~~**权限的校验**~~

   1. ~~从session中获取当前用户的权限的信息~~ 

   2. ~~循环权限字典  正则匹配~~

      ~~匹配成功:~~

      ~~记录menu_id    breadcrumb_list 添加内容~~

      ~~获取权限id   pid~~ 

      ​	~~有pid 	当前访问的是子权限~~

      ​			~~request.current_menu_id = pid~~ 

      ​			~~p_permission = permission_dict[str(pid )]~~

      ​			~~request.breadcrumb_list.append( {  title    p_permission [’title ‘]    url :  p_permission [’url ‘] }  )~~

      ​			~~request.breadcrumb_list.append(  {  title  i[title]  url  i['url']  }  )~~ 

      ​	~~没有pid 	当前访问的是父权限    二级菜单~~

      ​		   ~~request.current_menu_id = id~~

       		  ~~request.breadcrumb_list.append(  {  title  i[title]  url  i['url']  }  )~~ 	

      ~~return~~

   3. ~~拒绝请求  没有权限~~

### ~~2.2.登陆的视图~~

1. ~~**验证用户名和密码  验证成功 获取到用户的对象**~~

2. ~~**权限信息和菜单信息的初始化**~~

   1. ~~**获取当前用户的权限信息**~~

      ~~`obj.roles.filer('permissions__url__isnull=False').vales('permissions__url').distinct()`~~

      - ~~跨表~~
      - ~~去空~~
      - ~~去重~~

   2. ~~**构建数据结构 权限   菜单**~~

      ~~**权限：**~~

      ​	~~简单的权限控制~~

      ​	~~permission_list = [  {  'url': url  } ]~~

      ​	~~非菜单权限的归属~~

      ​	~~permission_list = [  {  url   id   pid } ]~~

      ​	~~路径导航~~

      ​    ~~permission_dict = { 权限的id :{  url   id   pid   title }    }~~ 

      ~~**菜单：**~~

      ​	~~动态生成一级菜单~~

      ​	~~menu_list  = [  { url   title  icon   }]~~ 

      ​    ~~二级菜单~~

      ​	~~menu_dict = {~~

      ​			~~一级菜单的id:  {~~ 

      ​						~~title~~ 

      ​						~~icon~~

      ​						~~children ： [~~

      ​							~~{   url   title   }~~

      ​					~~]~~

      ​			~~}~~	

      ​	~~}~~

      ​    ~~二级菜单  一级菜单排序~~

      ​	~~menu_dict = {~~

      ​			~~一级菜单的id:  {~~ 

      ​						~~title~~ 

      ​						~~icon~~

      ​						~~weight~~

      ​						~~children ： [~~

      ​							~~{   url   title   }~~

      ​					~~]~~

      ​			~~}~~	

      ​	~~}~~

      ​	~~非菜单权限归属~~

      ​	~~menu_dict = {~~

      ​			~~一级菜单的id:  {~~ 

      ​						~~title~~ 

      ​						~~icon~~

      ​						~~weight~~

      ​						~~children ： [~~

      ​							~~{   url   title  id    }~~

      ​					~~]~~

      ​			~~}~~	

      ​	~~}~~

   3. ~~保存权限和菜单信息、登录状态到session中      json序列化  字典的key是数字的话 序列化后边字符串~~

   4. ~~重定向到首页~~

### ~~2.3.模板~~

- ~~母版和继承~~
- ~~动态生成菜单~~
  - ~~inclusion_tag~~
  - ~~有序字典~~
  - ~~sorted~~ 
  - ~~js   css~~ 
- ~~路径导航~~
  - ~~inclusion_tag~~
  -  ~~permission_dict = { 权限的id :{  url   id   pid   title }    }~~ 

## 3.rbac组件应用流程

- 写好的rbac组件应用到新项目流程

1. **拷贝rbac组件到新项目中，并注册**

   ```
   INSTALLED_APPS = [
   		...
       'rbac.apps.RbacConfig'
   ]
   ```

2. **数据库迁移**

   1. 修改用户表

      ```python
      class User(models.Model):
          """用户表"""
          # username = models.CharField('用户名', max_length=32)
          # password = models.CharField('密码', max_length=32)
          roles = models.ManyToManyField(Role, verbose_name='用户所拥有的角色', blank=True)
      	# 多对多关联写成类  不写字符串
          class Meta:
              abstract = True  # 数据库迁移时不生成表  作基类 继承使用
      ```

   2. 新项目的用户表继承User

      ```python
      from rbac.models import User
      class UserProfile(User):
      ```

   3. 执行数据库迁移的命令

      1. 先删除rbac中migrations中的除了init之外的py文件

      2. 改rbac中的Role,Role不能是字符串

      3. 执行 

         python manage.py makemigrations
      
         python manage.py migrate

3. **路由配置**

   ```
   urlpatterns = [
    	....
       url(r'^rbac/', include('rbac.urls')),
   ]
   ```

4. **角色管理**

   <http://127.0.0.1:8000/rbac/role/list/>

5. **一级菜单管理**

   <http://127.0.0.1:8000/rbac/menu/list/>

6. **批量操作权限**

   <http://127.0.0.1:8000/rbac/multi/permissions/>

   批量输入标题

   给权限分配给一级菜单  给父权限分配子权限

7. **分配权限**

   - 如果新项目用的不是rbac的User，那么rbac中views里 distribute_permissions替换User为当前使用的用户model
- 给角色分权限  <http://127.0.0.1:8000/rbac/distribute/permissions/>
  
- 给用户分角色
  
8. **应用权限的中间件**

   ```
   MIDDLEWARE = [
   	...
       'rbac.middlewares.middleware.AuthMiddleWare',
   
   ]
   ```

   - 在settings中添加权限的配置

   ```python
   #  白名单  存在inclusion时需要街上前缀
   WHITE_LIST = [
       r'/login/$',
       r'/reg/$',
       r'/admin/.*'   
   ]
   
   # 免认证的地址
   NO_PERMISSION_LIST = [
       r'/index/'
   
   ]
   
   # 权限的session的key
   PERMISSION_SESSION_KEY = 'permission'
   
   # 菜单的session的key
   MENU_SESSION_KEY = 'menu'
   ```

   - 登录成功后调用权限信息初始化的函数--登录时调用init_permission(request, obj)函数

   - ```
     from rbac.service.init_permission import init_permission
     init_permission(request, obj)
     ```

   - 

9. **动态生成二级菜单**

   ```
   {% load rbac %}
   {% menu request %}
   ```

​	使用 css js

```
<link rel="stylesheet" href="{% static 'rbac/css/menu.css' %}">
<script src="{% static 'rbac/js/menu.js' %} "></script>
```

10. **路径导航**

    ```
    {% breadcrumb request %}
    ```

11. **权限控制到按钮级别**

    ```
    {% load rbac %}
    
    
    {% if request|has_permission:'add_consult' %}
           <a href="{% url 'add_consult' customer_id %}" class="btn btn-primary">新增</a>
    {% endif %}
    ```

