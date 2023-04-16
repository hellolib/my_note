 第一章 python基础

[TOC]

#### 1.为什么学习python?

```python
自己对编程的兴趣
通过朋友的介绍
语言简洁,简单易懂,具有强大的第三方库
跨平台性好， web开发,自动化测试运维，爬虫，人工智能，大数据处理都能做
具有着非常火爆的社区   
很多有名的大公司都在用 
```

#### 2.通过什么途径学习的Python?

```python
1.学校也有python相关的选修课
2.依靠书籍,自主学习 
3.网上的搜索资料   
视频 :51cto   慕课网   
网站 :菜鸟教程  官方文档   博客论坛 简书
读书 :Cook book  核心编程2\3   流畅的python  python学习手册
```

#### 3.公司线上和开发环境使用的什么系统？

```python
# 答案:
公司线上使用的是Linux系统, 具体版本为centos7.0
开发环境使用的是Windows系统

# 拓展
- 开发环境:
	就是与测试环境分开的独立客户机、服务器、配置管理工具等。
- 使用阶段：
1.开发人员每日上传代码到开发环境进行自测。
2.demo会议时，PM同学和开发同学一起测试。（项目开发流程）
发布频率：每天发布1-2次，做持续集成，改完code立马提交。

# 持续集成  优点是可以避免传统模式在集成阶段的除虫会议。持续集成主张项目的开发人员频繁的将他们对源码的修改提交(check in)到一个单一的源码库，并验证这些改变是否对项目带来了破坏。

- 测试环境:
	测试环境是指测试人员利用一些工具及数据所模拟出的、接近真实用户使用环境的环境，测试环境的目的是为了使测试结果更加真实有效。测试环境应该与开发环境分隔开，使用独立的客户机、服务器和配置管理工具。
    
- 线上环境:
	也称发布环境，真实用户访问的环境，要求不能有任何BUG，且不能频繁发布，这样对用户体验性不好。
```

#### 4.Python和Java、PHP、C、C#、C++等其他语言的对比？

```python
1、c,编译型语言它是现代编辑语言的老一辈了，在现代应用中使用不多，所有大部分语言，写法都和c语言差不多，常常被用作学习其他语言的基础
2、PHP解释型语言 主要适用于网页编辑，而python适合与各个领域
3、c++编译型语言 是面向对象的c语言，由于偏底层，所以性能非常高，主要用在一个要求高性能的领域
4、java混合型语言 学习起来python要比java简单快捷的多，java从c++的系统语言中继承5、python是解释性语言，不需要额外的编译过程，而c#必须编译后才能执行
6、python程序是开源的，c# 混合型语言 却不是开源的，python可以跨平台 
7、python比c++等这类语言，更容易学习，语法规则简单，语意化，易读易懂，容易维护
```

#### 5.简述解释型和编译型编程语言？

```python
#解释型语言:
1.解释型语言即代码在执行前边解释边执行, 如Python, Java,Ruby,Perl等语言.
2.解释型语言的优点在于省去了运行前预先整体编译的过程, 该特点成就了解释型语言良好的可移植性, 只要计算机中有该语言的解释器(虚拟机)即可运行. 但由于每一次运行都需要进行一次解释, 所以导致了解释型语言的运行效率低的特点.
3.一些网页脚本，服务器脚本以及辅助开发接口这样的对速度要求不高，对不同系统的兼容性有一定要求的程序则通常使用解释性语言
#编译型语言:
1.编译型语言即代码在执行前将代码进行整体编译形成计算机可以直接识别的二进制文件, 如C, C++等语言.
2.编译型语言的优点在于预先编译,所以在执行过程中计算机不需要再进行翻译,所以执行效率很高. 但由于不同计算机系统识别的二进制文件形式不同, 编译型语言经过编译后的二进制文件不能被所有系统识别, 所以导致了编译型语言的跨平台性差.另外代码经过编译后程序不可修改, 所以保密性好.
3.由于编译型语言的程序运行效率高, 执行速度快,同等条件下对系统的要求比较低, 因此适用于开发操作系统, 大型应用程序, 数据库等.
```

#### 6.Python解释器种类以及特点？

```python
# Python解释器共有5种:CPython, IPython, PyPy, Jython, IronPython.
1.CPython:官方版本解释器, 该解释器由C语言开发, 在命名行下运行python，就是启动CPython解释器，是使用最广的Python解释器。还可以自行编写Python解释器来执行Python代码.
2.IPython:该解释器是基于CPython之上的一个交互式解释器,内核其实是调用IE,IPython只是在交互模式下比CPython解释器有所增强, 执行代码的能力与CPython完全一样.属于灵活、可嵌入的解释器.
3.PyPy:该解释器的目标是执行速度的提高, PyPy采用JIT技术, 对Python代码进行动态编译, 所以可以显著提高Python代码的执行速度.
4.Jython:该解释器是运行在Java平台上的解释器, 可以直接把Python代码编译成Java字节码执行.
5.IronPython:该解释器是运行在.Net平台上的Python解释器, 可以把Python代码直接编译成.Net字节码.
```

#### 7.位和字节的关系？

```python
1.位(bit)
英文bit, 表示二进制位.位是计算机内部数据存储的最小单位, 一个二进制位只可以表示0或1两种状态.
2.字节(byte)
英文byte, 用大写的"B"表示.字节是计算机中数据处理的基本单位. 计算机中以字节为单位存储和解释信息,并规定一个字节由八个二进制位构成, 即1个字节等于8个比特.

1 Byte = 8 bit
# 数据存储是字节(Byte)为单位,数据传输大多是以位(bit)为单位.
```

#### 8.b、B、KB、MB、GB 的关系？

```python
-8b(bit)=1B(Byte)
-1KB(Kilobyte 千字节)=1024B;　　
-1MB(Megabyte 兆字节)=1024KB; 
-1GB(Gigabyte 吉字节)=1024MB;
-1TB(Trillionbyte 太字节)=1024GB;
-1PB(Petabyte 拍字节)=1024TB;
-1EB(Exabyte 艾字节)=1024PB;
-1ZB(Zettabyte 泽字节)1024EB;
-1YB(YottaByte 尧字节)1024ZB;
-1NB(NonaByte )=1024YB;
-1DB(DoggaByte)=1024NB;
 
# 换算关系
1TB = （1*1024）GB = （1*1024*1024）MB = （1*1024*1024*1024）KB = （1*1024*1024*1024*10224）B
```

#### 9.请至少列举个 5个PEP8 规范（越多越好）。

```python
1.缩进(Indentation):默认4个空格为一个缩进层次, 推荐在新项目中仅使用空格而不是制表符.
2.行最大长度(Maximum Line Length):限制行最大79个字符, 对顺序排放的大块文本, 推荐将长度限制在72字符.
3.空行(Blank Lines):用两行空行分割顶层函数和类的定义, 类内方法的定义用单个空行分割. 额外的空行可被用于分割相关函数组成的群.当空行用于分割方法的定义时, 在'class'行和第一个方法定义之间也要有一个空行.
4.导入(Import):通常应该在单独的行中导入一个模块,而不是一行中导入多个模块; 导入模块应该在文件的顶部, 仅在模块注释和文档字符串之后, 在模块的全局变量和常量之前.导入顺序为:标准库模块,相关的主包的导入,特定应用的导入.每组导入之间放置一个空行.
5.空格(Whitespace in Expressions and Statements):
	(1).紧挨着圆括号,方括号和花括号出不应该出现空格:print([1, 2])
	(2).紧贴在逗号,分号或冒号前不应该出现空格, 而应该在其后应该有空格:print(x, y, z)
	(3).紧贴着函数调用的参数列表前开式括号前不应出现空格:func(args)
	(4).紧贴在索引或分片开式的开式括号前不应该出现空格:value = dic["name"]
	(5).运算符两边应该各有一个空格.
		
6. 命名规范
	-尽量单独使用小写字母‘l’，大写字母‘O’等容易混淆的字母。
	-模块命名尽量短小，使用全部小写的方式，可以使用下划线。
	-包命名尽量短小，使用全部小写的方式，不可以使用下划线。
	-类的命名使用CapWords(驼峰)的方式，模块内部使用的类采用_CapWords的方式。
	-异常命名使用CapWords+Error后缀的方式。
	-函数命名使用全部小写的方式，可以使用下划线。
	-常量命名使用全部大写的方式，可以使用下划线。
	-类的属性（方法和变量）命名使用全部小写的方式，可以使用下划线。
	-类的属性若与关键字名字冲突，后缀一下划线，尽量不要使用缩略等其他方式。
	-为避免与子类属性命名冲突，在类的一些属性前，前缀两条下划线。比如：类Foo中声明__a,访问时，只能通过Foo._Foo__a，避免歧义。
	-类的方法第一个参数必须是self，而静态方法第一个参数必须是cls。

7. 注释:
	注释最好使用英文，最好是完整的句子，首字母大写，句后要有结束符，结束符后跟两个空格，开始下一句。如果是短语，可以省略结束符。
	-块注释，在一段代码前增加的注释。在‘#’后加一空格。段落之间以只有‘#’的行间隔。
	-行注释，在一句代码后加注释。
	-避免无谓的注释。

```

#### 10.求结果:求出v1~v6的值

```python
v1 = 1 or 3
v2 = 1 and 3
v3 = 0 and 2 and 1
v4 = 0 and 2 or 1
v5 = 0 and 2 or 1 or 4
v6 = 0 or False and 1

# 结果:
v1 == 1
v2 == 3
v3 == 0
v4 == 1
v5 == 1
v6 == False

# 知识点(方式一)
1.两个非零数进行'与运算',即and运算, 取and右侧的数字, 如果一侧为0, 则取0.
2.两个非零数进行'或运算', 即or运算, 取or左侧的数字, 如果一侧为0, 则取非0一侧.
2.'与或非运算'优先级为 : not > and > or
    
# 知识点(方式二)
1.'与或非运算'优先级为 : not > and > or
2.x or y 的值只可能是x或y. x为真就是x, x为假就是y
3.x and y 的值只可能是x或y. x为真就是y, x为假就是x
```

#### 11.ascii、unicode、utf-8、gbk 区别？

```python
1.asscii码: 最多只能用8位表示,所以最多只能表示256个字符. 不识别中文.
2.Unicode码: 中文占4个字节,英文2个字节
3.utf8: 中文3字节,欧洲2字节, 美洲1字节
4.gbk:中文2个字节,英文1字节
    
Ascii  :英文: 8位 1 字节 = 1字符    不支持中文 
Unicode:英文: 16位 2 字节 = 1字符   中文: 4字节 = 1字符 
Utf-8  :英文: 8bit = 1字节 = 1字符  中文:24位 3 字节 = 1字符 
Gbk    :英文: 1字节 = 1字符         中文: 2字节 = 1字符

```

#### 12.字节码和机器码的区别？

```python
1.当python程序执行时,Python内部会将源代码编译成字节码,并保存成以.pyc为扩展名的文件,若果下次在源码没有改变的情况下,python会加载该文件,这是对启动速度的一种优化.该过程是一个简单的翻译过程,生成的字节码是源代码底层的,与平台无关的表现形式, 字节码是计算机不能直接识别的.
2.程序要完成运行必须经Python虚拟机解释字节码后生成机器码, 机器码是计算机能够识别的语言.python是一门解释型语言,其实解释的并不是源代码, 而是在解释字节码.
```

#### 13.三元运算规则以及应用场景？

```python
1.三元运算符是分支语句实现的简单逻辑,功能与'if....else'流程语句一致
2.三元运算符规则 : if条件成立的执行结果<- if 条件 else ->if条件不成立的执行结果
3.应用场景 : 由于三元运算符结构简单,执行效率更高,当需要使用if分支语句实现的简单逻辑时, 可以将if分支语句整合嵌套为三元运算符. 

# 拓展:C语言实现三元运算符
C ? X : Y  # C为条件, X是C为True时的结果, Y是C为False时的结果
```

#### 14.列举 Python2和Python3的区别？

```python
1.Python3对Unicode字符的原生支持
-Python2中使用 ASCII 码作为默认编码方式导致string有两种类型str和unicode，Python3只支持unicode的string。
# python2和python3字节和字符对应关系为：
python2    python3    表现    转换    作用
str         bytes     字节   encode  存储
unicode     str       字符   decode  显示

2. Python3采用的是绝对路径的方式进行import。
- Python2中相对路径的import会导致标准库导入变得困难。Python3中，如果需要导入同一目录的文件必须使用绝对路径，否则只能使用相关导入的方式来进行导入。

3.Python2中存在老式类和新式类的区别，Python3统一采用新式类。新式类声明要求继承object，必须用新式类应用多重继承。

4.Python3使用更加严格的缩进。Python2的缩进机制中，1个tab和8个space是等价的，所以在缩进中可以同时允许tab和space在代码中共存。

-print语句被python3废弃，统一使用print函数

-exec语句被python3废弃，统一使用exec函数

-不相等操作符"<>"被Python3废弃，统一使用"!="

-long整数类型被Python3废弃，统一使用int

-xrange函数被Python3废弃，统一使用range，Python3中range的机制也进行修改并提高了大数据集生成效率

-Python3中这些方法再不再返回list对象：dictionary关联的keys()、values()、items()，zip()，map()，filter()，但是可以通过list强行转换：
-迭代器iterator的next()函数被Python3废弃，统一使用next(iterator)

-raw_input函数被Python3废弃，统一使用input函数

-字典变量的has_key函数被Python废弃，统一使用in关键词

-file函数被Python3废弃，统一使用open来处理文件，可以通过io.IOBase检查文件类型

-异常StandardError 被Python3废弃，统一使用Exception

-浮点数除法操作符/和//区别
	Python2:/是整数除法，//是小数除法
	Python3:/是小数除法，//是整数除法。
        
- round函数返回值区别
	Python2，round函数返回float类型值
    Python3，round函数返回int类型值
 
-比较操作符区别
	Python2中任意两个对象都可以比较
	Python3中只有同一数据类型的对象可以比较
```

#### 15.python2项目如何迁移到python3

```python
1.清查依赖包, 不支持 python3 的 lib 寻找替代品(常用 lib 基本都没问题).
2.将现有代码转写成 py2/3 兼容代码.
3.修复单元测试，用 tox 在 python2.7 和 python3.6 下跑单元测试, 保证后续代码不会 broken.
4.替换本地开发的 devbox 和 sandbox 环境.
5.灰度切换线上环境.

https://blog.csdn.net/u011089523/article/details/69681099
http://www.php.cn/python-tutorials-85889.html
```

#### 16.用一行代码实现数值交换

```python
a = 1
b = 2

# 知识点: 平行赋值
a, b = b, a
```

#### 17.python3中的int与python2中的long的区别

```python
# python2
1.python2中数值类型分为int(整形), long(长整形), float(浮点实际值),complex(复数).
2.其中int为整数, 没有小数点的正或负整数.
3.long,无限大小的整数, 语法规范上是在数字后面加一个L.
# python3
1.python3中数值类型只有int(整形), float(浮点数), complex(复数), 在python3中可以认为将python2中的int与long合并为python3中的int.
```

#### 18.xrange和range的区别？

```python
1.在python2中, range()函数生成的是一个列表,而xrange()函数是一个具有惰性运算机制的可迭代对象.
2.python3中只有range()函数,在python3中range()函数也是生成一个具有惰性运算机制的可迭代对象.
3.range和xrange都是在循环中使用，输出结果一样。
4.xrange则不会直接生成一个list，而是每次调用返回其中的一个值，内存空间使用极少，性能较好。

# 惰性计算(Lazy Evaluation)，又称懒惰计算、懒汉计算，是一个计算机编程中的一个概念，它的目的是要最小化计算机要做的工作。
```

#### 19.如何实现字符串反转?如: name = "wupeiqi”,反转为name = "iqiepuw”.

```python
1.name = "wupeiqi”
name = name[: : -1]

2.def reversed_str(str):
    return a_string[::-1]
reversed_str( "wupeiqi")
```

#### 20.文件操作时：xreadlines和readlines的区别？

```python
1.readlines()读取文件所有内容, 按行为单位放到一个列表,返回list类型.
2.xreadlines()读取文件返回一个迭代器, 来循环操作文件的每一行.利用循环取值结果和readlines是一样的, 但由于xreadlines的惰性运算机制, 在读取大文件时, 可以很好的减小内存的压力.
```

#### 21列举布尔值为False的常见值？

```python
0，“”，{}，[],（），set（）,False,不成立的表达式，None
```

#### 22.字符串、列表、元组、字典每个常用的5个方法？

```python
字符串用单引号(')或双引号(")括起来，不可变
1，find通过元素找索引，可切片，找不到返回-1
2，index，找不到报错。
3，split 由字符串分割成列表，默认按空格。
4，captalize 首字母大写，其他字母小写。
5，upper 全大写。
6，lower 全小写。
7，title，每个单词的首字母大写。
8，startswith 判断以什么为开头，可以切片，整体概念。
9，endswith 判断以什么为结尾，可以切片，整体概念。
10，format格式化输出
11,strip 默认去掉两侧空格，有条件， 12，lstrip,rstrip 14,center 居中，默认空格。
15，count查找元素的个数，可以切片，若没有返回0
16，expandtabs 将一个tab键变成8个空格，如果tab前面的字符长度不足8个，则补全8个，
17，replace（old，new,次数）
18，isalnum字符串由字母或数字组成 isalpha, 字符串只由字母组成  isdigit 字符串只由数字组成
19,swapcase 大小写翻转
20，for i in 可迭代对象。   
字典：
增：dic[“键”] = “值”    
	setdefault（“键”，“值”）
删： pop（键）            有返回值，返回的是值
    popitem              随机删除，有返回值以元祖的形式进行返回，第一个是键，第二个是值
    del                 删除整个字典 可以通过键删除
    clear               清空
改：dic[“键”] = “值”    不存在就是改
    update({})          参数是一个字典   有就改，没有就添加
查：get（键）            没有返回NONE 可以自定义返回值
其他操作  dic.keys()，   获取字典的键
          dic.values()   获取字典的值
          dic.items()   获取字典的键值 ，以元祖形式返回         
```

#### 23.is 和==的区别

```python
is  比较的是内存地址
== 比较的是值
```

#### 24.1,2,3,4,5能组成的互不相同且无重复的三个数

```python
l = [1,2,3,4,5]
l1=[]
 for i in l:
     for j in l:
         for m in l:
             if i!=j and i!=m and j!=m:
                 l1.append(str(i)+str(j)+str(m))
 print(len(l1),l1)
```

#### 25.什么是反射,反射的应用场景

```python
反射：某个命名空间中的某个"变量名",获取这个变量名对应的值
文件或者input输入的时候我们会得到某个变量名，这时候我们可以通过反射的方法来获取这个变量名对应的值
```

#### 26.简述python的深浅拷贝

```python
浅拷贝只拷贝一层[1,2,[3,5]]，这时候的浅拷贝后[3,5]的内存地址还是原来的。切片或者赋值都是浅拷贝
拷贝可变数据类型 不可变数据类型不拷贝
深拷贝完全拷贝，与原来的完全没有关系  不可变数据类型使用原来内存地址
```

#### 27.python垃圾回收机制

```python
 引用计数
#原理：当一个对象的引用被创建或者复制时，对象的引用计数加1；
# 当一个对象的引用被销毁时，对象的引用计数减1，
# 当对象的引用计数减少为0时，就意味着对象已经再没有被使用了，
# 可以将其内存释放掉。
# 优点：引用计数有一个很大的优点，即实时性，任何内存，一旦没有指向它的引用，
# 就会被立即回收，而其他的垃圾收集技术必须在某种特殊条件下才能进行无效内存的回收。
# 缺点：引用技术存在一个很大的问题－循环引用，因为对象之间相互引用，每个对象的引用都不会为0，
# 所以这些对象所占用的内存始终都不会被释放掉。
# 标记清除
#标记－清除只关注那些可能会产生循环引用的对象
# 缺点 标记和清除的过程效率不高
# 分代回收
# 将系统中的所有内存块根据其存活时间划分为不同的集合，每一个集合就成为一个“代”，
# Python默认定义了三代对象集合，垃圾收集的频率随着“代”的存活时间的增大而减小。
# 也就是说，活得越长的对象，就越不可能是垃圾，就应该减少对它的垃圾收集频率。
# 那么如何来衡量这个存活时间：通常是利用几次垃圾收集动作来衡量，
# 如果一个对象经过的垃圾收集次数越多，可以得出：该对象存活时间就越长


# 1、当内存中有不再使用的部分时，垃圾收集器就会把他们清理掉。它会去检查那些引用计数为0的对象，
# 然后清除其在内存的空间。当然除了引用计数为0的会被清除，
# 还有一种情况也会被垃圾收集器清掉：当两个对象相互引用时，他们本身其他的引用已经为0了。
# 2、垃圾回收机制还有一个循环垃圾回收器, 确保释放循环引用对象(a引用b, b引用a, 导致其引用计数永远不为0)。
```



#### 28.python的可变类型和不可变类型

```python
可变数据类型：列表、字典、可变集合
不可变数据类型：数字、字符串、元组、不可变集合
```

#### 29.求结果

```python
v = dict.fromkeys(['k1','k2'],[])
v['k1'].append(666)
print(v)
v['k1']=777
print(v)

执行结果：
{"k1":[ 666],"k2":[666]}  #此处执行的是浅拷贝
{"k1":777,"k2":[666]}   
```

#### 30.一行代码删除列表里重复的值

```python
可以利用集合去重的功能进行删除
```



#### 31.如何实现"1,2,3"变成['1','2','3']

```python
s = "1,2,3"
print(s.split(","))
```



#### 32.如何实现['1','2','3']变成"[1,2,3]

```python
[int(i) for i in ["1","2","3"]]
```



#### 33.比较a = [1,2,3]和b=[(1),(2),(3)]和c[(1,),(2,),(3,)]的区别？

```python
a 和 b 里面的元素都是int类型
c 里面的元素都是元组类型
```



#### 34.用一行代码生成[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

```python
  [i*i for i in range(1,11)]
  [i**2 for i in range(1,11)]
```



#### 35.常用字符串格式化有哪几种

```python
a = "python"  b = "java"
1. print("I like %s and %s" %(a, b))
2. print("I like {} and {}".format(a, b))
3. print(f"I like {a} and {b}")
```



#### 36.什么是断言(assert)应用场景

```python 
Python的assert是用来检查一个条件，如果它为真，就不做任何事。如果它为假，则会抛出AssertError并且包含错误信息
AssertError不是在测试参数时应该抛出的错误。
```



#### 37.有两个字符串a和b 每个字符串有逗号分隔的一些字符,按每个字符串的第一个值,合并a和b到c

```python
a=['a,1','b,3,22','c,3,4']
b = ['a,2','b,1','d,2']
按每个字符串的第一个值，合并a和b和c
c = ['a,1,2','b,3,22,1','c,3,4','d,2']
---------------------------------------------------------------------------------------
res = {}
for i in a:
    res[i[0]] = i.split(",")

for j in b:
    if j[0] in res:
        res[j[0]].append(j[2:])
    else:
        res[j[0]] = j.split(",")
c = [",".join(value) for value in res.values()]
print(c)
```

#### 38.有一个多层嵌套列表A=[1,2,[3,4,['434',...]]

```python 
def func(a):
    for i in a:
        if not isinstance(i, list):
            print(i)
        else:
        	func(i)
func(A)
```

#### 39.a=range(10),a[::3]的结果是

```python
py2: [0,3,6,9]
py3: range(0,10,3)
    
a = range(10),a[::-3]  
py2:[9,6,3,0]
py3:range(9,-1,-3)
```

#### 40.下面哪个命令可以从虚拟环境中退出 

```python
A:deactivate
B:exit
C:quit
D:以上均可
A
```

#### 41.将列表内的元素，根据位数合并成字典

```python
lst = [1,2,4,8,16,32,64,128,256,512,1024,32769,65536,4294967296]
dic = {}
for i in lst:
	if len(i) in dic:
        dic[len(str(i))].append(i)
    else:
        dic[len(str(i))] = [i]
结果：{1: [1, 2, 4, 8], 2: [16, 32, 64], 3: [128, 256, 512], 4: [1024], 5: [32769, 65536], 10: [4294967296]}
```

#### 42.请尽量用简洁的方法将二维数组转换成一堆数组

##### 例：转换前 lst = [[1,2,3],[4,5,6],[7,8,9]]

##### 转换后lst = [1,2,3,4,5,6,7,8,9]

```python
new_lst = []
for i in lst:
	new_lst.extend(i)
lst = new_lst
```

#### 43.将列表按下列规则排序，补全代码

##### 1.正数在前负数在后

##### 2.正数从小到大

##### 3.负数从大到小

##### 排序前[7,-8,5,4,0,-2,-5]   排序后[0,4,5,7,-2,-5,-8]

##### 补全代码：1.sorted(lst,key=lambda x:________)

```python
l = sorted(lst,key=lambda x:(x<0,abs(x)))
```

#### 44.哈希冲突回避算法有哪几种，分别有什么特点

```python
https://zhuanlan.zhihu.com/p/29520044
https://www.jianshu.com/p/4d3cb99d7580
```

#### 45.简述python的字符串驻留机制

```python
https://zhuanlan.zhihu.com/p/35362912  
我们都知道python中的引用计数机制，相同对象的引用其实都是指向内存中的同一个位置，这个也叫做“python的字符串驻留机制”
python的引用计数机制，并不是对所有的数字，字符串，他只对“  [0-9] [a-z] [A-Z]和"_"(下划线)  ”有效，当字符串中由其他字符比如“！ @ # ￥ % -”时字符驻留机制是不起作用的。
```

#### 46.以下代码输出是什么？list=['a','b','c','d','e'] print list[10:]

```
A. [ ]
B. 程序异常
C. ['a','b','c','d','e']
D 输出空

A
```

#### 47.python语言什么类型的数据才能作为字典的key

```python
A. 没有限制
B. 字母数字下划线
C. 字母
D. 可被hash的类型

D  不可变类型
```

#### 48.以下两段代码的输出一样吗，占用系统资源一样码，什么时候要用xrange代替range

##### for i in range(1):print i

##### for i in xrange(1):print i

```
输出一样，占用系统资源不一样，range是一个列表，xrange是一个可迭代对象
当range的数字比较大时，xrange会省很多空间
```

#### 49.如下代码段

```python
a = [1,2,3,[4,5],6]
b = a
c = copy.copy(a)
d = copy.deepcopy(a)
b.append(10)
c[3].append(11)
d[3].append(12)
请问a,b,c,d的值为

a = [1,2,3,[4,5,11],6,10]
b = [1,2,3,[4,5,11],6,10]
c = [1,2,3,[4,5,11],6]
d = [1,2,3,[4,5,12],6]
```

#### 50.现有字典d = {'a':26,'g':20,'e':20,'c':24,'d':23,'f':21,'b':25}请按照字段中的value字段进行排序

```python
res = sorted(d, key=lambda x:d[x])
```

#### 51.给定两个list A，B，请用python找出A,B中相同的元素和A,B中不同的元素

```python
A = [1,2,3,4,5,6]
B = [3,4,5,6,7,8,]
print(set(A)&set(B))
print(set(A)^set(B))
执行结果：
{3, 4, 5, 6}
{1, 2, 7, 8}
```

#### 52.下列叙述错误的是

```python
A. 栈是线性结构
B. 队列是线性结构
C. 线性列表是线性结构
D. 二叉树是线性结构
---------------------------------------------------------------
D #常用的线性结构有：线性表，栈，队列，双队列，数组，串。
  #常见的非线性结构有：二维数组，多维数组，广义表，树(二叉树等)，图
  #线性结构：一个有序数据元素的集合，数据元素之间是一对一关系的数据结构
  #非线性结构：没有对应关系的，一对多的，多对多的
```

#### 53.一个栈的输入序列为1,2,3,4,5，则下列序列中不可能是栈的输出顺序的是

```python
A. 1,5,4,3,2
B. 2,3,4,1,5
C. 1,5,4,2,3
D. 2,3,1,4,5
---------------------------------------------------------------C  #栈：先进后出
```

#### 54.下图哪些PEP被认为涉及到了代码规范

```python
PEP是Python Enhancement Proposals的缩写。一个PEP是一份为Python社区提供各种增强功能的技术规格，也是提交新特性，以便让社区指出问题，精确化技术文档的提案
A. PEP7
B. PEP8
关于代码的规范
C. PEP20
关于可读性的规范
D. PEP257
关于文档字符串的规范
---------------------------------------------------------------
B
https://blog.csdn.net/zV3e189oS5c0tSknrBCL/article/details/81463984
```

#### 55.下面哪些是python合法的标识符，哪些是python的关键字

```python
1. int32
2. 40XL
3. saving$
4. print
5. this
6. self
7.0x40L
8.true
9.big-daddy
10.True
11.if
12. do
13.yield
---------------------------------------------------------------
合法标识符：1,5,6,8,12
python关键字：4.10，11，13
python2中True和False是变量可以对其进行赋值
python3中True和False是关键字
```

#### 56.从0-99这100个数中随机取出10个，要求不能重复，可以自己设计数据结构

```python
import random
l  = random.sample(list(range(100)),10)
print(l)
print(set(random.sample(range(100),10)))

```

#### 57.python判断一个字典中是否有这些key：'AAA','BB','C','DD','EEE'(不使用for while)

```python
dic = {1:'1',2:'2','AA':'test'}
if "AAA" not in dic:
    if "BB" not in dic:
        if "C" not in dic:
            if "DD" not in dic:
                if "EEE" not in dic:
                    print("么得啊")
```

#### 58.有一个list['This','is','a','Boy','!'],所有元素都是字符串，对他进行大小写无关的排序

```python
lst = ['This','is','a','Boy','!']
ret =  sorted(lst,key=lambda x:x.upper())
print(ret)
答案：['!', 'a', 'Boy', 'is', 'This']

for index,i in enumerate(range(120)):
    print(index,chr(i))
    
print(ord("!"))  查询其ascii
```

#### 59.描述下dict的items()方法与iteritems()的不同

```
python字典的items方法作用：是可以将字典中的所有项，以列表方式返回。如果对字典项的概念不理解，可以查看Python映射类型字典基础知识一文。因为字典是无序的，所以用items方法返回字典的所有项，也是没有顺序的。
dic = {"k1":[1,2]}
print(dic.items())
结果：dict_items([('k1', [1, 2])])

python字典的iteritems方法作用：与items方法相比作用大致相同，只是它的返回值不是列表，而是一个迭代器。


Python 3.x 里面，iteritems() 和 viewitems() 这两个方法都已经废除了，而 items() 得到的结果是和 2.x 里面 viewitems() 一致的。
```

#### 60.请列举你所知道的python代码检测工具及他们间的区别

```python
pychecker是一个python代码静态分析工具，它可以帮助python代码找bug会对代码的复杂度提出警告
pulint高阶的python代码分析工具，分析python代码中的错误查找不符合代码风格标准



python代码检查工具：
Flake8 是由Python官方发布的一款辅助检测Python代码是否规范的工具，相对于目前热度比较高的Pylint来说，Flake8检查规则灵活，支持集成额外插件，扩展性强。Flake8是对下面三个工具的封装：
1）PyFlakes：静态检查Python代码逻辑错误的工具。
2）Pep8： 静态检查PEP8编码风格的工具。
3）NedBatchelder’s McCabe script：静态分析Python代码复杂度的工具。
不光对以上三个工具的封装，Flake8还提供了扩展的开发接口。

```

#### 61.介绍一下try except 的用法和作用

```
try....except执行try下的语句，如果发生异常，则执行过程跳到except语句，对每个except分支顺序尝试执行，如果异常与except中的异常组匹配，指行相应的语句.

```

#### 62.输入一个字符串，返回倒序排列的结果如：abcdef，返回fedcba

```python
s = 'abcdef'
print(s[::-1])

s =list(s)
s.reverse()
print("".join(s))
```

#### 63.阅读以下代码，并写出程序的输出结果

```python
alist = [2,4,5,6,7]
for var in alist:
    if var % 2 == 0:
        alist.remove(var)
alist的最终结果是？
----------------------------------------------------------------
[4,5,7]
```

#### 64.现有列表alist = [3,1,-4,-2],按照元素的绝对值大小进行排序

```python
alist = [3,1,-4,-2]
ret = sorted(alist,key=lambda x:abs(x))
print(ret)
```

#### 65.填空题

##### 1.表达式 3<4<5 是哪一个表达式的缩写()

```python
3 < 4 and 4 < 5
```

##### 2.获取python解释器版本的方法是（）

```python
python -V  cmd里面查看的方式

import sys   
print(sys.version)

```

##### 3.如果模块是被导入的，__ name __ 的值是（），如果模块是被直接执行的，__  	   __ name__的值是（）

```python
模块名字
'__main__'
```

##### 4.python的内存管理中，为了追踪内存中的对象，使用了（）这一简单技术

```python
python内部使用引用计数，来保持追踪内存中的对象，Python内部记录了对象有多少个引用，即引用计数，当对象被创建时就创建了一个引用计数，当对象不再需要时，这个对象的引用计数为0时，它被垃圾回收。所有这些都是自动完成，不需要像C一样，人工干预，从而提高了程序员的效率和程序的健壮性

```

##### 5.python的标准解释器是由C语言实现的，称为（），有java实现的被称为（）

```python
cpython  
jpython
```

##### 6.python中，（）语句能直接显示的释放内存资源

```python 
del
```

##### 7.python的乘方运算符是（）

```python
**
```

#### 66.现有字典mydict和变量onekey，请写出从mydict中取出onekey值的方法（mydict中是否存在onekey的键值不确定）

```python
mysict.get(onekey)
#有则返回值，没有则返回none
```

#### 67.现有一列表alist，请写出两种去除alist中重复元素的方法，其中

##### 1.要求保留原有列表中元素的排列顺序

```python
alist = [1,2,3,3,4,4,5,6]
lst = []
for i in alist:
    if i not in lst:
        lst.append(i)
print(lst)
```

##### 2.无需考虑原有列表中元素的排列顺序

```python
alist = [1,2,3,3,4,4,5,6]
list(set(alist))
```

#### 68.请描述unicode、utf-8、gbk等编码之间的区别

```python
ascii: 早期编码，只支持英文字母和一些符号，不支持中文
unicode: 万国码，能表示多种符号，Unicode只是一个符号集，一个中文占4个字节，英文占2个字节，浪费空间
utf-8: UTF-8就是在互联网上使用最广的一种unicode的实现方式,是一种变长的编码方式,根据不同的符号而变化字节长度.1中文占3字节，1英文占1字节
gbk 中文编码，1中文占2字节，1英文占1字节

a = 19
print(bin(a))
print(oct(a))   
print(hex(a))
执行结果：
0b10011
0o23
0x13
```

#### 69.哪些情况下，y != x - (x - y)会成立

```python
x,y是两个不相等的非空集合 #右边相当x,y的交集

x={1,2,3,4,5,6,7}
y={1,2,8}
print(x-(x-y)) {1, 2}
```

#### 70.用python实现99乘法表（两种不同的方法）

```python
1.print('\n'.join(['\t'.join(["%2s*%2s=%2s"%(j,i,i*j) for j in range(1,i+1)]) for i in range(1,10)]))

2.for i in range(1,10):
    for j in range(1,10):
        if j<i:
            print("{}*{}={}".format(i,j,i*j),end="  ")
        elif j==i:
            print("{}*{}={}".format(i, j, i * j))
            break
```

#### 71.获取list中元素个数和向末尾追加元素的方法分别是什么?

```python
len(),count()
append
```

#### 72.判断dict中有没有某个key的方法是什么

```python
dict = {'name':'dasd', 'age':10, 'Tel':110}
print(dict.has_key('name')) #python2中的方法 py3移除
print('name' in dict.keys())
print('name' in dict)
```

#### 73.l = range(100)

```pyhton
如何取第一个到第三个元素用的是  
如何取倒数第二个元素
如何取后十个
```

```python
l = range(100)
l1 = list(l)[0:3]
l2 = list(l)[-2]
l3 = list(l)[-10:]
```

#### 74.如何判断一个变量是否为字符串

```python
isinstance('dd',str)
type(变量)
```

#### 75.list和tuple的区别

```python
都是有序的list可修改 tuple不可修改
```

#### 76.a = dict(zip(('a','b','c','d','e'),(1,2,3,4,5))) a是什么

```python
print(dict(zip(('a','b','c','d','e'),(1,2,3,4,5))))
#{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}   可迭代的
```

#### 77.一行代码生成列表[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

```python
print([i**2 for i in range(1,11)])
```

#### 78.以下说法正确的是   B

```python
A continue是结束整个循环的执行     错误  结束当前循环进入下一循环
B 只能在循环体和switch中使用break语句 正确
C 再循环体内使用break语句或者continue语句的作用相同  错误
D 从多层循环嵌套中退出时只能使用goto语句 错 return exit
```

#### 79.读代码

```python
for i in range(5,0,-1):
    print(i)
执行结果：5,4,3,2,1

```

#### 80.写结果

```python
x = 'foo'
y=2
print(x+y)
会报错
```

#### 81.求结果

```python
kvps = {'1':'1','2':'2'}
theCopy = kvps  #相当于是赋值操作
kvps['1'] = 5
sum = kvps['1']+theCopy['1']
print(sum)
#10
```

#### 82.python实现tuple和list转化

```python
def func(*args):
    return args
print(func(*[1,2,3]))
强转
```

#### 83.type(1+2L*3.14)

```python
print(type(1+2L*3.14)) float类型
```

#### 84.k为整形 下列while循环执行的次数是

```python
k = 1000
count = 0
while k>1:
     print(k)
     k = k/2
     count += 1
 print(count)    10次
```

#### 85.以下什么是不合法的布尔表达式

```python
A x in range(6)  #代表一个范围
B 3=a       #错误 需要双等
C e>5 and 4==f
D (x-6) >5
```

#### 86.python不支持的数据类型有

```python
A char   不支持
B int
C float
D list 
```

#### 88.99(10进制)的八进制表示是什么

```python
print(oct(99))
0o143
```

#### 90.list对象alist=[{'name':'a','age':20},{'name':'b','age':30},{'name':'v','age':25}]按照age大小排序

```python
alist=[{'name':'a','age':20},{'name':'b','age':30},{'name':'v','age':25}]
ret =sorted(alist,key=lambda i:i['age'])
print(ret)
```

#### 91.关于python程序的运行性能方面,有什么手段能提升性能?

```python
1、使用多进程，充分利用机器的多核性能
2、对于性能影响较大的部分代码，可以使用C或C++编写
3、对于IO阻塞造成的性能影响，可以使用IO多路复用来解决
4、尽量使用Python的内建函数
5、尽量使用局部变量
6.减少函数调用次数,为了避免重复计算,不要把重复操作作为参数放入循环中
	while i < len(a):
        pass
7.采用映射替代条件查找
    映射(比如dict等)的搜索速度远快于条件语句(如if等)。
    #if查找
    if a == 1:
    b = 10
    elif a == 2:
    b = 20
    ...
    #dict查找，性能更优
    d = {1:10,2:20,...}
8、采用生成器表达式替代列表解析
```

#### 92.Python是如何进行内存管理的? Python的程序会内存泄露吗? 说说有没有什么发法阻止或检测内存泄露

```python
python内存管理机制 ( Pymalloc ) 包括三个方面：引用计数、垃圾收集、内存池。下面分别予以阐述。
1.  引用计数：python程序中使用的每个变量后台都有一个引用计数。赋值或调用操作，计数加一；相反，删除或移出窗口对象，计数减一。
2.  垃圾收集：将引用计数为0的对象所占有的内存空间释放。还有一个循环垃圾收集器，负责清理未引用的循环，如两个对象互相引用的情况。
3.  内存池：内存池是预先从内存中申请的内存块，当创建小于256 bits 的对象时，从内存池申请内存空间。创建大于256 bits 的对象从内存申请空间。释放内存时，来自内存池的内存空间返回给内存池。这样做的目的是为了减少内存碎片，提升效率。

什么是内存泄漏?
比方说,一台机器出现了内存泄漏的情况（内存 2G，htop 发现 Python 程序占用内存 200M，但是 free -m 发现真实使用内存达到了 1G ）。

Python也会内存泄露，Python本身的垃圾回收机制无法回收重写了del的循环引用的对象.检测方法:objgraph
1.程序员管理好每个python对象的引用，尽量在不需要使用对象的时候，断开所有引用
2.尽量少通过循环引用组织数据，可以改用weakref做弱引用或者用id之类的句柄访问对象
3.通过gc模块的接口可以检查出每次垃圾回收有哪些对象不能自动处理，再逐个逐个处理

https://www.jianshu.com/p/2b683cb5837c
```

#### 93.详细说说tuple,list,dict的用法,他们的特点

```python
元组tuple: 不可变,可嵌套任何数据类型,有序
	查: 按索引查
	增: 当元组的元素里有列表,字典时,可以对列表和字典增删改; 元组可以合并
        t1 = (1,2,3,[1,2,3],{1:'a'},{3,4},(1,2,3))
		t1[-2].append(5)
        print(t1)
        
    删: del 元组 (删除整个元组)
	其他: index,
         count,
         len
        
列表list:可变,不可哈希,有序
    增:append,
       insert,
       extend,
       列表+列表
    删:remove,
       pop(默认删除最后一个,可指定索引删除,有返回值),
       clear(清空),
       del lst[1](删除整个列表,可指定索引删除)
    改: lst[2]='a'
	查: 索引查,for循环
    其他: count,index,reverse,sort,len
        
字典dict:
    特点: 字典的键必须是不可变数据类型; 键唯一; 
    增: dic[“键”] = “值”  # 存在就修改,不存在就添加
        setdefault（“键”，“值”） # 存在就不修改
    删: pop(key) #有返回值
        popitem  # 随机删除,有返回值,以元组形式返回
        del # 可指定键删除
        clear
    改: dic[“键”] = “值”  # 存在就修改,不存在就添加
        update({})  #有就修改,没有就添加
    查: get(key), dic[key]
    其他: dic.keys()
          dic.values()
          dic.items()
    
```

#### 94.一个大小为 100G 的文件 etl_log.txt, 要读取文件中的内容, 写出具体过程代码?

```python
方法一:
with open("etl_log.txt",encoding='utf8') as f:
    for line in f:
        print(line,end='')
        
方法二: 分段读
    file_size = os.path.getsize("etl_log.txt")
    with open("etl_log.txt",encoding='utf8') as f:
        while file_size:
            content = f.read(1024)
            file_size -= len(content)
```

#### 95.已知 Alist = [1,2,3,1,2,1,3], 如何根据 Alist 得到 [1,2,3]

```python
l = []
for i in Alist:
    if i not in l:
        l.append(i)
print(l)
```

#### 96.已知 stra = 'wqedsfsdrfweeddfgdqw'

1. 如何获得最后两个字符

2. 如何获得第二个和第三个字符

   ```python
   1. print(stra[-2:])
   2. print(stra[1:3])
   ```

#### 97.Alist = ['a','b','c'] ,将 Alist 转化为 'a,b,c' 的实现过程

```python
print(','.join(Alist))
```

#### 98.已知 ip = '192.168.0.100' 代码实现提取各部分并写入列表

```python
l_ip = ip.split(',')
```

```python
# 如 10.3.9.12 转换规则为：
#         10            00001010
#           3            00000011 
#          9            00001001
#          12            00001100 
# 再将以上二进制拼接起来计算十进制结果：00001010 00000011 00001001 00001100 = ？
# def sum_ip(ip):
#     l = ip.split('.')
#     s = ''
#     for i in l:
#         b = bin(int(i))[2:]
#         b = (8-len(b)) * '0' + b
#         s += b
#     res = int(s,2)
#     return res
# ip = '10.3.9.12'
# res = sum_ip(ip)
# print(res)
```

#### 99.python 代码如何获取命令行参数?

```python
sys.argv():
    
	import sys
    user = sys.argv[1]
    pwd = sys.argv[2]
    print(user,pwd)
    命令行:
        C:\Users\Administrator>python D:\s17\面试题\面试题.py ning 123
        ning 123
        
getopt.getopt 

https://www.cnblogs.com/ouyangpeng/p/8537616.html
```

#### 100.写代码 

tupleA = ('a','b','c','d','e')

tupleB = (1,2,3,4,5)

RES = { 'a':1, 'b':2, 'c':3, 'd':4, 'e':5 }

写出由 tupleA 和 tupleB 得到 RES 的具体实现过程

```python
l = zip(tupleA,tupleB)
dic = {}
for i in l:
    dic[i[0]] = i[1]
print(dic)

print(dict(zip(tupleA,tupleB)))
```

#### 101.选出以下表达式表达正确的选项:

```pyhton
A. { 1:0,2:0,3:0}
B. {'1':0,'2':0,'3':0}
C. { (1,2):0, (4,3):0}
D. { [1,2]:0, [4,3]:0}
E. { {1,2}:0, {4,3}:0}

A,B,C
```

#### 102.what gets printed()?

```python
kvps = {'1':1,'2':2}
theCopy = kvps.copy()
kvps['1'] = 5
sum = kvps['1'] + theCopy['1']
print sum

A. 1
B. 2
C. 6
D. 10
E. An exception is thrown

C
```

#### 103.what gets printed()?

```python
numbers = [1,2,3,4]
numbers.append([5,6,7,8])
print len(numbers)

A.4
B.5
C.8
D.12
E.An exception is thrown

B.5
```

#### 104.what gets printed()?

```python
names1 = ['Amir','Barry','Chaies','Dao']
if 'amir' in names1:
    print 1
else: 
    print 2

A.1
B.2
C.An exception is thrown

B.2
```

##### 105.what gets printed()? Assuming python version2.x()

```python
print(type(1/2))

A.int
B.float
C.0
D.1
E.0.5

A,C
>>> print 1/2
0
>>> print(type(1/2))
<type 'int'>
>>> print 1.0/2
0.5
>>>
```

##### 106.以下用来管理Python库管理工具的是 

```python
A.API
B.PIP
C.YUM
D.MAVEN  #Maven的核心功能便是合理叙述项目间的依赖关系

B
```

#### 107.which numbers are printed() ?

```python
for i in range(2):
    print i
for i in range(4,6):
    print i

A.2,4,6
B.0,1,2,4,5,6
C.0,1,4,5
D.0,1,4,5,6,7,8,9
E.1,2,4,5,6

C.0,1,4,5
```

#### 108.求结果:

```python
import math
print (math.floor(5.5))

A.5
B.5.0
C.5.5
D.6
E.6.0

import math
print (math.floor(5.5)) #地板  5
print (math.ceil(5.5))  #天花板  6
#math的常用命令
gcd:返回x和y的最大公约数
pi:数字常量，圆周率
```

#### 109.关于Python 的内存管理,下列说法错误的是:

```python
A.变量不必事先声明
B.变量无需先创建和赋值而直接使用
C.变量无需指定类型
D.可以使用del释放资源

B

Python 是弱类型脚本语言，变量就是变量，没有特定类型，因此不需要声明。
但每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
用 del 语句可以释放已创建的变量（已占用的资源）。
```

#### 110.下面哪个不是 Python 合法的标识符?

```python
A.int32
B.40xl
C.self
D.name

B
```

#### 111.下列哪种说法是错误的

```python
A、除字典类型外，所有标准对象均可用于布尔测试
B、空字符串的布尔值是False
C、空列表对象的布尔值是False
D、值为0的任何数字对象的布尔值是False

A  空字典也可以用于布尔测试
dic = {}
print(bool(dic))
False
答案： A 
```

#### 112.下列表达式的值为True的是

```python
A、5+4j > 2-3j
not supported between instances of 'complex' and 'complex'
Python2 与 Python3 均不支持复数比较大小
B、3>2>2
False
C、(3,2)<("a","b")
not supported between instances of 'int' and 'str'
("3","2")<("a","b")
True
D、"abc">"xyz"
False
```

#### 113.关于Python的复数，下列说法错误的是

```python 
A、表示复数的语法是real + imagej
	Python 中复数的表示方法；
	a = 1 + 2j
	b= 1 + 2j
	print(a,b)
	print(a.imag,a.real)
    结果：(1+2j) (1+2j)
			2.0 1.0
B、实部和虚部都是浮点数
	复数的实部与虚部均为浮点数；
C、虚部后缀必须是j，且必须小写
	虚部的后缀可以是 “j” 或者 “J”；
D、方法conjugate（共轭的）,返回复数的共轭复数
	复数的 conjugate 方法可以返回该复数的共轭复数
    a = 1 + 2j
	print(a.conjugate())
    (1-2j)
答案：C
```

#### 114.关于字符串下列说法错误的是

```python 
A、字符应视为长度为1的字符串
	容量不同：字符常量只能是单个字符，字符串常量则可以含一个或多个字符。
	占用内存空间大小不同：字符常量占一个字节的内存空间，字符串常量占的内存字节数等于字符串中字节数加1。增加	的一个字节用来存放字符‘\0’,作为字符串的结束标志。
B、字符串以\0标志字符串的结束
C、既可以用单引号，也可以用双引号创建字符串
D、在三引号字符串中可以包含换行回车等特殊字符
答案：A
```

#### 115.以下不能创建一个字典的语句是

```python 
A、dic1={}
B、dic2={3:5}
C、dic3={[1,2,3]:"usetc"}
D、dic4={(1,2,3):"usetc"}
答案：C ，字典的键不能是可变数据类型
```

#### 116、python里面如何拷贝一个对象？（赋值，浅拷贝，深拷贝的区别）

```python
赋值操作：跟原对象一样的值与内存空间，相当于是完全赋值
l1 = [1,2,[3,4]]
l2= l1
print(id(l1),id(l1[2]),id(l2),id(l2[2]))
执行结果：2696598203336 2696598203144 
		2696598203336 2696598203144

浅拷贝： 只能赋值第一层，第二层的内存地址还是不能完全复制过来
python中以下这这几种方式来实现浅拷贝：
完全切片操作：B = A[:]
利用工厂函数：B = list(A) 或 B = dict(A) 等
使用copy模块的copy函数：B = copy.copy(A)
l1 = [1,2,[3,4]]
l2= l1.copy()
print(id(l1),id(l1[2]),id(l2),id(l2[2]))
执行结果：2537051663304 2537051663112 
		2537051734280 2537051663112

深拷贝：深层拷贝，与原对象完全无关
import copy
l1 = [1,2,[3,4]]
l2= copy.deepcopy(l1)
print(id(l1),id(l1[2]),id(l2),id(l2[2]))
执行结果：2486777490376 2486777490184 
		2486777561352 2486777561288
```

#### 117、描述在python中的元组，列表，字典的区别，并且分别写一段定义，添加，删除，操作的代码片段

```python
  元组tuple: 不可变,可嵌套任何数据类型,有序
	查: 按索引查
	增: 当元组的元素里有列表,字典时,可以对列表和字典增删改; 元组可以合并
        t1 = (1,2,3,[1,2,3],{1:'a'},{3,4},(1,2,3))
		t1[-2].append(5)
        print(t1)
        
    删: del 元组 (删除整个元组)
	其他: index,
         count,
         len
        
列表list:可变,不可哈希,有序
    增:append,
       insert,
       extend,
       列表+列表
    删:remove,
       pop(默认删除最后一个,可指定索引删除,有返回值),
       clear(清空),
       del lst[1](删除整个列表,可指定索引删除)
    改: lst[2]='a'
	查: 索引查,for循环
    其他: count,index,reverse,sort,len
        
字典dict:
    特点: 字典的键必须是不可变数据类型; 键唯一; 
    增: dic[“键”] = “值”  # 存在就修改,不存在就添加
        setdefault（“键”，“值”） # 存在就不修改
    删: pop(key) #有返回值
        popitem  # 随机删除,有返回值,以元组形式返回
        del # 可指定键删除
        clear
    改: dic[“键”] = “值”  # 存在就修改,不存在就添加
        update({})  #有就修改,没有就添加
    查: get(key), dic[key]
    其他: dic.keys()
          dic.values()
          dic.items()
```

#### 118、选择结果

```python
names1 = ['Amir','Barry','Chaies','Dao']
names2 = names1
names3 =  names1[:]
names2[0]='Alice'
names3[1]='Bob'
sum = 0
for ls in (names1,names2,names3):
    if ls[0]=='Alice':
        sum+=1
    if ls[1]=='Bob':
        sum+=10
print(sum)

A、11
B、12
C、21
D、22
E、23
答案：B  
names1
['Alice', 'Barry', 'Chaies', 'Dao'] 
names2   此时是赋值，相当于完全复制，所以其内存空间与值与names1是完全相同的
['Alice', 'Barry', 'Chaies', 'Dao'] 
names3   此时是浅拷贝，所以相当于只拷贝到表层，names3[1]='Bob' 时只改变了names3 中的值
['Amir', 'Bob', 'Chaies', 'Dao']  
```

#### 119.下列程序输出结果是

```python
x = True
y = False
z = False
if x or y and z:
    print('yes')
else:
    print('no')
 答案：yes
```

#### 120、1 or 2和1 and 2输出分别是什么 为什么

```python
1 or 2    or判断里面，第一值为真，取第一个就可以  1
1 and 2   and里，必须满足都为真，所以取第二个值   2
```

#### 121、1<(2==2)和1<2==2的结果分别是什么 ，为什么

```python
1<(2==2)  为False
1<2==2  由于python中不支持连续判断，所以会拆分成 1<2 and 2==2 所以结果为True
```

#### 122、如何打乱一个排好序的list对象alist

```python
import random
alist=[1,2,3,4]
random.shuffle(alist)
print(alist)
执行结果：[1, 3, 4, 2]
```

#### 123、如何查找一个字符串中特定的字符 find和index的差异

```python
find
如果字符串查找不存在的，返回-1
index
如果字符串查找不存在的，报错
```

#### 124.把aaabbcccd这种形式的字符串压缩城a3b2c3d1这种形式

```python
s="aaabbcccd"
lst=[]
for i in s:
    if i not in lst:
        lst.append(i)
        lst.append(str(s.count(i)))
msg="".join(lst)
print(msg)
运行结果：a3b2c3d1


s = "aaabbcccd"
dic ={"name":[]}
for i in s:
    if i not in dic["name"]:
        dic["name"].append(i)
        dic["name"].append(str(s.count(i)))
print("".join(dic["name"]))
```

#### 125.python一个数如果恰好等于他的因子之和 这个数就成为完数例如6=1+2+3 找出1000以内所有的完数

```python 
def func(num):
    res = {1}
    max_divisor = num // 2
    for i in range(2, max_divisor+1):
        div, mod = divmod(num, i)
        if mod == 0:
            res.add(i)
    return res
for i in range(1,1001):
    res = func(i)
    if sum(res) == i:
        print(i)
        
        
def func(num):
    res = {1}
    max_divisor = num // 2
    for i in range(2, max_divisor+1):
        if max_divisor<=i:
            break
        div, mod = divmod(num, i)
        if mod == 0:
            max_divisor = div
            res.add(i)
            res.add(div)
    return res

for i in range(1,1001):
    res = func(i)
    if sum(res) == i:
        print(i)
```

#### 126.输入一个字符串 输出这个字符串中字符的所有组合

```python
列如 输入字符串'1,2,3'则输出位1，2，3，12，13，23，123（组合数不考虑顺序 所以12，21是等价的）

import math
def func(eg):
    # 构建原子性列表及全部原子集合(即最后一层结果)
    atom_list = [set(i) for i in eg.split(',')]
    all_atom_set = set(eg.split(','))

    # 计算元素个数
    length = len(atom_list)

    # 构建中间字典，key值为包含的字符个数，如第一层{1: [{'1'}, {'2'}, {'3'}，{'4'}]}
    res_dict = dict()
    res_dict[1] = atom_list
    res_dict[length] = [all_atom_set]

    if length > 2:
        # 在第一层到总层数的一半时，每一层的结果都是上一层结果基础上分别对原子列表中的每个原子（已包含的去除）求并集（如一层的{1}对应二层的结果为{'1', '2'}, {'1', '3'}，{'1', '4'}）
        # 在总层数的一半到总层数时，每一层的结果都是全部原子集合对总层数-当前循环层数每个集合的差集，如第3层的结果就是第4层（假设总共4层）分别对第1层集合的差集
        for i in range(2, math.ceil(length/2)+1):
            for j in res_dict[i-1]:
                for k in atom_list:
                    if not k & j:  # 循环上层列表中的集合不包含本次要加入的原子集合
                        tem_set = j | k  # 构建临时集合，进行下一步操作
                        if i not in res_dict:  # 新一层的字典初始化
                            res_dict[i] = [tem_set]
                            res_dict[length-i] = [all_atom_set-tem_set]
                        elif tem_set not in res_dict[i]:  # 加入
                            res_dict[i].append(tem_set)
                            res_dict[length-i].append(all_atom_set-tem_set)

        # 在上层循环中没有倒数第二层结果，需要单独添加
        res_dict[length - 1] = []
        for i in res_dict[1]:
            res_dict[length - 1].append(all_atom_set-i)

    # 输出
    for i in range(1,length+1):
        for j in res_dict[i]:
            print(''.join(j), end=',')

func('1,2,3,4,5')




def func(list,num=0,l=[]):
    if num < len(list):
        for x in range(0,len(list)):
            for i in range(0,len(list)):
                if x != x+i and x+i+num<=len(list)-1:
                    str = list[x] + list[x + i]
                    for n in range(1, num + 1):
                        str = str + list[x + i + n]
                    l.append(str)
        num += 1
        func(list,num)
    l =list+l
    return l
a = '1,2,3'
list = a.split(",")
print(func(list))


l = ['1','2','3']
lst = []
r = 1
while r < len(l):
    count = 0
    for i in range(count,len(l)-1):
        count += 1
        for j in range(count,len(l)): 
            if l[j] == l[i]:
                count += 1
                continue
            if j+r <= len(l):
                j = ''.join(l[j:j+r])
                lst.append(l[i]+j)
            else:
                continue
    r += 1
lst.extend(set(l))
print(lst)
```

#### 127、执行以下代码后，i 和 n 的值为

```python
1.int i = 10
2.int n = i++%5
A、10,0
B、10,1
C、11,0
D、11,1

i++ 是后加加，在表达式内不自增
int n= i++%5; // i=10进入，n=0，出了表达式 才自增 i=11;  

int i=11
int m= ++i%5; // i=11 进入, 前加加，则先增1，i=12, m=2, 出表达式 i 维持12
答案是：C

```

#### 128.执行以下代码段后，X的值为

```python
1. int x =10;
2. x+=x-=x-x;
A、10
B、20
C、30
D、40

因为赋值运算符（+=，-=）没有算术运算符的运算优先级高，所以先算x-x，即：
10-10=0。表达式变为x+=x-=0，再按顺序执行赋值运算符。即x+=x，即x=x+x，x=20；
```

#### 129.对于一个非空字符串，判断其是否可以有一个子字符串重复多次组成，字符串只包含小写字母且长度不超过10000

```python
例：
1.输入"abab"
2.输出True
3.样例解释：输入可由“ab”重复两次组成
例：
1.输入"abcabc"
2.输出True
3.样例解释：输入可由“abc”重复三次组成
例：
1.输入"aba"
2.输出False

答案：
class Solution(object):                         
    def repeatedSubstringPattern(self, s):      
        n=len(s)                                
        for i in range(1,n//2+1):               
            if n%i==0:                          
                a=s[:i];j=i                     
                while j<n and s[j:j+i]==a:      
                    j += i                      
                if j==n:return True             
        return False                            
ret = Solution()                                
print(ret.repeatedSubstringPattern("abcabc"))   
                                                

    
def c(s):
    n = len(s)
    for i in range(1, n // 2 + 1):
        if n % i == 0:
            a = s[:i]
            j = i
            while j < n and s[j:j + i] == a:
                j += i
            if j == n: return True
    return False
print(c("abcabc"))
```



























​       