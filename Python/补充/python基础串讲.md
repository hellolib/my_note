## python基础串讲

```python
1.重要基础
2.企业的开发流程
3.面试题讲解
作业:面试题前35页
```

## 1.重要知识点

1. Python和其他语言的对比:

   ```python
   C语言,更偏向底层的语言/自己代码实现内存管理,所以性能很好,一般用作底层开发;
   PHP,在web开发方面比较厉害;
   Java,各方面都有广泛的应用,一般适用于企业级应用的开发;
   C# ,与java类似,但是由于和windows server 的绑定关系,逐渐走向衰退;
   Python,解释性语言,从诞生开始就自然成长,在各个领域都有很丰富的类库:web/爬虫/数据分析/机器学习/人工智能.
   Golang,因为docker的流行,go语言支持高并发.跟接近底层语言;
   
   # 大学学习过c语言,但是长时间没用,差不多都忘了,主要写的是python代码,目前在自学go语言.
   ```

2. 公司项目使用的python2还是python3?

   ```python
   很多老项目仍然在使用python2,但是新项目使用的都是python3
   ```

3. python2和python3的区别?

   ```python
   1.默认解释器编码不同:python2是ascii码,python3是utf-8;
   2.字符串和字节不同:python2中的字符串有 unicode(显示格式)和 str(存储格式),python3中的字符串是str(显示格式)和 bytes(存储格式);
   3.经典类和新式类:py2中有经典类和新式类,py3中只有新式类(默认继承object);
   4.字典的keys和values:py2中这两种方法返回的都是列表,python3中返回的是一个迭代器,节约内存资源;
   5. yield from:在py3中新加了yield from关键字,可以在迭代器中调用另外一个迭代器;
   6.进程池线程池:py2中只有进程池(from multiprocessing import Pool),没有线程池,python3中存在	进程池和线程池(from concurrent.futures.thread import ThreadPoolExecutor
   from concurrent.futures.process import ProcessPoolExecutor)
   7.另外还有xrange/range,input/print(),int/long的区别;
   ```

4. **常用**数据类型的方法:

   ```python
   1.str: strip().split(),format(),join(),encode(),str()
   2.list:append(),pop(),extent(),insert(),remove(),sort(),list()
   3.dict:get(),keys(),items(),values(),update()
   4.tuple:tuple(),
   5.set:add(),discard(),intersection(),union(),difference()
   ```
   
5. 集合 求交集/并集/差集

   ```python
   #.intersection() 交集
   命令后的 () 可以是集合,也可以是列表.
   info = {1,2,3}
   print(info.intersection({1,3,4})) #{1,3}
   #.union()并集
   info = {1,2,3}
   print(info.union({1,3,4})) #{1,2,3,4}
   #.difference()差集
   info = {1,2,3}
   print(info.union({1,3,4})) #{2,4}
   ```

6. 运算符的计算:

   ```python
   () > not > and > or
   # 先进行数学运算,再进行逻辑运算
   ```

7. 深浅拷贝的区别?

   ```python
   对于不可变数据类型来说都一样,对于可变数据类型:
   # 浅拷贝:
   只拷贝数据的第一层
   # 深拷贝:
   对于可变的数据类型,拷贝拷贝嵌套层级中的所有可变类型
   ----------------------------------------------------------
   id(查看内存地址),赋值更改内存地址,内部变更改变量的值
   # 内存地址
   a = 1
   b = 1
   id(a) = id(b)
   #按理说a与b的id不该一样,但是在python中,为了提高运算性能,对某些特殊情况进行了缓存.(小数据池)缓存
   对象:
   1. 整型： -5 ~ 256
   2. 字符串："alex",'asfasd asdf asdf d_asdf ' ----"f_*" * 3 - 重新开辟内存。
   == 与is 区别
   ==比较的是值是否一致
   is 比较内存地址是否一致
   ```

8. pass 的作用

   ```python
   占位,不进行任何操作
   ```

9. *args和**kwargs的作用

   ```python
   # *args
   只接收位置参数,可以接收多个,实参传递到形参时是以元组形式传递,函数内部打印args是一个元组,内部*args会将元组打散成多个元素
   # **kwargs
   只接收关键字参数,可以接收多个,实参传递到形参时是以字典的形式传递的,实参传递字典时加**会将字打散之后传递
   ```

10. 对于函数的参数传递的是值还是引用?

    ```python
    引用,就是内存地址,可以修改
    ```

11. 函数的**形参默认值如果是一个可变类型**的话有一个大坑?

    ```python
    # 问题描述
    如果调用函数时，不对b进行赋值，那么b就是在创建函数之处函数内部创建的一个空列表。只要不传值都使用此默认值。
    
    # 代码示例
    def func(a,b=[]):
    	b.append(a)
    	return b
    v1 = func(1) # [1,]
    v2 = func(2,[])
    v3 = func(3)
    print(v1,v2,v3)
    # [1,3]
    # [2]
    # [1,3]
    ```

12. 闭包

    ```python
    # 什么是闭包?
    内层函数对外层函数值进行了使用。:嵌套函数,内部应用外部的值
    # 通过闭包保留原来的值
    func_list = []
    def func(arg):
    	def inner():
    		print(arg)
    	return inner
    for item in range(10):
    	func_list.append(func(item))
    func_list[0]()
    func_list[1]()
    ```

13. 装饰器

    ```python
    # 手写
    def outer(func):
    	def inner(*arg,**kwargs):
    		return func(*arg,**kwargs)
    	return inner
    不改变原代码原功能的情况下在代码之前或者代码之后添加一些功能,有点开放封闭的意思:源码封闭,外部开放
    # 应用场景:
    类里的静态属性
    Django中:
        自定义标签
        cbv加装饰器
        csrf装饰器
        缓存和信号
    Flask:
        路由,特殊的装饰器
    ```

14. 带参数的装饰器

    ```python
    # 手写
    def counter(num):
    	def outer(func):
    		def inner(*arg,**kwargs):
    			for i in range(num):
    			ret = func(*arg,**kwargs)
    			return ret
    		return inner
        return outer
    @counter(6)
    def func():
    	print('函数')
    ```

15. 反射

    ```python
    getattr(),setattr(),hasattr()
    #低级用法:
    通过字符串去对象中查找方法或者属性等getattr(obj,'func')()
    #高级用法：
    Django中间件,flask上下文管理：LocalProxy对象,字符串的配置文件
    #应用场景：
    django中间件实现原理基于反射。
    Flask中LocalProxy对象。
    ```

16. 生成器

    ```python
    # yield关键字的作用
    函数中如果存在yield，那么此函数就变成了生成器函数。
    obj = func() # obj就是生成器
    # 手写生成器
    def func():
    	for i in range(10):
    		yield i
    v2 = func()
    for i in v2:
        print(2)
    v2 = (i for i in range(10)) # 生成器推导式，创建了一个生成器，内部循环为执行。
    ----------------
    迭代器，对可迭代对象中的元素进行逐一获取，迭代器对象的内部都有一个 next方法，用于以一个个
    获取数据。
    可迭代对象，可以被for循环且此类对象中都有 iter方法且要返回一个迭代器（生成器）。
    生成器，函数内部有yield则就是生成器函数，调用函数则返回一个生成器，循环生成器时，则函数内部
    代码才会执行。
    生成器是特殊的迭代器（**）：
    ```

    

17. 面向对象 上下文 机制

    ```python
    # 手写 P31-22题(手写)
    class Foo(object):
        def do_something(self):
            print("我做了点什么")
    class Context(object):
        def __enter__(self):
            print('进入')
            return Foo()
        def __exit__(self, exc_type, exc_val, exc_tb):
            print("退出")
    with Context() as ctx:
        print("程序开始")
        ctx.do_something()
    ```

18. super的作用

    ```python
    super()方法就是调用父类的方法;
    # 深度优先和广度优先
    深度优先: 对每一个可能的分支路径深入到不能再深入为止，而且每个结点只能访问一次。
    广度优先:从上往下对每一层依次访问，在每一层中，从左往右（也可以从右往左）访问结
    点，访问完一层就进入下一层，直到没有结点可以访问为止。
    # mro 
    新式类: 默认继承object,支持super()方法,多继承广度优先C3算法,具有mro方法
    经典类: python2中不继承ovject就是经典类,没有super,多继承深度优先,没有mro方法
    ```

19. 常用模块

    ```python
    # 内置模块 & 第三方模块
    内置模块:
        loging :操作日志
    	json :通用序列化模块
    	pickle :python特有的序列化模块
    	time :和时间交互
    	datetime :和时间交互
    	os : 和操作系统交互
    	sys :和解释器交互
    	hashlib :提供摘要算法
    	re :正则表达式模块
    	random :随机模块
    常用的第三方模块:
        导入的都可以称为第三模块
    
    # map,fittler,xxx
    map(函数,可迭代对象),映射函数,循环每个元素,然后让每个元素执行函数,将每个函数执行的结果保存到新的列表,并返回.
    v1 = [11,22,33,44]
    result = map(lambda x:x+100,v1)
    print(list(result)) # 特殊
    
    fifter(函数,可迭代对象),筛选过滤,循环每个元素然后执行函数,将每个函数执行的结果筛选保存到新的列表并返回.
    result = filter(lambda x: True if type(x) == int else False ,v1)
    print(list(result))
    result = filter(lambda x: type(x) == int ,v1)
    print(list(result))
    
    reduce(函数,可迭代对象),循环每个元素,然后执行函数,将每个函数执行的结果筛选保存到新的列表并返回.
    import functools
    v1 = ['wo','hao','e']
    def func(x,y):
    	return x+y
    result = functools.reduce(func,v1)
    print(result)
    ```

20. re模块中的match和search 的区别

    ```python
    # re.match(pattern表达式规则, string)
    	-从头开始,等同于re.search加上^号
    	-如果不是起始位置匹配成功的话，match()就返回none
    	-匹配成功re.match方法返回一个匹配的对象，否则返回None
    #re.search("匹配规则", "要匹配的字符串")
    	-匹配成功re.search方法返回一个匹配的对象，否则返回None
    	-从头到尾从头匹配字符串中取出第一个符合条件的项.
    	-re.match只匹配字符串的开始，如果字符串开始不符合正则表达式,则匹配失败，函数返回		None；而re.search匹配整个字符串，直到找到一个匹配。
    ```

21. 贪婪匹配

    ```python
    默认贪婪匹配 ,总是在符合匹配规则的范围内尽可能多的匹配
    非贪婪匹配,(惰性匹配):总是在符合匹配规则的范围内尽可能少的匹配
    ```

22. 正则表达式基本书写

    ```python
    # 经常写写
    ```

23. zip函数

    ```python
    python divmod() 函数把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a // b, a % b)。
    >>>divmod(7, 2)
    (3, 1)
    >>> divmod(8, 2)
    (4, 0)
    ```

24. 常用的内置函数

    ```python
    map/filter/max/min/sum/zip/sorted/divmod/print/input
    ```

25. 单例模式

    ```python
    当一个py文件被导入时,就是一个单例模式;
    __new__方法:实例化对象的时候,首先执行__new__方法,返回的是一个对象,内部没有数据,然后紧接着执行__init__方法,填充数据;
        
    # 单例写成一个装饰器
    def wrapper(cls):
        instance = {}
        def inner(*args,**kwargs):
    		if cls not in instance:
                instance[cls] = cls(*args,**kwargs)
           	return instance[cls]
        return inner
    
    @wrapper
    class cls(object):
        def __init__(self):
            print('实例化')
            
    # 线程加单例
    import time
    from threading import Lock
    class A:
    	__instance = None
    	lock = Lock()
    	def __new__(cls, *args, **kwargs):
    		with cls.lock:
    			if not cls.__instance:
    				time.sleep(0.1)
    				cls.__instance = object.__new__(cls)
    		return cls.__instance
    	def __init__(self,name,age):
    		self.name = nam
        
    # 适用场景：
    1.需要生成唯一序列的环境
    2.需要频繁实例化然后销毁的对象。
    3.创建对象时耗时过多或者耗资源过多，但又经常用到的对象。 
    4.方便资源相互通信的环境
    #优点：
    1.实现了对唯一实例访问的可控
    2.对于一些需要频繁创建和销毁的对象来说可以提高系统的性能。
    #缺点：
    1. 不适用于变化频繁的对象
    2.滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出。
    3.如果实例化的对象长时间不被利用，系统会认为该对象是垃圾而被回收，这可能会导致对象状态的丢失。
    ```

26. 原类:

    ![1568174523823](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1568174523823.png)

- 源类

  ![1568174679281](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1568174679281.png)



- metaclass指定由谁创建类,默认值是type

## 2.企业的开发流程

```你和老男孩是一个团队```

1. 入职第一天

   ```python
   1.各种账号(企业邮箱,钉钉,VPN,gitlab账号,任务管理账号:禅道/蒲公英任务管理/git)
   2.项目(地址,自己看,熟悉项目)
   ```

2. 熟悉项目

   ```代码要以,功能为核心,质量其次```

- 小项目:类似于crm项目
  - 类似于crm项目
  - 工作
    - 找BUG,修复:粗粒度观察项目,不需要读懂每一行代码,注重效率!
    - Linux的部署(环境安装)
    - 新功能迭代更新

- 中等项目:
  - 人员配备
    - 产品经理:产品功能,项目原型图
    - UI : 图片生成
    - 前端 : 代码实现前端界面
    - 后端: 表结构设计/页面+后台进行开发
    - 测试: 功能和性能测试
    - 运维: 代码发布,代码部署(自动化工具Jenkins)

- 大型项目:
  
- 基于git协同开发
  
- 面试题

  1. 如何使用git实现协同开发?

     ```python
     每个人都要有一个自己的分支;最后完成之后合并到dev分支中;
     ```


- 虚拟环境:

  ```python
  1.安装virtualenv -pip install virtualenv
  2.创建目录 virtualenv env_name (类似于安装python)
  3.激活虚拟环境: activate
  4.写代码
  5.安装模块
  6.退出虚拟环境 deactivate
  -----
  虚拟环境迁移
  requeirements.text
  1.生成requeirements.text
  	进入项目目录下执行 requeirements.text,找到当前环境中的模块
  ```

- 代码检测:

  ```python
  检测工具:Flake8
  命令: flake8 name.py
  ```

- sorted

  ```python
  key -- 主要是用来进行比较的元素，只有一个参数，指定可迭代对象中的一个元素来进行排序。
  [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
  >>> sorted(L, key=lambda x:x[1])               # 利用key
  [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
  ```

- lamada表达式

  ```python
  lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
  def func(a1,a2):
  	return a1 + 100
  func = lambda a1,a2: a1+100 #经典格式
  
  ```

- 99乘法表一行

  ```python
  print (["%d*%d=%d"%(i,j,i*j) for i in range(1,10) for j in range(1,i+1)])
  ```

- 如何判断是方法还是函数

  ```python
  from types import MethodType, FunctionType
  boo = isinstance(func, MethodType)
  ```

- drf 框架继承的视图类

  ![1568102237409](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1568102237409.png)