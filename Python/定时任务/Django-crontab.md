# Django Crontab 定时任务

## 一. 方案比对

### 1. celery

- celery 异步任务功能强大, 在小项目不建议用，杀鸡用牛刀。

### 2. apscheduler  (推荐使用)

- 简单环境下，推荐使用, Django中使用会有问题.

- 安装
   `pip install apscheduler`
- 使用

```python
from apscheduler.schedulers.background import BackgroundScheduler
scheduler = BackgroundScheduler()
scheduler.add_job(test.job, 'interval', minutes=1)
scheduler.start()
```

- 优缺点:
   和schedule类似, 是schedule的加强版, 帮用户已经封装了线程，使用比schedule方便

> 参考: https://apscheduler.readthedocs.io/en/latest/index.html

### 3. schedule

- 安装
   `pip install schedule`
- 使用

```swift
import schedule
import time

def job():
    print("I'm working...")

schedule.every(10).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every(5).to(10).minutes.do(job)
schedule.every().monday.do(job)
schedule.every().wednesday.at("13:15").do(job)
schedule.every().minute.at(":17").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

- 优缺点
   优点：简单, 不依赖django，python都可以用
   缺点:  在django环境需要另起线程

> 参考: https://pypi.org/project/schedule/

### 4. django-crontab (推荐使用)

- 安装
   `pip install django-crontab`

- 优缺点:
   运行和django无关，依赖的是linux的crontab定时服务，因此无法在windowns下运行。

> 参考: https://pypi.org/project/django-crontab/



## 二. Django-crontab 

**!!运行和django无关，依赖的是linux的crontab定时服务。**

### 1. Django-crontab 安装使用

- 安装
   `pip install django-crontab`
- 部署
   在django项目的settings里添加如下:

```python
INSTALLED_APPS = (
    'django_crontab',
    ...
)
```

- 创建定时任务
  - 此处打印的内容不会出现在控制台, 只会输出到日志文件中

```python
def my_scheduled_job():
  print('定时任务') # 此处打印的内容不会出现在控制台, 只会输出到日志文件中
```

- 配置定时任务
  - 在django项目的settings里添加如下:

```python
# 定时任务
# 中文乱码
CRONTAB_COMMAND_PREFIX = 'LANG_ALL=zh_cn.UTF-8'
# 每项工作执行后 --直译
CRONTAB_COMMAND_SUFFIX = '2>&1' 
# 添加定时任务(函数中的输出语句,是输出在.log文件中的)
CRONJOBS = (
    ('*/1 * * * *',  # 每分钟执行定时任务
     'apps.crontab_container.tasks.test.run',   # 必须是项目app下,否则报错
     '>> /Users/liusaisai/workspace/my_project/sichuan_pro/sc_api_server/log/log.log'),
)

```

- 启动或者修改后重新启动 定时任务
   `python manage.py crontab add`
- 显示定时任务
   `python manage.py crontab show`
- 删除定时任务
   `python manage.py crontab remove`

### 2. Linux crontab

```python
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 6) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)


# 示例
0 */2 * * * /sbin/service httpd restart  意思是每两个小时重启一次apache 

50 7 * * * /sbin/service sshd start  意思是每天7：50开启ssh服务 

50 22 * * * /sbin/service sshd stop  意思是每天22：50关闭ssh服务 

0 0 1,15 * * fsck /home  每月1号和15号检查/home 磁盘 

1 * * * * /home/bruce/backup  每小时的第一分执行 /home/bruce/backup这个文件 

00 03 * * 1-5 find /home "*.xxx" -mtime +4 -exec rm {} \;  每周一至周五3点钟，在目录/home中，查找文件名为*.xxx的文件，并删除4天前的文件。

30 6 */10 * * ls  意思是每月的1、11、21、31日是的6：30执行一次ls命令
20 03 * * * . /etc/profile;/bin/sh /var/www/runoob/test.sh > /dev/null 2>&1 
```

- 秒级控制实现

```python
(
  '* * * * * ',
  'apps.crontab_container.tasks.test.run',
  '>> {}/log/test.log'.format(BASE_DIR)
),
( # s级控制实现
  '* * * * * sleep 10;',
  'apps.crontab_container.tasks.test.run',
  '>> {}/log/test.log'.format(BASE_DIR)
),
```

