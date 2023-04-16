# 5.23-40 数据库(四) 数据查询&pymysql

## 1.单表查询

### 1.1 关键字优先级

```python
#关键字优先级!!!!!!
from
where
group by
select
distinct(去重)
having
order by
limit
```

### 1.2 简单查询

```python
#简单查询
    SELECT id,emp_name,sex,age,hire_date,post,post_comment,salary,office,depart_id 
    FROM employee;
    SELECT * FROM employee;
    SELECT emp_name,salary FROM employee;

#避免重复DISTINCT
    SELECT DISTINCT post FROM employee;    

#通过四则运算查询
    SELECT emp_name, salary*12 FROM employee;
    SELECT emp_name, salary*12 AS Annual_salary FROM employee;
    SELECT emp_name, salary*12 Annual_salary FROM employee;

#定义显示格式
   CONCAT() 函数用于连接字符串
   SELECT CONCAT('姓名: ',emp_name,'  年薪: ', salary*12)  AS Annual_salary 
   FROM employee;
   
   CONCAT_WS() 第一个参数为分隔符
   SELECT CONCAT_WS(':',emp_name,salary*12)  AS Annual_salary 
   FROM employee;

#结合CASE语句：(了解)
   SELECT(CASE
           WHEN emp_name = 'jingliyang' THEN
               emp_name
           WHEN emp_name = 'alex' THEN
               CONCAT(emp_name,'_BIGSB')
           ELSE
               concat(emp_name, 'SB')
           END) as new_name
   FROM employee;
```

### 1.3 where约束

- 比较运算:;
  - 数学运算 ><  >=  <=   <>不等于   != 不等于  
  - 逻辑运算 not and or

- 范围筛选 

  - 逻辑运算与in, between 拼接
  -  in(80,90,100) 值是80或90或100 ---多选一
  - between 80 and 100 值在80到100之间 ---区间查询
  - like 'e%'  匹配e开头的字符串---字符串模糊查询
    - %通配符,匹配任意长度的任意内容
    - '%n%'含有n的字符

  - like 'e_'  匹配e开头的一个字 ``` _只表示一个字符```---字符串模糊查询
    - _通配符,匹配长度为1的任意内容
    - _ _ _通配符,匹配长度为3的任意内容
  - regexp 正则表达式  ---正则匹配  
    ```SELECT * FROM employee WHERE emp_name REGEXP 'm{2}';```

  ```python
  #对于null操作只能通过is
  is null 
  is not null
  ```

### 1.4 group by 分组(!!!)

- 执行优先级从高到低：where > group by > having 

- 总是根据会重复的项来进行分组
- 分组总是和聚合函数一起用

```python
SELECT post FROM employee GROUP BY post;
# 会把字段post里相同的值归为一组,并只显示改组的第一个,但是所有数据仍保存在组内
```

### 1.5 聚合函数(!!!)

聚合:把很多行的同一个字段进行一次统计,最终的得到一个结果;

- count(字段名)   --该字段出现次数

  - count(*)统计共有多少条数据

- sum(字段名)   --该字段值的和

- avg(字段名)   --该字段值的平均值

- min(字段名)   --该字段值的最小值  

- max(字段名)   --该字段值的最大值

  ```python
  #最高最低可以取到最大最小值,但是取极值的时候会把字段的值分组,所以取不到极值对应的id或者其他值,因为分组显示第一个值,需要使用多表操作;
  ```

### 1.6 分组聚合(!!!面试)

```python
#按照岗位分组，并查看每个组有多少人
select post,count(id) as count from employee group by post;
```

### 1.7  HAVING过滤(!!!)

```python
#！！！执行优先级从高到低：where > group by > having 
#1. Where 发生在分组group by之前，因而Where中可以有任意字段，但是绝对不能使用聚合函数。
#2. Having发生在分组group by之后，因而Having中可以使用分组的字段，无法直接取到其他字段,可以使用聚合函数
```

- 执行优先级从高到低：where > group by > having 

- hacing必须是跟group by一起用

  ```python
  #实例
  select post,avg(salary) from employee group by post having avg(salary) > 10000 and avg(salary) <20000;
  ```

### 1.8 ORDER BY 查询排序

- order by 某一个字段 asc;   --默认是升序asc,从小到大
- order by 某一个字段 desc;   --指定降排列,从大到小
- order by 第一个字段 asc,第二个字段 desc;  --指定先根据第一个字段升序排列，在第一个字段相同的情况下，再根据第二个字段排列

### 1.9 limit

- 取前n个  limit n   ==  limit 0,n
- 分页    limit m,n   从m+1开始取n个
- limit n offset m == limit m,n  从m+1开始取n个

### 小结

```小结
# select distinct 需要显示的列 from 表
#                             where 条件
#                             group by 分组
#                             having 过滤组条件
#                             order by 排序
#                             limit 前n 条
```

![1558608732994](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558608732994.png)

## 2.pymySQL模块

<https://www.cnblogs.com/Eva-J/articles/9772614.html>

- 第三方模块,操作数据库

```python
import pymysql

db = pymysql.connect("数据库ip","用户","密码","数据库" ) # 打开数据库连接
cursor = db.cursor(pymysql.cursors.DictCursor)       ## 使用 cursor() 方法创建一个游标对象cursor,pymysql.cursors.DictCursor使输出内容变成字典
cursor.execute("SELECT VERSION()")                    # 使用 execute() 方法执行 SQL 查询
db.commit()         								  # 删除更新时,提交到数据库执行
data = cursor.fetchone()                              # 使用 fetchone() 方法获取单条数据
#results = cursor.fetchall() 						  # 获取所有记录列表
print ("Database version : %s " % data)
db.close()                                            # 关闭数据库连接
```

```python
try:
   cursor.execute(sql)  # 执行sql语句
   db.commit()          # 执行sql语句
except:
   db.rollback()        # 发生错误时回滚
```

## 3.多表查询

<https://www.cnblogs.com/Eva-J/articles/9688383.html>

- 子查询和连表查询**优先使用连表查询,效率更高**

### 3.1 连表查询

- 先把两张表连在一起再查;

- **两张表连接方法**

  ```select 字段 from 表1 inner/left/right  join 表2 on 表1.字段=表2.字段;```

  - **内连接 inner join**

    ```python
  #两张表条件不匹配的项不会出现在结果中
    
    #找两张表共有的部分，相当于利用条件从笛卡尔积结果中筛选出了正确的结果
    #department没有204这个部门，因而employee表中关于204这条员工信息没有匹配出来
  mysql> select employee.id,employee.name,employee.age,employee.sex,department.name from employee inner join department on employee.dep_id=department.id; 
    +----+-----------+------+--------+--------------+
  | id | name      | age  | sex    | name         |
    +----+-----------+------+--------+--------------+
  |  1 | egon      |   18 | male   | 技术         |
    |  2 | alex      |   48 | female | 人力资源     |
  |  3 | wupeiqi   |   38 | male   | 人力资源     |
    |  4 | yuanhao   |   28 | female | 销售         |
    |  5 | liwenzhou |   18 | male   | 技术         |
    +----+-----------+------+--------+--------------+
  
    ```
  
  - **外连接**
  - **左外链接 left join**
  
  ```python
  #永远全量显示左表中的数据
      
  #以左表为准，即找出所有员工信息，当然包括没有部门的员工
  #本质就是：在内连接的基础上增加左边有右边没有的结果
  mysql> select employee.id,employee.name,department.name as depart_name from employee left join department on employee.dep_id=department.id;
      +----+------------+--------------+
    | id | name       | depart_name  |
      +----+------------+--------------+
    |  1 | egon       | 技术         |
      |  5 | liwenzhou  | 技术         |
      |  2 | alex       | 人力资源     |
      |  3 | wupeiqi    | 人力资源     |
    |  4 | yuanhao    | 销售         |
      |  6 | jingliyang | NULL         |
    +----+------------+--------------+
  ```
  
    - 右外连接 right join (了解)
    
      - 参考左连接
      
      ```python
      #永远全量显示右表中的数据
      ```
    
    - 全外链接(了解)
    
      

### 3.2 子查询

- 子查询是将一个查询语句嵌套在另一个查询语句中。
- 内层查询语句的查询结果，可以为外层查询语句提供查询条件。
- 子查询中可以包含：IN、NOT IN、ANY、ALL、EXISTS 和 NOT EXISTS等关键字
- 还可以包含比较运算符：= 、 !=、> 、<等

-----------------------------------------------------------------------------------------------------

- **带IN关键字的子查询**

  ```python
  #查询平均年龄在25岁以上的部门名
  select id,name from department
      where id in 
          (select dep_id from employee group by dep_id having avg(age) > 25);
  
  #查看技术部员工姓名
  select name from employee
      where dep_id in 
          (select id from department where name='技术');
  ```

-  **带比较运算符的子查询**

  ```python
  #比较运算符：=、!=、>、>=、<、<=、<>
  #查询大于所有人平均年龄的员工名与年龄
  mysql> select name,age from emp where age > (select avg(age) from emp);
  +---------+------+
  | name    | age  |
  +---------+------+
  | alex    | 48   |
  | wupeiqi | 38   |
  +---------+------+
  rows in set (0.00 sec)
  
  
  #查询大于部门内平均年龄的员工名、年龄
  select t1.name,t1.age from emp t1
  inner join 
  (select dep_id,avg(age) avg_age from emp group by dep_id) t2
  on t1.dep_id = t2.dep_id
  where t1.age > t2.avg_age;
  ```

- **带EXISTS关键字的子查询**

  EXISTS关字键字表示存在。在使用EXISTS关键字时，内层查询语句不返回查询的记录。
  而是返回一个真假值。True或False
  当返回True时，外层查询语句将进行查询；当返回值为False时，外层查询语句不进行查询

```python
#department表中存在dept_id=203，Ture
mysql> select * from employee
    ->     where exists
    ->         (select id from department where id=200);
+----+------------+--------+------+--------+
| id | name       | sex    | age  | dep_id |
+----+------------+--------+------+--------+
|  1 | egon       | male   |   18 |    200 |
|  2 | alex       | female |   48 |    201 |
|  3 | wupeiqi    | male   |   38 |    201 |
|  4 | yuanhao    | female |   28 |    202 |
|  5 | liwenzhou  | male   |   18 |    200 |
|  6 | jingliyang | female |   18 |    204 |
+----+------------+--------+------+--------+

#department表中存在dept_id=205，False
mysql> select * from employee
    ->     where exists
    ->         (select id from department where id=204);
Empty set (0.00 sec)
```