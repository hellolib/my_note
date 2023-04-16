# rbac项目--  基于角色的权限控制--面试!!

## 1.如何实现权限控制？

- url代表权限

### 1.1简单权限控制--表结构

**权限表**

- url   权限   url的地址 正则表达式  ^$ 
- title   标题

**角色表**

- name  角色名称
- permissions   多对多   关联权限表    

**用户表**

- username  用户名
- password  密码
- roles  多对都  关联角色

**角色和权限的关系表**

**用户和角色的关系表**

### 1.2 一级菜单--表结构

**权限表**

- url   权限   url的地址 正则表达式  ^$ 
- title   标题
- is_menu   是否是菜单 
- icon 图标   

### 1.3 二级菜单--表结构

**菜单表**

- title  一级菜单的名称
- icon  图标

**权限表**

- url   权限   url的地址 正则表达式  ^$ 
- title   标题
- menu   外键  关联菜单表    有menu_id   当前的权限是二级菜单  没有menu_id   当前的权限是普通的权限 

### 1.4 二级菜单  对一级菜单进行排序--表结构

**菜单表**

- title  一级菜单的名称
- icon  图标
- weight   整形    

### 1.5 非菜单 权限归属

**权限表**

- url   权限   url的地址 正则表达式  ^$ 
- title   标题
- menu   外键  关联菜单表    有menu_id   当前的权限是二级菜单  没有menu_id   当前的权限是普通的权限 
- parent  外键 关联权限表  子关联    有parent_id    当前的权限是子权限   没有parent_id    当前的权限是父权限 二级菜单

## 2. 流程+技术点

- 完成的django生命周期

![1563265113582](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1563265113582.png)

### 2.1.中间件

1. **获取当前的访问的url地址**

2. **白名单校验**

   - re 
   - settings

3. request.current_menu_id = None

4. request.breadcrumb_list = [  { titel 首页  url :index } ]

5. **验证登录状态**

   ​	没有登录跳转到登录页面

6. **免认证地址的校验**

   - re
   - settings

7. **权限的校验**

   1. 从session中获取当前用户的权限的信息 

   2. 循环权限字典  正则匹配

      匹配成功

      记录menu_id    breadcrumb_list 添加内容

      获取权限id   pid 

      ​	有pid 	当前访问的是子权限

      ​			request.current_menu_id = pid 

      ​			p_permission = permission_dict[str(pid )]

      ​			request.breadcrumb_list.append( {  title    p_permission [’title ‘]    url :  p_permission [’url ‘] }  )

      ​			request.breadcrumb_list.append(  {  title  i[title]  url  i['url']  }  ) 

      ​	没有pid 	当前访问的是父权限    二级菜单

      ​		   request.current_menu_id = id

       		  request.breadcrumb_list.append(  {  title  i[title]  url  i['url']  }  ) 	

      return

   3. 拒绝请求  没有权限

### 2.2.登陆的视图

1. **验证用户名和密码  验证成功 获取到用户的对象**

2. **权限信息和菜单信息的初始化**

   1. **获取当前用户的权限信息**

      `obj.roles.filer('permissions__url__isnull=False').vales('permissions__url').distinct()`

      - 跨表
      - 去空
      - 去重

   2. **构建数据结构 权限   菜单**

      **权限：**

      ​	简单的权限控制

      ​	permission_list = [  {  'url': url  } ]

      ​	非菜单权限的归属

      ​	permission_list = [  {  url   id   pid } ]

      ​	路径导航

      ​    permission_dict = { 权限的id :{  url   id   pid   title }    } 

      **菜单：**

      ​	动态生成一级菜单

      ​	menu_list  = [  { url   title  icon   }] 

      ​    二级菜单

      ​	menu_dict = {

      ​			一级菜单的id:  { 

      ​						title 

      ​						icon

      ​						children ： [

      ​							{   url   title   }

      ​					]

      ​			}	

      ​	}

      ​    二级菜单  一级菜单排序

      ​	menu_dict = {

      ​			一级菜单的id:  { 

      ​						title 

      ​						icon

      ​						weight

      ​						children ： [

      ​							{   url   title   }

      ​					]

      ​			}	

      ​	}

      ​	非菜单权限归属

      ​	menu_dict = {

      ​			一级菜单的id:  { 

      ​						title 

      ​						icon

      ​						weight

      ​						children ： [

      ​							{   url   title  id    }

      ​					]

      ​			}	

      ​	}

   3. 保存权限和菜单信息、登录状态到session中      json序列化  字典的key是数字的话 序列化后边字符串

   4. 重定向到首页

### 2.3.模板

- 母版和继承
- 动态生成菜单
  - inclusion_tag
  - 有序字典
  - sorted 
  - js   css 
- 路径导航
  - inclusion_tag
  -  permission_dict = { 权限的id :{  url   id   pid   title }    } 

## 3.应用