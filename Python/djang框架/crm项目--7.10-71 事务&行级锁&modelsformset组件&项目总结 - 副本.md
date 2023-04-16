## 内容回顾

1.crm

客户关系管理系统

2.使用者

销售 班主任 助教 讲师  财务 BOSS 学生

3.业务+技术点

1.注册+登录+注销

用户表

- 注册

  - modelform

    ```python 
    from django import forms 
    from crm import models
    class RegForm(forms.Modelform):
        
        class Meta:
            model = models.UserProfile
            fields = '__all__'  # ['']
            exclude = []  
            
            labels = {
                'username':'用户名'
               
            }
            
        def clean_username(self):
            # 通过校验规则 返回当前字段的值
            # 不通过校验规则  抛出异常 
            
        def clean(self):
            self._validate_unique = True  # 检验唯一
            # 通过校验规则 返回当前所有的值
            self.add_error('re_password', '两次密码不一致')
            # 不通过校验规则  抛出异常 
            
        def __init__():
            
            
            
    使用：
    视图：
    	form_obj = RegForm()
        render(request,'reg.html',{'form_obj':form_obj})
        
        form_obj = RegForm(data=request.POST)
        form_obj.is_valid()
        form_obj.save()
        
        
    模板：
    	{{ form_obj.as_p }}
        
        {{ form_obj.username }}    ——》  # input 框
        {{ form_obj.username.label }}    ——》   # 中文提示
        {{ form_obj.username.id_for_label }}    ——》   # input 框的ID
        {{ form_obj.username.errors }}    ——》   # 一个字段所有的错误
        {{ form_obj.username.errors.0 }}    ——》  # 一个字段第一个的错误
        
        {{ form_obj.errors }}     ——》   # 表单所有的错误
        {{ form_obj.non_filed_errors }}     ——》   # __all__的错误
        
    
    ```

- 登录

  - 登录视图
    - 获取用户名和密码校验
    - 校验失败 返回登录页面 错误提示
    - 校验成功
      - 保存用户的pk和登录状态 到session中   user_id   is_login 
      - 跳转到index

  - 中间件
    - process_request
    - 白名单
    - 校验
      - 获取is_login 
        - 没有  跳转到登录页面
        - 有
          - 从session中获取到user_id 查询用户对象
          - 把用户对象保存到request.user_obj（不建议使用user）

2.客户的管理

- 展示客户

  - 展示不同的字段
    1. 普通字段  对象.字段名
    2. choice      `对象.get_字段名_display()`
    3. 外键       对象.外键字段   给关联的对象的类中定义`__str__`
    4. 自定义方法
       1. 多对多    
       2. html     safe   mark_safe 

  - 公户和私户 

  - 分页 

    - page=1    start =(page-1) * per_num  start =page * per_num 

  - 模糊搜索

    - Q   

      ```python 
       q = Q() 
       q.connector = 'OR' 
       q.children.append( Q(('{}__contains',query)) )
      ```

- 新增和编辑客户

  modelform

  两个URL  一个视图函数  一个模板

  视图函数;

  ```python
  新增：
  	get   form_obj = CustomerForm()
      post  form_obj = CustomerForm(request.POST)
      	  form_obj.is_valid()
            form_obj.save()
  编辑：
  	get   form_obj = CustomerForm(instance=obj)
      post  form_obj = CustomerForm(request.POST,instance=obj)
      	  form_obj.is_valid()
            form_obj.save()
  
  ```

  模板：

  ```html
  {% for field in form_obj %}
  	
  	{{ field }}   {{ field.label }}   {{ field.id_for_label }}  {{ field.errors.0 }} 
  		
  {% endfor %}
  
  {{ field.non_field_errors.0 }} 
  ```

- 公户和私户的转换

  反射 + ORM 

3.跟进记录的管理

- 展示跟进记录  
  - 展示销售所有的跟进记录
  - 展示某个客户的所有的跟进记录
- 新增和编辑跟进记录
  - modelform
  - 修改字段的choices结果
    - 限制跟进人为当前的用户
    - 限制客户为当前用户的私户    

4.报名表的管理    

## -----------------

1.公户变私户

## 行级锁

数据库

```sql
begin； 开启事务

select * from t6 where f1 = 23.46 for update;   加行级锁

commit； 结束事务
```

django：

```python
from django.db import transaction

try: #开启事务
    with transaction.atomic():

        queryset = models.Customer.objects.filter(pk__in=pk, consultant=None).select_for_update() #加行级锁
        queryset.update(consultant=self.request.user_obj)
except Exception as e: 
	print(e)



```

## **私户的上限**

settings：

```python
MAX_CUSTOMER_NUM = 3  # 配置写大写
```

views：

```python
from Aida_crm.settings import MAX_CUSTOMER_NUM
from django.conf import settings
settings.MAX_CUSTOMER_NUM
```

3.班主任

- 班级的管理

- 课程记录的管理

- 学习记录（上课记录）的管理

  批量插入

  ```python
  study_record_list = [] 
  for student in students:
      	study_record_list.append(models.StudyRecord(course_record=course_record,student=student))
      # bulk_create批量创建 ,study_record_list对象列表
  models.StudyRecord.objects.bulk_create(study_record_list,batch_num = 10)#当batch_num参数存在时,每次插入数量,如果不存在,内一次全部插入
  ```



form

modelform

## modelformset

```python 
from django.forms import modelformset_factory
ModelFormSet = modelformset_factory(models.StudyRecord, StudyRecordForm, extra=0)
    form_set_obj = ModelFormSet(queryset=models.StudyRecord.objects.filter(course_record_id=course_record_id))
    


{{ form.instance  }}     StudyRecord对象
{{ form.attendance }}    _>  input select框

编辑
{{ form_set_obj.management_form }}
{{ form.id }}

ModelFormSet = modelformset_factory(models.StudyRecord, StudyRecordForm, extra=0)
    form_set_obj = ModelFormSet(queryset=models.StudyRecord.objects.filter(course_record_id=course_record_id),data=request.POST)
    
    
form_set_obj.is_valid()  对提交的数据进行校验
form_set_obj.save()  	保存修改

```



