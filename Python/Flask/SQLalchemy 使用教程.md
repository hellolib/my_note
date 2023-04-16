# SQLalchemy 使用教程

## 前戏:

​        不用怀疑,你肯定用过Django中的orm,这个orm框架是django框架中自己封装的,在Django中配置和使用较为简单,但是并不适用于其他web框架,而今天说的sqlalchemy是兼容python语言的orm框架,相信你已经明白谁牛逼!

​		**下面**,接下来....

​		还有比案例更好的教程吗,那下面请您享用为您准备好的大餐...

## 1.单表操作

### 1.1创建表

- 导入sqlachemy资源包

- 案例

  ```python
  from sqlalchemy.ext.declarative import declarative_base
  
  BaseModel = declarative_base()
  
  # 创建 Class / Table
  from sqlalchemy import Column,Integer,String
  
  class User(BaseModel):
      __tablename__ = "user" # 创建Table时名字
      id = Column(Integer,primary_key=True,autoincrement=True)
      name = Column(String(32),nullable=False,index=True,unique=True)
      # Column 定义数据列
      # int string 数据类型
  
  # 数据库引擎的创建:
  from sqlalchemy.engine import create_engine
  engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/dbname?charset=utf8") # 数据库连接驱动语句
  
  #利用 User 去数据库创建 user Table
  BaseModel.metadata.create_all(engine) # 数据库引擎
  # 数据库呢? 数据库服务器地址呢?
  # 数据库连接呢?
  ```

### 1.2CURD(增删改查)

- 案例

  ```python
  # 模拟 navcat 操作
  # 1.选择数据库
  from sqlalchemy.engine import create_engine
  engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/s21?charset=utf8")
  # 2.选择表
  # 3.创建查询窗口
  from sqlalchemy.orm import sessionmaker
  select_db = sessionmaker(engine) # 选中数据库
  db_session = select_db() # 已经打开查询窗口
  # 4.写入SQL语句
  user = User(name="Alexander.DSB.Li") # == insert into user(`name`) value ("Alexander.DSB.Li")
  user_list = [User(name="Alex's Father"),User(name="李杰")]
  # 放入查询窗口
  db_session.add(user)
  db_session.add_all(user_list)
  # 5.提交sql语句
  db_session.commit()
  # 6.关闭查询窗口
  db_session.close()
  
  
  # 简单无条件查询
  # """
  # select * from user  table_user == class_User
  # """
  res = db_session.query(User).all() # 查询全部符合条件的数据
  res = db_session.query(User).first() # 查询符合条件的第一条数据
  print(res.id,res.name)
  
  # 简单条件查询
  # """
  # select * from user where id=3
  # """
  res = db_session.query(User).filter(User.id==3).all()
  print(res[0].id,res[0].name)
  res = db_session.query(User).filter_by(id=3).all()
  
  res = db_session.query(User).filter(User.id==3 , User.name == "123").all()
  print(res)
  #
  is_true_or_false = User.id==3 and User.name == "123"
  
  
  # 修改数据 update
  res = db_session.query(User).filter(User.id == 1).update({"name":"李亚历山大"})
  db_session.commit()
  db_session.close()
  
  # 删除数据
  res = db_session.query(User).filter(User.id == 2).delete()
  db_session.commit()
  db_session.close()
  ```

## 2.一对多

### 2.1创建表

- 案例

  ```python
  from sqlalchemy.ext.declarative import declarative_base
  from sqlalchemy import Column,Integer,String,ForeignKey
  from sqlalchemy.engine import create_engine
  
  #ORM精髓 relationship
  from sqlalchemy.orm import relationship
  
  engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/s21?charset=utf8")
  BaseModel = declarative_base()
  
  # 一对多
  class School(BaseModel):
      __tablename__ = "school"
      id = Column(Integer,primary_key=True)
      name = Column(String(32),nullable=False)
  
  class Student(BaseModel):
      __tablename__ = "student"
      id = Column(Integer,primary_key=True)
      name = Column(String(32),nullable=False)
      sch_id = Column(Integer,ForeignKey("school.id"))
  
      # 关系映射
      stu2sch = relationship("School",backref="sch2stu")
  
  
  BaseModel.metadata.create_all(engine)
  ```

  

### 2.2CURD

- 使用案例

  ```python
  from sqlalchemy.orm import sessionmaker
  from app01.static.上午.createForeignKey import engine
  
  select_db = sessionmaker(engine)
  db_session = select_db()
  
  # 增加数据
  # 先建立一个学校 再查询这个学校的id 利用这个ID 再去创建学生添加sch_id
  # relationship 正向添加 relationship字段出现在哪个类
  # stu = Student(name="DragonFire",stu2sch=School(name="OldBoyBeijing"))
  # stu sql 语句
  # db_session.add(stu)
  # db_session.commit()
  # db_session.close()
  
  # relationship 反向添加
  # sch = School(name="OldBoyShanghai")
  # sch.sch2stu = [
  #     Student(name="赵丽颖"),
  #     Student(name="冯绍峰")
  # ]
  #
  # db_session.add(sch)
  # db_session.commit()
  # db_session.close()
  
  
  # 查询 relationship 正向
  # res = db_session.query(Student).all()
  # for stu in res:
  #     print(stu.name,stu.stu2sch.name)
  
  # 查询 relationship 反向
  # res = db_session.query(School).all()
  # for sch in res:
  #     # print(sch.name,len(sch.sch2stu)) 学校里面有多少学生
  #     for stu in sch.sch2stu:
  #         print(sch.name,stu.name)
  ```

## 3.多对多

### 3.1 创建表

- 案例

  ```python
  from sqlalchemy.ext.declarative import declarative_base
  from sqlalchemy import Column,Integer,String,ForeignKey
  from sqlalchemy.engine import create_engine
  from sqlalchemy.orm import relationship
  
  BaseModel = declarative_base()
  engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/s21?charset=utf8")
  
  class Girl(BaseModel):
      __tablename__ = "girl"
      id = Column(Integer,primary_key=True)
      name = Column(String(32),nullable=False)
  
      gyb = relationship("Boy",backref="byg",secondary="hotel") # secondary="hotel" 数据表中的数据才能证明两者关系
  
  class Boy(BaseModel):
      __tablename__ = "boy"
      id = Column(Integer,primary_key=True)
      name = Column(String(32),nullable=False)
  
  class Hotel(BaseModel):
      __tablename__ = "hotel"
      id = Column(Integer,primary_key=True)
      bid = Column(Integer,ForeignKey("boy.id"))
      gid = Column(Integer,ForeignKey("girl.id"))
  
  
  BaseModel.metadata.create_all(engine)
  ```

  

### 3.2CURD

- 案例

  ```python
  from sqlalchemy.orm import sessionmaker
  from app01.static.上午.createM2M import engine
  
  select_db = sessionmaker(engine)
  db_session = select_db()
  
  # 增加数据 relationship 正向添加
  # g = Girl(name="赵丽颖",gyb=[Boy(name="DragonFire"),Boy(name="冯绍峰")])
  # db_session.add(g)
  # db_session.commit()
  # db_session.close()
  
  # 增加数据 relationship 反向添加
  # b = Boy(name="李杰")
  # b.byg = [
  #     Girl(name="罗玉凤"),
  #     Girl(name="朱利安"),
  #     Girl(name="乔碧萝")
  # ]
  #
  # db_session.add(b)
  # db_session.commit()
  # db_session.close()
  
  
  # 查询 relationship 正向
  # res = db_session.query(Girl).all()
  # for g in res:
  #     print(g.name,len(g.gyb))
  
  
  # 查询 relationship 反向
  # res = db_session.query(Boy).all()
  # for b in res:
  #     print(b.name,len(b.byg))
  ```

  

