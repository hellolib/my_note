# MySQL 视图、触发器、函数、存储过程

## 1.视图

- 视图的概念

  ```
  视图就是一条 select 语句执行后返回的结果集。
  ```

- 视图的特性

  ```
  视图是对若干张基本表的引用，一张虚表，查询语句的执行结果，不存储具体的数据（基本表数据发生了改变，视图也会跟着改变）
  ```

-  **视图的作用** 

  ```
  方便操作，特别是查询操作，减少复杂的SQL语句，增强可读性；更加安全，数据库授权命令不能限定到特定的行和特定的列，但通过合理创建视图，可以把权限限定到行列级别；
  ```

- 应用场景

  ```
  1.权限控制的时候，不希望用户访问表中某些敏感信息的列
  2.对表进行复杂的关联查询时
  ```

- 示例：

  -  查询小飞上的所以课程相关信息 

    ```sql
    SELECT sid, sname, student.name from course 
    LEFT JOIN student on course.sid = student.course_id
    where student.name = '小飞';
    ```

  -  创建视图 view_student_course 之后查询

    ```sql
    create ALGORITHM = UNDEFINED
    DEFINER = 'root'@'%'
    SQL SECURITY DEFINER
    VIEW view_student_course AS (
    SELECT sid, sname, student.name from course 
    LEFT JOIN student on course.sid = student.course_id
    );
    
    SELECT * from view_student_course where name = '小飞';
    ```

## 2.触发器

- 什么是触发器
  
- 触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合。
  
- 触发器的特性：
  1. 在 begin end体， begin … end; 之间的语句可以写的简单或者复杂
  2. 什么条件触发：insert、update、delete
  3. 什么时候触发：在增删改前或者后
  4. 触发频率： 针对每一行执行
  5. 触发器定义在表上，附着在表上

- 创建触发器

  - 单个触发器

    ```sql
    CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件 ON 表名 FOR EACH ROW 执行语句;
    ```

    ```sql
    -- 创建了一个名为trig1的触发器，一旦在work表中有插入动作，就会自动往time表里插入当前时间
    
    mysql> CREATE TRIGGER trig1 AFTER INSERT
        -> ON work FOR EACH ROW
        -> INSERT INTO time VALUES(NOW());
    ```

  - 创建有多个执行语句的触发器

    ```sql
    -- CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
    ON 表名 FOR EACH ROW
    BEGIN
            执行语句列表
    END;
    ```

    ```sql
    mysql> DELIMITER ||
    mysql> CREATE TRIGGER trig2 BEFORE DELETE
        -> ON work FOR EACH ROW
        -> BEGIN
        -> INSERT INTO time VALUES(NOW());
        -> INSERT INTO time VALUES(NOW());
        -> END||
    mysql> DELIMITER ;
    ```

- 查看触发器

  ```
  mysql> SHOW TRIGGERS\G;
  ……
  
  结果，显示所有触发器的基本信息；无法查询指定的触发器。
  
  
  在information_schema.triggers表中查看触发器信息
  mysql> SELECT * FROM information_schema.triggers\G
  ……
  
  结果，显示所有触发器的详细信息；同时，该方法可以查询制定触发器的详细信息。
  mysql> select * from information_schema.triggers 
      -> where trigger_name='upd_check'\G;
  ```

- 删除触发器

  ```
  DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
  ```

  

## 3.函数

**3.1 什么是函数**

```
函数存储着一系列sql语句，调用函数就是一次性执行这些语句。所以函数可以降低语句重复。但要注意的是**函数注重返回值，不注重执行过程**，所以一些语句无法执行。所以函数并不是单纯的sql语句集合。mysql有内置函数，也能够自定义函数

​补充：**函数与存储过程的区别：函数只会返回一个值，不允许返回一个结果集。函数强调返回值，所以函数不允许返回多个值的情况，即使是查询语句。**
```

**3.2 函数的创建**

语法：

```sql
Create function function_name(参数列表)
returns 返回值类型
BEGIN
函数体内容
END
```

相关说明：

​     函数名：应该合法的标识符，并且不应该与已有的关键字冲突。一个函数应该属于某数据库，可以使用db_name.funciton_name的形式执行当前函数所属数据库，否则默认为当前数据库。
​     参数列表：可以有一个或多个函数参数，甚至是没有参数也是可以的。对于每个参数，由参数名和参数类型组成。
​     返回值： 指明返回值类型
​     函数体：自定义函数的函数体由多条可用的MySQL语句，流程控制，变量申明等语句构成。需要指明的是函数体中一定要含有return 返回语句。

 

**3.3 自定义示例**

　　（1）无参数函数定义

```sql
delimiter $$

CREATE FUNCTION hello()
RETURNS VARCHAR(255)
BEGIN
RETURN 'Hello world, i am mysql';
END $$

delimiter ;
```

调用函数：

```
MariaDB [db1]> select hello();
+-------------------------+
| hello()                 |
+-------------------------+
| Hello world, i am mysql |
+-------------------------+
```

　　（2）含有参数的自定义函数

```sql
delimiter $$
CREATE FUNCTION f1(
    t1 int,
    t2 int)
RETURNS INT
BEGIN
    DECLARE num int;
  set num = t1 + t2;
  RETURN(num);
END $$
delimiter ;
```

调用函数：

```sql
MariaDB [db1]> select f1(1, 100);
+------------+
| f1(1, 100) |
+------------+
|        101 |
+------------+
```

**3.4 查看库中的函数**

```sql
-- 查看函数
show FUNCTION status;
-- 查看函数的创建过程：
show create function func_name;
```



## 4.存储过程

### **4.1 什么是存储过程**

```python
存储过程是一组为pb，它存储在数据库中，一次编译后永久有效，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它。

#优点：
   （1）将重复性很高的一些操作，封装到一个存储过程中，简化了对这些SQL的调用；
   （2）批量处理：SQL+循环，减少流量，也就是“跑批”；
   （3）统一接口，确保数据的安全
```

###  **4.2 存储过程的创建和调用** 

- 创建存储过程：

```sql
CREATE
    [DEFINER = { user | CURRENT_USER }]
PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

characteristic:
    COMMENT 'string'
  | LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }

routine_body:
Valid SQL routine statement

[begin_label:] BEGIN
[statement_list]
……
END [end_label]
```

- 示例：

  - 创建存储过程:  这个存储过程做了两件事，一个是查询所有的teacher，另一个就是向student表中插入一条数据 

  ```python
  CREATE PROCEDURE p1()
  BEGIN
      select * from teacher;
      insert into userinfo(username, password) VALUES ('xiaoA', '123');
  END ;
  ```

  - 调用

  ```sql
  MariaDB [db1]> call p1;
  +-----+-----------+
  | tid | name      |
  +-----+-----------+
  |   1 | 周杰伦    |
  |   2 | 那英      |
  |   3 | 汪峰      |
  |   4 | 哈林      |
  +-----+-----------+
  rows in set (0.00 sec)
  
  Query OK, 1 row affected (0.00 sec)
  ```

### 4.3存储过程的参数

 存储过程可以有 0 个或多个参数，用于存储过程的定义 

- 参数类型：
  - IN 输入参数：表示调用者向过程传入值（传入值可以是字面量或变量）；
  - OUT输出参数：表示过程向调用者传出值(可以返回多个值)（传出值只能是变量）；
  - INOUT输入输出参数：既表示调用者向过程传入值，又表示过程向调用者传出值（值只能是变量）

**IN输入参数的使用**

```
delimiter $$
CREATE PROCEDURE p2(in t1 int)
BEGIN
    SELECT t1;
  set t1 = 2;
    SELECT t1;
END $$
delimiter ;
```

 调用存储过程：

```
MariaDB [db1]> set @t1 = 1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [db1]> call p2(@t1);
+------+
| t1   |
+------+
|    1 |
+------+


+------+
| t1   |
+------+
|    2 |
+------+

MariaDB [db1]> select @t1;
+------+
| @t1  |
+------+
|    1 |
+------+
```

以上可以看出，t1 在存储过程中被修改，但并不影响@t1 的值，因为前者为局部变量、后者为全局变量。

**OUT 输出参数**

```
delimiter $$
CREATE PROCEDURE p3(out t_out int)
BEGIN
    SELECT t_out;
  set t_out = 2;
    SELECT t_out;
END $$
delimiter ;
```

调用存储过程：

```
MariaDB [db1]> set @t_out =1 ;

MariaDB [db1]> call p3(@t_out);
+-------+
| t_out |
+-------+
|  NULL |
+-------+
# 因为out是向调用者输出参数，不接收输入的参数，所以存储过程里的p_out为null

+-------+
| t_out |
+-------+
|     2 |
+-------+

MariaDB [db1]> select @t_out;
+--------+
| @t_out |
+--------+
|      2 |
+--------+
# 调用了 p3 存储过程，输出参数，改变了 t_out 变量的值
```

**inout输入参数**

```
delimiter $$
CREATE PROCEDURE p4(inout t_inout int)
BEGIN
    SELECT t_inout;
  set t_inout = 2;
    SELECT t_inout;
END $$
delimiter ;
```

 调用存储过程：

```
MariaDB [db1]> set @t_inout = 1;

MariaDB [db1]> call p4(@t_inout);
+---------+
| t_inout |
+---------+
|       1 |
+---------+

+---------+
| t_inout |
+---------+
|       2 |
+---------+

MariaDB [db1]> select @t_inout;
+----------+
| @t_inout |
+----------+
|        2 |
+----------+
```

调用了 p4 存储过程，接受了输入的参数，也输出参数，改变了变量

注意：

　 （1）如果过程没有参数，也必须在过程名后面写上小括号

 　（2）确保参数的名字不等于列的名字，否则在过程体中，参数名被当做列名来处理

建议使用：

　　输入值使用 in 参数；

　　输入值使用 in 参数；

　　inout参数就尽量少用

### **4.4 存储过程-事务**

　　在执行一个存储过程中，我们无法确定这个存储过程是否执行成功，如果执行失败，我们是否要考虑回滚的问题。这里就需要存储过程对于事务的支持：

```
delimiter //
create procedure p4(
    out status int
)
BEGIN
    1. 声明如果出现异常则执行{
        set status = 1;
        rollback;
    }
       
    开始事务
        -- 由秦兵账户减去100
        -- 方少伟账户加90
        -- 张根账户加10
        commit;
    结束
    
    set status = 2;
    
    
END //
delimiter ;
```

存储过程支持事务如下：

```
delimiter $$
CREATE PROCEDURE p5(out p_return_code tinyint)
BEGIN
    DECLARE exit HANDLER for SQLEXCEPTION
    BEGIN
    -- 执行失败，则返回 1
        set p_return_code = 1;
        ROLLBACK; -- 如果出错，则回滚
    END;
    START TRANSACTION;
    INSERT into userinfo(username, password) VALUES ('xiaoB', '222');
    COMMIT;
    -- 执行成功，则返回 2
    set p_return_code = 2;
END $$
delimiter ;

执行：
MariaDB [db1]> set @p_return_code=0;

MariaDB [db1]> call p5(@p_return_code);

MariaDB [db1]> select @p_return_code;
+----------------+
| @p_return_code |
+----------------+
|              2 |
+----------------+
```

变量 p_return_code = 2 说明存储过程执行成功。



### **4.5 使用 pymysql 模块调用存储过程**

```
import  pymysql

config = {
    'host': '192.168.118.11',
    'user': 'root',
    'password': '123456',
    'database': 'db1'
}

db = pymysql.connect(**config)
with db.cursor(cursor=pymysql.cursors.DictCursor) as cursor:
    cursor.callproc('p3', (0,))   # 使用 callproc 调用存储过程
    cursor.execute('select @_p3_0') # 查询 out 参数的返回值
    r2 = cursor.fetchall()  # 获取返回值
    print(r2)

执行结果：
[{'@_p3_0': 2}]
```

###  **4.6 查看存储过程**

```
-- 查看存储过程：
show procedure status; 

-- 查看存储过程创建的过程：
show create procedure proc_name;
```

*转自： https://www.cnblogs.com/hukey/p/10396404.html 