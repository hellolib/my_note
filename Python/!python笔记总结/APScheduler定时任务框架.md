# APScheduler定时任务框架

## 1.简介

 	APScheduler是一个Python**定时任务框架**，提供了**基于日期**、**固定时间间隔**以及**crontab**类型的任务，并且可以**持久化任务**。基于这些功能，我们可以很方便的实现一个python定时任务系统。

## 2.安装

```python
pip install apscheduler
```

## 3.组成部分

**触发器**（triggers）：触发器包含调度逻辑，描述一个任务何时被触发，按日期或按时间间隔或按 cronjob 表达式三种方式触发。每个作业都有它自己的触发器，除了初始配置之外，触发器是完全无状态的。

**作业存储器**（job stores）：作业存储器指定了作业被存放的位置，默认情况下作业保存在内存，也可将作业保存在各种数据库中，当作业被存放在数据库中时，它会被序列化，当被重新加载时会反序列化。作业存储器充当保存、加载、更新和查找作业的中间商。在调度器之间不能共享作业存储。

**执行器**（executors）：执行器是将指定的作业（调用函数）提交到线程池或进程池中运行，当任务完成时，执行器通知调度器触发相应的事件。

**调度器**（schedulers）：任务调度器，属于控制角色，通过它配置作业存储器、执行器和触发器，添加、修改和删除任务。调度器协调触发器、作业存储器、执行器的运行，通常只有一个调度程序运行在应用程序中，开发人员通常不需要直接处理作业存储器、执行器或触发器，配置作业存储器和执行器是通过调度器来完成的。

## 4.操作作业

### 添加

​		add_job的第二个参数是trigger，它管理着作业的调度方式。它可以为date, interval或者cron。对于不同的trigger，对应的参数也相同。 

- add_job() 添加定时任务

```python
import time
 
from apscheduler.schedulers.blocking import BlockingScheduler

def my_job():
    print time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))

sched = BlockingScheduler()
sched.add_job(my_job, 'interval', seconds=5)
sched.start()
```

-  scheduled_job()修饰器 添加定时任务

```python
import time
from apscheduler.schedulers.blocking import BlockingScheduler
sched = BlockingScheduler()
 
@sched.scheduled_job('interval', seconds=5)
def my_job():
    print time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))
sched.start()
```

### 移除

```python
job = scheduler.add_job(myfunc, 'interval', minutes=2)
job.remove()
#如果有多个任务序列的话可以给每个任务设置ID号，可以根据ID号选择清除对象，且remove放到start前才有效

sched.add_job(myfunc, 'interval', minutes=2, id='my_job_id')
sched.remove_job('my_job_id')
```

### 暂停/恢复

```
apsched.job.Job.pause()
apsched.schedulers.base.BaseScheduler.pause_job()
```

### 获得job列表

获得调度作业的列表，可以使用`get_jobs()`来完成，它会返回所有的job实例。或者使用`print_jobs()`来输出所有格式化的作业列表。也可以利用get_job(任务ID)获取指定任务的作业列表 

```
job = sched.add_job(my_job, 'interval', seconds=2 ,id='123')
print sched.get_job(job_id='123')
print sched.get_jobs()
```

### 关闭调度器

 默认情况下调度器会等待所有正在运行的作业完成后，关闭所有的调度器和作业存储。如果你不想等待，可以将wait选项设置为False。

```python
sched.shutdown()
sched.shutdown(wait=False)
```

## 5.作业运行的控制

### corn定时调度

```python
(int|str) 表示参数既可以是int类型，也可以是str类型

(datetime | str) 表示参数既可以是datetime类型，也可以是str类型

 

year (int|str) – 4-digit year -（表示四位数的年份，如2008年）

month (int|str) – month (1-12) -（表示取值范围为1-12月）

day (int|str) – day of the (1-31) -（表示取值范围为1-31日）

week (int|str) – ISO week (1-53) -（格里历2006年12月31日可以写成2006年-W52-7（扩展形式）或2006W527（紧凑形式））

day_of_week (int|str) – number or name of weekday (0-6 or mon,tue,wed,thu,fri,sat,sun) - （表示一周中的第几天，既可以用0-6表示也可以用其英语缩写表示）

hour (int|str) – hour (0-23) - （表示取值范围为0-23时）

minute (int|str) – minute (0-59) - （表示取值范围为0-59分）

second (int|str) – second (0-59) - （表示取值范围为0-59秒）

start_date (datetime|str) – earliest possible date/time to trigger on (inclusive) - （表示开始时间）

end_date (datetime|str) – latest possible date/time to trigger on (inclusive) - （表示结束时间）

timezone (datetime.tzinfo|str) – time zone to use for the date/time calculations (defaults to scheduler timezone) -（表示时区取值）
```

- 某一定时时刻执行

```python
#表示2017年3月22日17时19分07秒执行该程序
sched.add_job(my_job, 'cron', year=2017,month = 03,day = 22,hour = 17,minute = 19,second = 07)

#表示任务在6,7,8,11,12月份的第三个星期五的00:00,01:00,02:00,03:00 执行该程序
sched.add_job(my_job, 'cron', month='6-8,11-12', day='3rd fri', hour='0-3')

#表示从星期一到星期五5:30（AM）直到2014-05-30 00:00:00
sched.add_job(my_job(), 'cron', day_of_week='mon-fri', hour=5, minute=30,end_date='2014-05-30')

#表示每5秒执行该程序一次，相当于interval 间隔调度中seconds = 5
sched.add_job(my_job, 'cron',second = '*/5')
```

![1571301599276](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1571301599276.png)

### interval 间隔调度

- 参数介绍

  ```python
  weeks (int) – number of weeks to wait
  days (int) – number of days to wait
  hours (int) – number of hours to wait
  minutes (int) – number of minutes to wait
  seconds (int) – number of seconds to wait
  start_date (datetime|str) – starting point for the interval calculation
  end_date (datetime|str) – latest possible date/time to trigger on
  timezone (datetime.tzinfo|str) – time zone to use for the date/time calculations
  ```

- 示例

  ```python
  #表示每隔3天17时19分07秒执行一次任务
  sched.add_job(my_job, 'interval',days  = 03,hours = 17,minutes = 19,seconds = 07)
  ```

  

### date 定时调度

- 作业只会执行一次

  ```python
  run_date (datetime|str) – the date/time to run the job at  -（任务开始的时间）
  
  timezone (datetime.tzinfo|str) – time zone for run_date if it doesn’t have one already
  ```

- 例子

  ```python
  # The job will be executed on November 6th, 2009
  
  sched.add_job(my_job, 'date', run_date=date(2009, 11, 6), args=['text'])
  
  # The job will be executed on November 6th, 2009 at 16:30:05
  
  sched.add_job(my_job, 'date', run_date=datetime(2009, 11, 6, 16, 30, 5), args=['text'])
  ```

  

