### 1.展示客户信息

- 普通的字段
  
- 对象.字段名|过滤器
  
- 有choice参数的字段
  - 对象.字段名   — >数据库的数据
  - `对象.get_字段名_display()`      —>数据库的数据对应的中文提示,可以在模板和py文件中使用

- 外键

  - 对象.外键  —>  外键对象   可以自定义`__str__`,显示想要内容

  - 对象.外键.字段

- 自定义方法:

  - 多对多：

  ```python
  def show_class(self):
      return ' '.join([str(i) for i in self.class_list.all()])
  ```

- 自定义的需求：

  ```python
  def show_status(self):
      color_dict = {
          'signed': "green",
          'unregistered': 'red',
          'studying': 'blue',
          'paid_in_full': 'gold'
      }
      return mark_safe(
          '<span style="color: white;background: {};padding: 5px" >{}</span>'.format(color_dict.get(self.status),self.get_status_display()))
  ```

- 前端自定义日期时间

  - 第一种方法:使用时间过滤器 

    - |data:'Y-m-d'
    - |datatime:'Y-m-d H:i:s'

  - 第二种方法:在settings中更改全局配置

    - 把USE_L10N = False,然后 写入

      DATETIME_FORMAT='Y-m-d H:i:s'

      DATE_FORMAT = 'Y-m-d'

- save使用

  - 直接在模板文件中使用|save方法

  - 在视图文件导入save模块

    ```python
    form django.utils.safestring.import mark_safe
    
    def func()
    	return mark_safe('不转义字符串')
    ```

- str(对象)
  - 会调用该对象内的__ str __方法

- models中,字段属性verbose_name='用户名',字段显示中文名称

  在form表单中字段属性lable_name='用户名',字段显示中文名称

  ```python
  class UserProfile(models.Model):
      """
      用户表
      """
      username = models.EmailField(verbose_name='用户名', max_length=255, unique=True, )
  ```

  

### 2.分页 

见代码

### 3.git版本回退

git reset --hard 版本号               -->版本回退

git log 						-->查看日志

git reflog						-->查看日志详情

### 4. 克隆 推送 拉取

git clone https://gitee.com/old_boy_python_stack_21/teaching_plan.git  克隆远程仓库

git push origin master 推送本地代码到远程仓库

git pull origin master  拉取远程仓库代码到本地仓库

