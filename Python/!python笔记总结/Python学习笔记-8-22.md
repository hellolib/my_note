# 					Python学习笔记

博客地址:https://www.cnblogs.com/bigox/

## 第一章 计算机基础

### 1.1  硬件

计算机基本的硬件由：CPU / 内存 / 主板 / 硬盘 / 网卡  / 显卡 等组成，只有硬件但硬件之间无法进行交流和通信。需要操作系统进行协调工作

### 1.2 操作系统

操作系统用于协同或控制硬件之间进行工作，常见的操作系统有那些:

- windows,个人办公系统,图形窗口
- linux
  - centos 【公司线上一般用】
  - redhat 收费
  - unbutu 图形界面较好
- mac,个人开发使用,人机交互较好

### 1.3  解释器或编译器

编程语言的开发者写的一个工具，将用户写的代码转换成010101交给操作系统去执行。

#### 1.3.1  解释和编译型语言

解释型语言就类似于： 实时翻译，代表：Python / PHP / Ruby / Perl

编译型语言类似于：说完之后，整体再进行翻译，代表：C / C++ / Java / Go ...

### 1.4 软件（应用程序）

软件又称为应用程序，就是我们在电脑上使用的工具，类似于：记事本 / 图片查看 / 游戏

### 1.5 进制

对于计算机而言无论是文件存储 / 网络传输输入本质上都是：二进制（010101010101），如：电脑上存储视频/图片/文件都是二进制； QQ/微信聊天发送的表情/文字/语言/视频 也全部都是二进制。

进制：

- 2进制 bin，计算机内部。
- 8进制 oct
- 10进制 int ，人来进行使用一般情况下计算机可以获取10进制，然后再内部会自动转换成二进制并操作。
- 16进制 hex ，一般用于表示二进制（用更短的内容表示更多的数据）。

## 第二章 Python入门

### 2.1 环境的安装

- 解释器：py2 / py3 （环境变量）
- 环境安装参见<https://blog.csdn.net/ling_mochen/article/details/79314118>
- 开发工具：pycharm

### 2.2 编码

#### 2.2.1 编码基础

- ascii:只针对英语编码,8位表示一个东西,共有2**8个,unicode 分为 ecs2 (2字节) 和 ecs4 (4字节).
- unicode:万国码,32位表示一个东西,共有2**32个;
- utf-8:给unicode压缩优化,用尽量少的位数表示一个东西;(8意思是8的倍数),3个字节表示中文.
- gbk:两个字节表示中文.
- gb2312

#### 2.2.2 python编码相关

输入

- py2:   	num = raw_input("提示输入内容")
- py3 :          num= input("提示输入内容")

输出

- py2:	print "小钻风"
- py3:        print("小钻风")

对于Python默认解释器编码：

- py2： 默认解释器编码ascii
- py3： 默认解释器编码utf-8

如果想要修改默认编码，则可以使用：

```python
# -*- coding:utf-8 -*- 
```

注意：对于操作文件时，要按照：以什么编写写入，就要用什么编码去打开。

### 2.3 变量

问：为什么要有变量？

为某个值创建一个“外号”，以后在使用时候通过此外号就可以直接调用。

- 变量要求

1. 变量只能由数字,字母和下划线组成
2. 数字不能开头,不能纯数字
3. 不能包含python关键字
4. 见名知意
5. 命名方法下划线命名,例:big_ox

### 2.4 if条件判断

基本结构

```
if 条件 :
	结果
else:
	结果
```

### 2.5while循环

1. while循环基本结构;

   ```python
   while 条件:   
       结果
       # 如果条件为真，那么循环则执行
       # 如果条件为假，那么循环不执行
   ```

2. debug模式显示每一步运行结果;

3. 关键字

- **break       **              #终止**当前**循环;
- **continue **(继续)    #如果碰到continue,则退出当前循环,立即回到while条件位置.

4. ~~*while else   #while条件不再满足时执行else.*~~

### 2.6for循环

(公共功能)

- for循环基本结构

  for 新变量 in 原变量:

  ​		结果                                ----------------循环取出原变量的字符赋值给新变量

```python
name = 'bigox'
for new_name in name:
    print(new_name)
```

- for循环中break和continue用法一样
- 在使用循环中,有穷尽的优先考虑for循环,无穷尽的考虑while循环

range界限

```python
#  range(1,5,1)    #第一个参数是范围的起始位置,第二个参数是范围的结束位置,第三个为步长,默认为1,取值顾首不顾尾.
for num in range(1,9):
	print(num)          #=======>输出为1-8
```

### 2.7运算符(特殊)

1. 算数运算

   - %取余
   - **幂
   - //整除

2. 比较运算

   - ==比较对象是否相等
   - !=不等于

3. 赋值运算 

   - +=  加法赋值: c+=a  <==>   c =c+a

4. **逻辑运算**

   - **bool类型数字0和空字符串''是False,其余是True.**

   1. **and  "与"**

      ```
      v  = 1 and 9   =====>  v = 9   #如果第一个值转换为布尔值时如果为True,则v=第二个值;
      v1 = 0 and 1   =====>  v = 0   #如果第一个值转换为布尔值时如果为False,则v=第一个值;
      v1 = 0 and ''  =====>  v = 0   #如果有多个and,从左到右进行判断.
      ```

   2. **or  "或"**

      ```
      v  = 1 or 9   =====>  v = 1    #如果第一个值转换为布尔值时如果为True,则v=第一个值;
      v1 = 0 or 1   =====>  v = 1    #如果第一个值转换为布尔值时如果为False,则v=第二个值;
      v1 = 0 or ''  =====>  v = ''   #如果有多个or,从左到右进行判断.
      ```

   3. **not   "非"**

   - **在没有()的情况下,not优先级大于and,and优先级大于or,即 () >not >and > or.同一优先级从左往右计算.**
   - 先进行数学运算,在进行逻辑运算

   补充:

   in / not in

   ```python
   #示例:(是否包含敏感字符)
   while True:
       text = input('请输入你要说的话:')
       if '傻逼'in text:
           print('包涵敏感字符')
   ```

## 第三章 数据类型

### 3.0 bytes 字节类型

- 一般用于数据存储和网络传输

- 字符串都是由unicode编码

  ```python
  v = 'alex'.encode('utf-8')  # 将字符串转换成字节（由unicode编码转换为utf-8编码）
  v = 'alex'.encode('gbk')    # 将字符串转换成字节（由unicode编码转换为gbk编码）
  ```


### 3.1 整型（int）

#### 3.1.1 整型的长度

py2中有：int/long

py3中有：int （int/long）

#### 3.1.2 整除

py2和py3中整除是不一样。

py2除法只能取整数,py3除法取全部值

py2除法取全部值,需要加代码:
from __future__ improt division
value = 3/2
print(value)

### 3.2 布尔（bool）

布尔值就是用于表示真假。True和False。

其他类型转换成布尔值：

- str()
- ...

对于：None /  "" / 0 .... -> false

### 3.3 字符串（str）

#### 3.3.1字符串格式化:

 字符串是写代码中最常见的，python内存中的字符串是按照：unicode 编码存储。对于字符串是不可变。

1. \n换行符

2. 基本格式

   ```python
   name = input('请输入姓名:')
   age = input('请输入年龄:')
   job = input('请输入工作:')
   hobby = input('请输入爱好:')
   msg = '''
   ---------- info of bigox ----------
   Name	:%s
   Age		:%s
   Job		:%s
   Hobby	:%s
   ------------- end -------------'''
   print(msg%(小钻风,500,it,girl,))
   ```

3. - %s	表示字符串;
   - %d        表示数字;
   - %%        字符串格式化时表示百分数.

- 扩展:

  - %s

  ```python
  #方式1:%s与替换值相对性,构造元组
  msg = "我是%s,年龄%s" %('alex',19,)
  print(msg)
  
  #方式2:%s中间加一个变量,最后构造字典匹配值
  msg = "我是%(n1)s,年龄%(n2)s" %('alex',19,)
  print(msg)
  ```

  - format

  ```python
  # v1 = "我是{0},年龄{1}".format('alex',19)
  v1 = "我是{0},年龄{1}".format(*('alex',19,))
  print(v1)
  
  # v2 = "我是{name},年龄{age}".format(name='alex',age=18)
  v2 = "我是{name},年龄{age}".format(**{'name':'alex','age':18})
  print(v2)
  ```

  

**注意要点:**

1.input 输入内容全是字符串.

2.py版本输入格式区别

- py2: name = raw_input('请输入姓名')
- py3: name = input('请输入姓名')

#### 3.3.2字符操作方法

字符串 str:upper/lower/isdecimal/strip/replace/split/startswith/endswith/format/encode/join

1. 大写： upper

   ```
   v = 'ALEX'
   v1 = v.upper()
   print(v1)
   v2 = v.isupper() # 判断是否全部是大写
   print(v2)
   ```

2. 小写：lower

   ```
   v = 'alex'
   v1 = v.lower()
   print(v1)
   v2 = v.islower() # 判断是否全部是小写
   print(v2)
   
   ############ 了解即可
   v = 'ß'
   # 将字符串变小写（更牛逼）
   v1 = v.casefold()
   print(v1) # ss
   v2 = v.lower()
   print(v2)
   ```

3. 判断是否是数字： isdecimal

   ```
   v = '1'
   # v = '二'
   # v = '②'
   v1 = v.isdigit()  # '1'-> True; '二'-> False; '②' --> True
   v2 = v.isdecimal() # '1'-> True; '二'-> False; '②' --> False
   v3 = v.isnumeric() # '1'-> True; '二'-> True; '②' --> True
   print(v1,v2,v3)
   # 推荐用 isdecimal 判断是否是 10进制的数。
   
   # ############## 应用 ##############
   v = ['alex','eric','tony']
   for i in v:
       print(i)
   num = input('请输入序号：')
   if num.isdecimal():
       num = int(num)
       print(v[num])
   else:
       print('你输入的不是数字')
   ```

4. 去空白+\t+\n + 指定字符串.strip()

   ```python
   v1 = "alex "
   print(v1.strip())
   
   v2 = "alex\t"
   print(v2.strip())
   
   v3 = "alex\n"
   print(v3.strip())
   
   v1 = "alexa"
   print(v1.strip('al'))
   ```

5. 替换 replace

   ```
   #.replace()
   message = input('请输入"大傻逼"')
   new_mes = message.replace('傻逼','**')
   print(new_mes)
   #.replace('原字符','替换字符','替换个数')
   ```

6. .startswith()  判断是否以()开头,输出值为bool类型

   ```python
   name = 'bigox'
   print(name.startswith('big'))
   ```

7. .endswith() 同.startswith()用法相同
8. .format()格式(同字符串格式化)

```python
name = '我叫:{0},年龄:{1}'.format('bigox',24)
print(name)
```

9. .encode() :编码转换

```python
name  = '刘'              #解释器读取到内存后,按照unicode编码存储:8字节.

print(name.encode('utf-8'))    #转化为utf-8编码
```

​	10 .join()循环每个元素,并在元素之间加入连接符.

```python
name = 'bigox'
new_name = '_'.join(name)
print(new_name)       #输出结果为  b_i_g_o_x
```

11. 分割:字符串切割后形成列表

    ```python
    #.split()
    name = 'abcdefg'
    new_name = name.split('d')
    print(new_name)
    #.split('分割点',分割次数)
    ```

    - 分割时引用字母或数字分割时该字母数字消失,如果是标点符号,则不消失.

### 3.4 列表list

列表独有方法

1. ** .append()列表最后追加元素**

   ```python
   lst = ["麻花藤", "林俊杰", "周润发", "周芷若"] 
   print(lst) 
   lst.append("wusir") 
   print(lst)
   ```

2. **.insert**在指定的索引位置插入元素

   ```python
   lst = ["麻花藤", "林俊杰", "周润发", "周芷若"] 
   print(lst) 
   lst.insert(1,"wusir") 
   print(lst)
   ```

3. **.remove()**指定元素删除

   .remove("指定元素")

4. **.pop()**删除索引元素:()如果没有索引值的话默认删除出最后一个

   .pop(索引)

5. **.clear()**清空

6. del  list[ :]切片删除

7. **.extend**()添加

   ```python
   li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]
   s = 'qwert'
   li.extend(s)
   print(li)
   #---------------------------
   lst = ["王志文", "张一山", "苦海无涯"]
   lst.extend(["麻花藤", "麻花不疼"])
   print(lst)
   ```

8. reverse反转

```
v = [1,2,3,4,5,6]
v.reverse()
print() #[6, 5, 4, 3, 2, 1]
```

9. sort排序

```
v = [1,3,7,4,5,6]
v.sort()
print() 	#[1, 3, 4, 5, 6, 7]
#v.sort()	从小到大排序(默认)
#v.sort(reverse=True)  从大到小排序
```

- 列表转字符串:

  ```
  nums = [11,22,33,44]
  for a in range(0,len(nums)):
      nums[a] = str(nums[a]) 
  result = ''.join(nums)
  print(result)
  ```

  3.5元组tuple

- 元组为不可变类型

- 元组子元素不可变,而子元素内部的子元素是可以变的,取决于元素是否为可变对象
- 元组中如果只有一个元素,一定要添加一个逗号,否者不是元组(1,)
- 可嵌套

### 3.5字典dict

- 字典:帮助用户表示事物的信息(事物有多个属性)

- 基本格式:**字典键的数据类型不能为list和tuple,值可以为任何类型.**

  ```python
  dictionary = {'键':'值','键':'值','键':'值'}
  ```

独有功能:

```pyhton
info = {"name":'刘伟达','age':18,'gender':'男','hobby':'同桌'}
```

- 获取字典info所有键.keys()

  ```pytho
  for i in info.keys():
  	print(i)
  ```

- 获取字典info所有值.values()

  ```python
  for i in info.values():
  	print(i)
  ```

- 获取字典info所有的键值对.items()

  ```python
  for i in info.items():
  	print(i)
  ```

- .get()取值

  ```
  info = {'k1':'v1','k2':'v2'}
  a = info.get('k1')
  print(a)	#v1
  info2 = ['11111']
  b = info.get('11111',22222)
  print(b)	#22222
  ```

- .update()更新,存在就更改,不存在增加

  ```
  info = {'k1':'v1','k2':'v2'}
  info.update({'k1':'v0','k3':'v3'})
  print(info)	#{'k1': 'v0', 'k2': 'v2', 'k3': 'v3'}
  ```

****判断是否存在敏感字符

- in/not in 是否包涵(输出类型为bool类型)

  str/list/tuple/dict都适用

  ```python
  # # 经典例题 # #
  # 让用户输入任意字符串，然后判断此字符串是否包含指定的敏感字符。
  
  #char_list = ['利奇航','堂有光','炸展会']
  #content = input('请输入内容：') # 我叫利奇航  / 我是堂有光  / 我要炸展会
  
  char_list = ['利奇航','堂有光','炸展会']
  i = True
  content = input('请输入内容:')
  for mes in char_list:
      if mes in content:
      	i = False
         	break
  if i:
      print(无敏感字符)
  else:
      print(包含敏感字符)
      
  ```

- **字典扩展**-**有序字典****Orderedict**

  ```python
  from collections import OrderedDict
  
  info = OrderedDict()
  info['k1'] = 123
  info['k2'] = 456
  
  print(info.keys())
  print(info.values())
  print(info.items())
  ```

  

### 3.6集合set

- 在集合中True与数字1重复,False与数字0重复
- 列表, 字典和集合为可更改类型,不可哈希,不能存放在集合当中.

**集合独有功能,**

- .add() 添加

- .discard()删除

- 要修改,需要先删除再添加

- .clear()清空

- .update()

  ```python
  info = {'k1','k2'}
  info.update({'k1','k3','v3'})
  print(info)	#{'k1','k2','k3','v3'}
  ```

- .intersection() 交集

  ​	***命令后的  () 可以是集合,也可以是列表.***

  ```python
  info = {1,2,3}
  print(info.intersection({1,3,4}))	#{1,3}
  ```

- .union()并集

  ```python
  info = {1,2,3}
  print(info.union({1,3,4}))	#{1,2,3,4}
  ```

- .difference()差集

  ```python
  info = {1,2,3}
  print(info.union({1,3,4}))	#{2,4}
  ```

### 3.7 公共功能

- len
- 索引
- 切片
- 步长
- for循环

- 列表, 字典和集合为可更改类型,不可哈希,不能存放在集合当中.也不能作为字典的键.

### 3.8内存相关

- id(查看内存地址)

- 赋值更改内存地址,内部变更改变量的值

- 内存地址

  ```python
  a = 1
  b = 1
  id(a) = id(b)
  #按理说a与b的id不该一样,但是在python中,为了提高运算性能,对某些特殊情况进行了缓存.(小数据池)缓存对象:
  1. 整型：  -5 ~ 256 
  2. 字符串："alex",'asfasd asdf asdf d_asdf '       ----"f_*" * 3  - 重新开辟内存。
  ```

- == 与is 区别

  - ==比较的是值是否一致
  - is 比较内存地址是否一致

- 对str,int,bool,tuple不可变数据类型深浅拷贝都一样,对于list,dict,set可变数据类型才有区别

  ```python
  ############## 示例 ############
  v1 = 'alex'
  import copy                    #固定格式
  v2 = copy.copy(v1)
  print(id(v1),id(v2))
  ```

- **浅拷贝 copy.copay()**

  - **拷贝第一层.**

- **深拷贝 copy.deepcopy()**

  - **拷贝嵌套层次中的所有可变类型**

### 3.9各数据类型常用操作总结

**一.数字/整型int**

- int()强行转化数字

**二.bool类型False&True**

- bool()强行转化布尔类型.
- 0,None,及各个空的字符类型为False.其余均为Ture.

**三.字符串str**

- str()强行转化字符串

  ```
  #列表转化字符换
  nums = [11,22,33,44]
  for a in range(0,len(nums)):
      nums[a] = str(nums[a]) 
  result = ''.join(nums)
  print(result)
  ```

- .upper()转化大写

  ```
  name = 'abc'
  new_name = name.upper()
  print(new_name)  
  ```

- .lower()转化小写

  ```
  name = 'ABC'
  new_name = name.lower()
  print(new_name)
  ```

- .replace()替换

  ```
  message = input('请输入"大傻逼"')
  new_mes = message.replace('傻逼','**')
  print(new_mes)
  #.replace('原字符','替换字符','替换个数')
  ```

- .strip()去首尾空格

  ```
  name = ' abc '
  new_name = name.strip()
  print(new_name)
  #.rstrip()去除右侧空格   .lstrip()去除左侧空格
  ```

- .split()分割

  ```
  name = 'abcdefg'
  new_name = name.split('d')
  print(new_name)
  #.split('分割点',分割次数)
  ```

- .isdecimal()判断是否可以转化位数字

  ```
  while True:
      num = input('请输入内容:')      
      num1= num.isdigit()             #print(num1) 数字的话输出True,非数字输出FALSE            
      if num1:
          print('你输入正确')
          break
      else:
          print('请输入数字')
  ```

- .startswith()  判断是否以()开头,输出值为bool类型

  ```
  name = 'bigox'
  print(name.startswith('big'))
  ```

- endswith() 判断是否以()结尾,输出值为bool类型 同.startswith()用法相同

- .format()格式(同字符串格式化)

  ```
  name = '我叫:{0},年龄:{1}'.format('bigox',24)
  print(name)
  
  ```

- .encode() :编码转换

  ```
  name  = '刘'              #解释器读取到内存后,按照unicode编码存储:8字节.
  print(name.encode('utf-8'))    #转化为utf-8编码
  
  ```

- .join()循环每个元素,并在元素之间加入连接符.

  ```
  name = 'bigox'
  new_name = '_'.join(name)
  print(new_name)       #输出结果为  b_i_g_o_x
  
  ```

**四.列表list**

- 列表转换list()

  ```
  #列表转化字符换
  nums = [11,22,33,44]
  for a in range(0,len(nums)):
      nums[a] = str(nums[a]) 
  result = ''.join(nums)
  print(result)
  
  ```

- .pop(索引)

  a = li.pop(2) #在列表中删除,并将删除的此数据赋值给a

  ```
  name = ['bigox','xo','ox']
  name.pop(1)
  print(name)
  
  ```

- del 列表 [索引]

  ```
  name = ['bigox','xo','ox']
  del name[0:2]
  print(name)
  
  ```

- .append()列表最后追加元素

  ```
  lst = ["麻花藤", "林俊杰", "周润发", "周芷若"] 
  print(lst) 
  lst.append("wusir") 
  print(lst)
  
  ```

- .insert()在指定的索引位置插入元素

  ```
  lst = ["麻花藤", "林俊杰", "周润发", "周芷若"] 
  print(lst) 
  lst.insert(1,"wusir") 
  print(lst)
  
  ```

- remove()**指定元素删除

  ```
  name = ['bigox','xo','ox']
  name.remove(xo)
  print(name)
  
  ```

- .clear()**清空

- .extend**()添加

  ```python
  li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]
  s = 'qwert'
  li.extend(s)
  print(li)
  #---------------------------
  lst = ["王志文", "张一山", "苦海无涯"]
  lst.extend(["麻花藤", "麻花不疼"])
  print(lst)
  
  ```

- .reverse()反转

  ```
  v = [1,2,3,4,5,6]
  v.reverse()
  print() #[6, 5, 4, 3, 2, 1]
  
  ```

- .sort排序

  ```
  v = [1,3,7,4,5,6]
  v.sort()
  print() 	#[1, 3, 4, 5, 6, 7]
  #v.sort()	从小到大排序(默认)
  #v.sort(reverse=True)  从大到小排序
  
  ```

**五.元组tuple**

- 强制转换：

  - tuple('adfadfasdfasdfasdfafd')

    ```python
    v1 = tuple('adfadfasdfasdfasdfafd')
    print(v1) # ('a', 'd', 'f', 'a', 'd', 'f', 'a', 's', 'd', 'f', 'a', 's', 'd', 'f', 'a', 's', 'd', 'f', 'a', 'f', 'd')
    
    ```

  - tuple([11,22,33,44])

    ```python
    v1 = tuple([11,22,33,44])
    print(v1)  # (11, 22, 33, 44)
    
    ```

- 元组子元素不可变,而子元素内部的子元素是可以变的,取决于元素是否为可变对象

- 元组中如果只有一个元素,一定要添加一个逗号,否者不是元组

**六.字典dict**

- 字典键的数据类型不能为list和tuple,值可以为任何类型.

- .keys()取键

  ```
  for i in info.keys():
  	print(i)
  
  ```

- .values()取值

  ```
  for i in info.values():
  	print(i)
  
  ```

- .items()取键值对

  ```
  for i in info.items():
  	print(i)
  
  ```

- .get()以键取值,如果键不存在返回原定结果

  ```
  info = {'k1':'v1','k2':'v2'}
  a = info.get('k1')
  print(a)	#v1
  info2 = ['11111']
  b = info.get('11111',22222)
  print(b)	#22222
  
  ```

- .update()更新_存在覆盖更新,不存在添加

  ```
  info = {'k1':'v1','k2':'v2'}
  info.update({'k1':'v0','k3':'v3'})
  print(info)	#{'k1': 'v0', 'k2': 'v2', 'k3': 'v3'}
  
  ```

**七.集合set**

- 无序,不可重复

- 在集合中True与数字1重复,False与数字0重复

- .add() 添加

  ```
  info = {'k1','k2'}
  info.add('k3')
  print(info)	
  
  ```

- .discard()删除

  ```
  info = {'k1','k2','k3'}
  info.discard('k3')
  print(info)	
  
  ```

- 要修改,需要先删除再添加

- .clear()清空

- .update()

  ```python
  info = {'k1','k2'}
  info.update({'k1','k3','v3'})
  print(info)	#{'k1','k2','k3','v3'}
  
  ```

- .intersection() 交集

  ​	***命令后的  () 可以是集合,也可以是列表.***

  ```python
  info = {1,2,3}
  print(info.intersection({1,3,4}))	#{1,3}
  
  ```

- .union()并集

  ```python
  info = {1,2,3}
  print(info.union({1,3,4}))	#{1,2,3,4}
  
  ```

- .difference()差集

  ```python
  info = {1,2,3}
  print(info.union({1,3,4}))	#{2,4}
  
  ```

## 第四章 文件操作

### 4.1 文件基本操作

```python
obj = open('路径',mode='模式',encoding='编码')
obj.write()
obj.read()
obj.close()
```

### 4.2  打开模式

```python
基本模式
#打开文件
f=open('要打开文件路径',mode='r/w/a/',encoding='文件原来编码') #f为接收变量
#操作文件
data = f.()  # 读取文件内部全部内容,data为接收内容
f.write('要写内容')
#关闭文件
f.close()
```

```python
#文件打开新操作,自动关闭
with open('text.txt',mode= 'a',encoding='utf-8') as v:
    data = a.read()
# 缩进中的代码执行完毕之后自动关闭
```

- r / w / a

- r+ / w+ / a+

- rb / wb / ab

- r+b / w+b / a+b 

  **w/wb**

  - w 模式传入的是字符串,写入时计算机进行了两步操作:

    1. 将写入内容根据指定编码encoding转换为对应二进制语言

       ```python
       # 字符串转化为二进制
       mes = '示例'
       a = mes.encode('utf-8')
       #二进制文件转化为字符串
       mes = '01010101010100'
       a = mes.decode('utf-8')
       ```

    2. 将二进制写入到文件里

       - 一般用于文字写入

  - wd 模式传入的是二进制文件,想写入字符串需要进行转换操作

    1. 要把写入内容先转化为二进制语言(encode)
    2. wd再将二进制文件写入文件
       - 一般用于图片/音频/视频/未知编码写入

   **r/rb**

  - r  读取时计算机进行了两步操作:
    1. 读取硬盘上的二进制
    2. 按照指定编码转换成对应文件
  - rb  读取到的是二进制数据,不进行任何转换.

  **a/ab**

  - 用法类比w/r

### 4.3 操作

- read() , 全部读到内存

- read(1) 

  - 1表示一个字符

    ```python
    obj = open('a.txt',mode='r',encoding='utf-8')
    data = obj.read(1) # 1个字符
    obj.close()
    print(data)
    ```

  - 1表示一个字节

    ```python
    obj = open('a.txt',mode='rb')
    data = obj.read(3) # 1个字节
    obj.close()
    ```

- write(字符串)

  ```python
  obj = open('a.txt',mode='w',encoding='utf-8')
  obj.write('中午你')
  obj.close()
  ```

- write(二进制)

  ```python
  obj = open('a.txt',mode='wb')
  
  # obj.write('中午你'.encode('utf-8'))
  v = '中午你'.encode('utf-8')
  obj.write(v)
  obj.close()
  ```

- seek(光标字节位置)，无论模式是否带b，都是按照字节进行处理。

  ```
  obj = open('a.txt',mode='r',encoding='utf-8')
  obj.seek(3) # 跳转到指定字节位置
  data = obj.read()
  obj.close()
  
  print(data)
  
  obj = open('a.txt',mode='rb')
  obj.seek(3) # 跳转到指定字节位置
  data = obj.read()
  obj.close()
  
  print(data)
  ```

- tell(), 获取光标当前所在的字节位置

  ```
  obj = open('a.txt',mode='rb')
  # obj.seek(3) # 跳转到指定字节位置
  obj.read()
  data = obj.tell()
  print(data)
  obj.close()
  ```

- flush，强制将内存中的数据写入到硬盘

  ```
  v = open('a.txt',mode='a',encoding='utf-8')
  while True:
      val = input('请输入：')
      v.write(val)
      v.flush()
  
  v.close()
  ```

### 4.4 关闭文件

文艺

```python
v = open('a.txt',mode='a',encoding='utf-8')

v.close()
```

二逼

```python
with open('a.txt',mode='a',encoding='utf-8') as v:
    data = v.read()
	# 缩进中的代码执行完毕后，自动关闭文件
```



### 4.5  文件内容的修改

```python
with open('a.txt',mode='r',encoding='utf-8') as f1:
    data = f1.read()
new_data = data.replace('飞洒','666')

with open('a.txt',mode='w',encoding='utf-8') as f1:
    data = f1.write(new_data)
```

- 大文件修改
  - 在大文件读取时,不能一次性读取,需要按字节read(字节数)或for循环按行读取

```python
with open('a.txt',mode='r',encoding='utf-8') as f1,open('b.txt',mode='w',encoding='utf-8') as f2:	#打开文件a,b
    for line in f1:																				#按行取a的内容
        new_line = line.replace('666','999')													#修改内容
        f2.write(new_line)																		#修改后的写入b,完成修改
```

## 第五章 函数

### 5.1函数基础

- 三元(目)运算:

  ```python
  v =  前面  if 条件 else 后面    #条件为真v=前面,条件为假v=后面
  #等同于
  if 条件：
  	v = '前面'
  else:
      v = '后面'   
  #示例:
  # 让用户输入值，如果值是整数，则转换成整数，否则赋值为None
  data = input('>>>')
  value =  int(data) if data.isdecimal() else None 
  ```

- 函数定义:函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。函数能提高应用的模块性，和代码的重复利用率。

####  5.1.1.基本结构

```python
def 函数名():
    # 函数内容
    pass				#pass占位符,没有任何操作

# 函数的执行
函数名()
```

- 函数如果不被调用,内部代码将不被执行.

- 运行示例

  ```python
  def get_list_first_data():
      v = [11,22,33,44]
      print(v[0])
  get_list_first_data()
  ```

#### 5.1.2参数

- 参数分为动态参数和静态参数,又叫动参和形参.

  ```python
  def get_list_first_data(aaa): # aaa叫形式参数(形参)
      v = [11,22,33,44]
      print(v[aaa])
  get_list_first_data(1) # 2/2/1调用函数时传递叫：实际参数（实参）
  ```

  - 小练习

    ```python
    # 1. 请写一个函数，函数计算列表 info = [11,22,33,44,55] 中所有元素的和。
    
    def get_sum():
        info = [11,22,33,44,55]
        data = 0
        for item in info:
            data += item
        print(data)
    
    get_sum()
    
    # 2. 请写一个函数，函数计算列表中所有元素的和。
    
    def get_list_sum(a1):
       	data = 0
        for item in a1:
            data += item
       	print(data)
        
    get_list_sum([11,22,33])
    get_list_sum([99,77,66])
    v1 = [8712,123,123]
    get_list_sum(v1)
    
    # 3. 请写一个函数，函数将两个列表拼接起来。
    def join_list(a1,a2):
        result = []
        result.extend(a1)
        result.extend(a2)
        print(result)
        
    join_list([11,22,33],[55,66,77]
    
    # 4. 计算一个列表的长度
    def my_len(arg):
    	count = 0
    	for item in arg:
              count += 1
    	print(count)
    
    v = [11,22,33]
    my_len(v)
    len(v)
    ```

    

#### 5.1.3 返回值return

- return后续代码不会被执行

- 只能返回一次

- 如果要返回多个数据，可先把多个数据包装成一个整体。整体返回（列表、元组、字典.......）

  ```python
  def caculate(a, b):
      he = a + b
      cha = a - b
      return (he, cha)
  ```

- 小练习

  ```python
  # 1. 让用户输入一段字符串，计算字符串中有多少A字符的个数。有多少个就在文件a.txt中写多少个“李邵奇”。
  
  def get_char_count(data):
      sum_counter = 0
      for i in data:
          if i == 'A':
              sum_counter += 1
              
  	return sum_counter
  
  def write_file(line):
      if len(line) == 0:
          return False  # 函数执行过程中，一旦遇到return，则停止函数的执行。
      with open('a.txt',mode='w',encoding='utf-8') as f:
          f.write(line)
  	return True 
  
  
  content = input('请输入:')
  counter = get_char_count(content)
  write_data = "李邵奇" * counter 
  status = write_file(write_data)
  if status:
      print('写入成功')
  else:
      print('写入失败')
         
  # 2. 写函数，计算一个列表中有多少个数字，打印： 列表中有%s个数字。
  #    提示：type('x') == int 判断是否是数字。
  """
  # 方式一：
  def get_list_counter1(data_list):
      count = 0
      for item in data_list:
          if type(item) == int:
              count += 1
  	msg = "列表中有%s个数字" %(count,)
      print(msg)
      
  get_list_counter1([1,22,3,'alex',8])
  
  # 方式二：
  def get_list_counter2(data_list):
      count = 0
      for item in data_list:
          if type(item) == int:
              count += 1
  	return count
      
  v = get_list_counter1([1,22,3,'alex',8])
  msg = "列表中有%s个数字" %(v,)
  print(msg)
  """
  
  # 2. 写函数，计算一个列表中偶数索引位置的数据构造成另外一个列表，并返回。
  """
  # 方式一：
  def get_data_list1(arg):
      v = arg[::2]
      return v
  
  data = get_data_list1([11,22,33,44,55,66])
  
  # 方式二：
  def get_data_list2(arg):
      v = []
      for i in range(0,len(arg)):
      	if i % 2 == 0:
      		v.append(arg[i])
     	return v
  
  data = get_data_list2([11,22,33,44,55,66])
  
  """
  
  # 3. 读取文件，将文件的内容构造成指定格式的数据，并返回。
  """
  a.log文件
      alex|123|18
      eric|uiuf|19
      ...
  目标结构：
  a.  ["alex|123|18","eric|uiuf|19"] 并返回。
  b. [['alex','123','18'],['eric','uiuf','19']]
  c. [
  	{'name':'alex','pwd':'123','age':'18'},
  	{'name':'eric','pwd':'uiuf','age':'19'},
  ]
  """
  #c问答案:
  def read_log(txt):#定义函数
      l=['name','age','job']
      l1 = []
      with open(txt, mode='r', encoding='utf-8') as f:
          for mes in f:#取f行,'alex|123|18','eric|uiuf|19'
              count=0
              dic={}
              for v in mes.strip().split('|'):#mes.split()切割字符串['alex','123','18']
                  dic[l[count]]=v  #取键赋值
                  count +=1
              l1.append(dic)
      print(l1)
  read_log('a.log')
  ```

### 5.2参数祥解

- 实际参数可以是任何值

- 函数没有返回值,默认返回None

- 函数内部执行时,遇到return就终止运行

- return可以返回任何数据类型的值,多个值时返回元组

  ```python
  # enconding: utf-8
  def test():
      return 1,2,3
  print(test())		#(1, 2, 3)
  ```

#### 5.2.1传参

- **传参**:调用函数并传递参数,实参与形参必须一一对应

- **位置传参**:严格按照位置先后顺序传参

- **关键字传参**:直接赋值传参,无先后顺序

- **混合传参**:位置参数与关键字参数混合使用,**位置参数一定在关键字参数之前**

  ```python
  def func(a1, a2):
  print(a1, a2)
  func(a2=99,a1=2)
  # 关键字传参数和位置传参可以混合使用（位置传入的参数 > 关键字参数在后 = 总参数个数）
  def func1(a1, a2, a3):
  print(a1, a2, a3)
  # func(1, 2, a3=9)
  # func(1, a2=2, a3=9)
  # func(a1=1, a2=2, a3=9)
  # func(a1=1, 2,3) # 错误
  ```

#### 5.2.2 默认参数

- **默认参数:**在函数建立时定义好的参数.
- 默认参数可传可不传.不传参使用默认值,传递参数相当于重新对默认参数赋值.

#### 5.2.3 *参数    万能参数(打散)

- (*args)只接收位置参数

- 格式(*参数)可以接收多个参数,但是**只支持位置传参**

- 实参传递到形参时是以元组形式传递

- 传参时实参前加星(*实参),先把实参打散,再传参到形参

  ```python
  def func(*args):
  print(args)
  func(1)
  func(1,2) # args=(1, 2)
  func((11,22,33,44,55)) # args=((11,22,33,44,55),)
  func(*(11,22,33,44,55)) # args=(11,22,33,44,55)
  ```

#### 5.2.4 **参数   万能参数

- **(kwargs)

- 只接收关键

- 字参数

- 实参传递到形参时是以字典形式传递{'k'=v}

- 传参时实参前加**,直接传递字典

  ```python
  def func(*args,**kwargs):
  print(args,kwargs)
  # func(1,2,3,4,5,k1=2,k5=9,k19=999)
  func(*[1,2,3],k1=2,k5=9,k19=999)
  func(*[1,2,3],**{'k1':1,'k2':3})
  func(111,222,*[1,2,3],k11='alex',**{'k1':1,'k2':3})
  ```

注意:一般*args与**kwargs一起使用,这是超级无敌万能参数

```
经典例题:
#    def func(*args,**kwargs):
#        print(args,kwargs)
#    # a. 执行 func(12,3,*[11,22]) ，输出什么？
#    # b. 执行 func(('alex','武沛齐',),name='eric')
'''a.(12,3,11,22) {}
b.(('alex','武沛齐'),) {'name':'eric'}'''
```



**参数重点总结**

- 调用（执行）函数时，传参：位置参数 > 关键字参数
- 定义函数：
  - def func(a)
  - def func(a,b=None)   # 对于默认值，如果是可变类型，----> 坑。 
  - def func(*args,**kwargs)

### 5.3作用域

- 作用域就是作用范围，按照生效范围可以分为全局作用域和局部作用域。

- python

  - 一个py文件是一个全局作用域
  - 一个函数是一个作用域

- 作用域中查找数据规则:优先查找自己局域内数据,自己没有再去父级作用域查找,以此类推,**可以找到,可以修改,不能为父级作用域的变量重新赋值.**

- *global '全局,全球'强制更改**全局作用作用域** ,先global+变量,再对变量赋值*

- *nonlocal '外地'  强制更改**父级作用域变量 **,先nonlocal+变量,再对变量赋值*

  ```python
  # 示例一
  name = ["老男孩",'alex']
  def func():
      global name
      name = '我'
  func()
  print(name)
  # ############################## nonlocal
  name = "老男孩"
  def func():
      name = 'alex'
      def inner():
          nonlocal name # 找到上一级的name
          name = 999
      inner()
      print(name)
  func()
  print(name)
  ```

  

- **全局作用域:**全局作用域内的数据公用

  - 全局变量全部大写

- **局部作用域:**局部作用域可以使用全局作用域内的数据,但是全局作用域使用不了局部作用域的数据即

  - 局部变量正常变量定义

- 函数的作用域链：小范围作用域可以使用大范围的变量，但是反之不行，他是单向的。

- 函数内只可以调用全局作用域的函数

  ```python
  # x = 10
  # def func():
  #     x = 9
  #     print(x)
  #     def x1():
  #         x = 999
  #         print(x)
  #     print(x)
  #     x1()
  #
  # func()
  
  # x = 10
  # def func():
  #     x = 8
  #     print(x)
  #     def x1():
  #         x = 999
  #         print(x)
  #     x1()
  #     print(x)
  #
  # func()
  
  
  # x = 10
  # def func():
  #     x = 8
  #     print(x)
  #     def x1():
  #         print(x)
  #     x1()
  #     print(x)
  #
  # func()
  
  
  
  # x = 10
  # def func():
  #     x = 8
  #     print(x)
  #     def x1():
  #         print(x)
  #     x = 9
  #     x1()
  #     x = 10
  #     print(x)
  #
  # func()
  
  ```

- **作用域小结**
  - 函数为作用域
  - 自己 > 父级 > 父级 > 全局 【读/修改（可变）】
  - 重新赋值：
    - global 全局	
    - nonlocal   外层

### 5.4高级函数

- 函数可以当做变量来使用:

  ```python
  def func():
  	print(123)
  func_list = [func, func, func]
  # func_list[0]()
  # func_list[1]()
  # func_list[2]()
  for item in func_list:
  	v = item()
  	print(v)
  ```

- 函数可以当做参数进行传递,谁调用的函数返回值就给谁.

  ```python
  def func(arg):
  	print(arg)
  func(1)
  func([1,2,3,4])
  def show():
  	return 999
  func(show)
  ```

- 子作用域只能读取或修改父级的值，不能重新赋值。

  ```python
  #经典例题 ---------------企业面试题--------
  
  def func():
      print('花费查询')
  def bar():
      print('语音沟通')
  def base():
      print('xxx')
  def show():
      print('xxx')
  def test():
      print('xxx')
  info = {
      'f1': func,
      'f2': bar,
      'f3':base,
      'f4':show,
      'f5':test
  }
  choice = input('请选择要选择功能：')
  function_name = info.get(choice)
  if function_name:
      function_name()
  else:
      print('输入错误')
  ```

- 函数当做返回值

- 高级一点的内置函数(了解,懂什么意思)

  - **map(函数,可迭代对象)**,映射函数,循环每个元素,然后让每个元素执行函数,将每个函数执行的结果保存到新的列表,并返回.

    ```python
    v1 = [11,22,33,44]
    result = map(lambda x:x+100,v1)
    print(list(result)) # 特殊
    ```

  - **fifter(函数,可迭代对象)**,筛选过滤,循环每个元素然后执行函数,将每个函数执行的结果筛选保存到新的列表并返回.

    ```python
    result = filter(lambda x: True if type(x) == int else False ,v1)
    print(list(result))
    
    result = filter(lambda x: type(x) == int ,v1)
    print(list(result))
    ```

  - **reduce(函数,可迭代对象)**,循环每个元素,然后执行函数,将每个函数执行的结果删选保存到新的列表并返回.

    ```python
    import functools
    v1 = ['wo','hao','e']
    
    def func(x,y):
        return x+y
    result = functools.reduce(func,v1) 
    print(result)
    
    result = functools.reduce(lambda x,y:x+y,v1)
    print(result)
    ```

### 5.5 lambda表达式

```python
# 三元运算，为了解决简单的if else的情况，如：
if 1 == 1:
    a = 123
else:
    a = 456

a =  123  if 1 == 1 else 456		#经典格式

# lambda表达式，为了解决简单函数的情况，如：
def func(a1,a2):
    return a1 + 100 

func = lambda a1,a2: a1+100			#经典格式
```

- **列表所有方法返回值基本都是None,出了pop会返回要删除的元素**

- 字符串所有方法返回值基本都是新值

  ```python
  # 练习题
  USER_LIST = []
  func1 = lambda x: USER_LIST.append(x)
  
  v1 = func1('alex')
  print(v1)
  print(USER_LIST)
  
  # 练习题
  func1 = lambda x: x.split('l')
  
  v1 = func1('alex')
  print(v1)
  
  ```

### 5.6内置函数

- 自定义函数:自我编写函数.

- 内置函数:python系统内置函数.

- lambda表达式也叫匿名函数.  

- 函数与函数之间的数据互不影响,**每次运行函数都会开一个辟新的内存**.

  ```python
  item = 10
  def func():
      item = 2
      def inner():
          print(item)
      for item in range(10):
          pass 
      inner()
  func()
  ```

  - 函数销毁条件:
    - **函数运行完毕**
    - **函数内部元素没有被其他使用**
  - 可迭代数据类型:可被for循环的类型

  |                    内置函数                     |                   含义                    |
  | :---------------------------------------------: | :---------------------------------------: |
  | len,open,range,id,type print,input,强制转换命令 |                    ...                    |
  |                      abs()                      |                  绝对值                   |
  |                     float()                     |                  浮点型                   |
  |                      max()                      |                  最大值                   |
  |                      min()                      |                  最小值                   |
  |                      sum()                      |                   求和                    |
  |                    divmod()                     |       两数相除的商和余数(页面选择)        |
  |                      pow()                      |         指数运算pow(2,3)= 2**3=8          |
  |                      bin()                      |            十进制转化为二进制             |
  |                      oct()                      |            十进制转化为八进制             |
  |                      int()                      |           其他进制转化为十进制            |
  |                      hex()                      |           十进制转化为十六进制            |
  |                      chr()                      | 十进制数字转换成unicode编码中的对应字符串 |
  |                      ord()                      | 根据字符在unicode编码中找到其对应的十进制 |

  - len,open,range,id,type

  - print,input

  - 强制转换

  - 数学相关

    - abs，绝对值

      ```python
      v = abs(-1)
      print(v)
      ```

    - float，转换成浮点型（小数）

      ```python
      v = 55
      v1 = float(55)
      print(v1)
      ```

    - max，找到最大值

      ```
      v = [1,2,311,21,3,]
      result = max(v)
      print(result)
      ```

    - min，找最小值

      ```
      v = [1,2,311,21,3,]
      result = min(v)
      print(result)
      ```

    - sum，求和

      ```
      v = [1,2,311,21,3,]
      result = sum(v)
      print(result)
      ```

    - divmod，两数相除的商和余数

      ```python
      a,b = divmod(1001,5)
      print(a,b)
      ```

      ```python
      # 经典______________练习题  请通过分页对数据进行展示
      """
      要求：
          每页显示10条数据
          让用户输入要查看的页面：页码
      """
      
      USER_LIST = []
      for i in range(1,836):
          temp = {'name':'你少妻-%s' %i,'email':'123%s@qq.com' %i }
          USER_LIST.append(temp)
      
      # 数据总条数
      total_count = len(USER_LIST)
      
      # 每页显示10条
      per_page_count= 10
      
      # 总页码数
      max_page_num,a = divmod(total_count,per_page_count)
      if a>0:
          max_page_num += 1
      
      while True:
          pager = int(input('要查看第几页：'))
          if pager < 1 or pager > max_page_num:
              print('页码不合法，必须是 1 ~ %s' %max_page_num )
          else:
              """
              # 第1页：USER_LIST[0:10] -> 0123456789
              # 第2页：USER_LIST[10:20]
              # 第3页：USER_LIST[20:30]
              ...
              """
              start = (pager-1) * per_page_count
              end = pager * per_page_count
              data = USER_LIST[start:end]
              for item in data:
                  print(item)
      ```

  - 进制转换:

  - 十进制数转化为其他进制时十进制数必须是整型.

  - 其他进制转化为十进制时其他进制数必须是字符串,并在字符串后注明多少进制.

    - bin，将十进制转化成二进制

      ```python
      num = 13
      v1 = bin(num)
      print(v1)
      ```

    - oct，将十进制转换成八进制,

      ```python
      num = 8
      v1 = oct(num)
      print(v1)
      ```

    - int，将其他进制转化成十进制

      ```python
      # 二进制转化成十进制
      v1 = '0b1101'
      result = int(v1,base=2)
      print(result)
      
      # 八进制转化成十进制
      v1 = '0o1101'
      result = int(v1,base=8)
      print(result)
      
      # 十六进制转化成十进制
      v1 = '0x1101'
      result = int(v1,base=16)
      print(result)
      ```

    - hex，将十进制转换成十六进制

      ```python
      num = 16
      v1 = hex(num)
      print(v1)
      ```

  - **----企业面试题------**

    ```python
    # 1字节等于8位
    # IP: 192.168.12.79  ->  001010010 . 001010010 . 001010010 . 001010010
    
    # 1. 请将 ip = "192.168.12.79" 中的每个十进制数转换成二进制并通过,连接起来生成一个新的字符串。
    ip = "192.168.12.79"
    ip_list = ip.split('.') # ['192','168','12','79']
    result = []
    for item in ip_list:
        result.append(bin(int(item)))
    print(','.join(result))
    
    
    # 2. 请将 ip = "192.168.12.79" 中的每个十进制数转换成二进制: 
    #          0010100100001010010001010010001010010 -> 十进制的值。
    
    # 3232238671
    ```

### 5.7闭包

- 在python中闭包,会进行内存驻留,普通函数执行完毕后销毁
- 全局里存放会有污染和不安全的现象
- 装饰器的本质就是闭包

- 应用场景:

  - 装饰器/SQLAIchemy源码

- 函数可以作为变量

- 函数可以作为参数

- **函数可以作为返回值**

  ```python
  def bar():
      def inner():
          print(123)
      return inner
  v = bar()
  v()
  ```

- **闭包**:为一个函数创建一块区域(内部变量供自己使用),为他以后执行提供数据

  ```python
  # 基本格式
  def func(name):
      def inner():
          print(name)
  	return inner 
  
  v1 = func('alex')
  v1()
  v2 = func('eric')
  v2()
  ```

  ```python
  #练习
  # 第一题
  name = 'alex'
  def base():
      print(name)
  
  def func():
   	name = 'eric'
      base()
  
  func() # {name=eric, }
      
  
  # 第二题
  name = 'alex'
  
  def func():
   	name = 'eric'
      def base():
      	print(name)
      base()
  func()
  
  # 第三题
  name = 'alex'
  
  def func():
   	name = 'eric'
      def base():
      	print(name)
      return base 
  base = func()
  base()
  
  ```

  ########################**闭包企业面试题**#######################

  ```python
  info = []
  def func(i):
      def inner():
          print(i)
      return inner
  for item in range(10):
      info.append(func(item))
  info[0]()
  info[1]()
  info[4]()
  ```

### 5.8 内置模块初始(.py文件)

**1.将指定的字符串加密:**

- 将指定的字符串加密md5加密

```python
import hashlib
def get_md5(data):
    obj = hashlib.md5()
    obj.update(data.encode('utf-8'))
    result = obj.hexdigest()
    return result
val = get_md5('123')
print(val)
```

- 加盐

  ```python
  import hashlib
  def get_md5(data):
      obj = hashlib.md5("sxff123ad".encode('utf-8'))#md5(加入任意字符串增加密码复杂度)
      obj.update(data.encode('utf-8'))
      result = obj.hexdigest()
      return result
  val = get_md5('123')
  print(val)
  ```

应用:

```python
#账户密码加密函数:
import hashlib
USER_LIST = []
def get_md5(data):
    obj = hashlib.md5("gfdsjdxff123ad".encode('utf-8'))
    obj.update(data.encode('utf-8'))
    result = obj.hexdigest()
    return result
#用户注册:
def register():
    print('**************用户注册**************')
    while True:
        user = input('请输入用户名:')
        if user == 'N':
            return
        pwd = input('请输入密码:')
        temp = {'username':user,'password':get_md5(pwd)}
        USER_LIST.append(temp)
#用户登录
def login():
    print('**************用户登陆**************')
    user = input('请输入用户名:')
    pwd = input('请输入密码:')
    for item in USER_LIST:
        #校验新输入密码加密后与用户注册加密是否相同
        if item['username'] == user and item['password'] == get_md5(pwd):
            return True
register()
result = login()
if result:
    print('登陆成功')
else:
    print('登陆失败')
```



**2.随机验证码模块应用:**

- chr()  十进制数字转换成unicode编码中的对应字符串 
- ord()  根据字符在unicode编码中找到其对应的十进制
- random.randit(最小值,最大值) 生产随机数

```python
#生产随机验证码
import random
def get_random_code(length=6):
    data = []
    for i in range(length):
        v = random.randint(65,90)	#random.randit(最小值,最大值) 生产随机数
        data.append(chr(v))
    return  ''.join(data)
code = get_random_code()
print(code)
```

```python
import random # 导入一个模块 
v = random.randint(起始,终止) # 得到一个随机数
```

**3.密码不显示（只能在终端运行）**

```python
import getpass
pwd = getpass.getpass('请输入密码：')
if pwd == '123':
    print('输入正确')
```

## 第六章 装饰器/生成器/迭代器

### 6.0推导式

#### 6.0.1列表推导式

- 方便生成一个列表

- 基本格式

  ```python
  # 变量 = [元素 for 元素 in 可迭代对象]
  # 变量 = [元素 for 元素 in 可迭代对象 if 条件]  **条件为True时元素才能添加到列表
  lis = [i for i in 'bigox']
  print(lis)
  
  lis = [i+100 for i in rang(10)]
  print(lis)
  
  lis = [99 if i>5 else 66 for i in range(10)]
  print(lis)
  ```

```python
# 新浪微博面试题
v8 = [lambda x:x*i for i in range(10)] 
# 1.请问 v8 是什么？
# 2.请问 v8[0](2) 的结果是什么？

# 面试题
def num():
    return [lambda x:i*x for i in range(4)]
# num() -> [函数,函数,函数,函数]
print([ m(2) for m in num() ]) # [6,6,6,6]
```

- 推导式中,if判断条件在for循环之前的作用就是条件判断,放在for循环之后就是筛选;

  ```python
  #判断
  lis = [99 if i>5 else 66 for i in range(10)]
  #筛选
  lis = [99 for i in range(10) if >5]
  ```

- 列表推导式比较特殊,会生成一个自己的作用域,i在列表推导式的作用域中,在外面读不到.

#### 6.0.2集合推导式

- 参照列表推导式,注意集合不能重复

  ```python
  # 变量 = {元素 for 元素 in 可迭代对象}
  # 变量 = {元素 for 元素 in 可迭代对象 if 条件}  **条件为True时元素才能添加到列表
  ```

#### 6.0.3字典推导式

- 基本格式

  ```python
  dic = {'k'+str(i) for i in range(10)}
  ```

#### 6.0.4生成器推导式

```python
def func():
   for i in range(10):
       yield i
v2 = func()
v2 = (i for i in range(10)) # 生成器推导式，创建了一个生成器，内部循环为执行。
```

### 6.1装饰器

#### 6.1.1基本结构

```python
def 外层函数(参数):
    def 内层函数(*args,**kwargs);
    	return 参数(*args,**kwargs)
   	return 内层函数
@外层函数
def index()
	pass	
```

```python
#示例:
def func(arg):
    def inner():
        v = arg()
        return v 
    return inner 
@func
def index():
    print(123)
    return 666

print(index)
```

- @func  :执行func函数把下面色函数当做参数传递,相当于：func(index)
- 将外层函数的返回值(内层函数)重新赋值给下面的函数名index,index = func(index)

#### 6.1.2装饰器基本应用

应用场景:想要为函数扩展功能时,使用装饰器

- 计算运行时间

  - 时间模块:time.time()获取当前时间
  - time.sleep('秒数')睡眠

  ```python
  # 计算函数执行时间
  
  def wrapper(func):
      def inner():
          start_time = time.time()
          v = func()
          end_time = time.time()
          print(end_time-start_time)
          return v
      return inner
  @wrapper
  def func1():
      time.sleep(2)
      print(123)
  @wrapper
  def func2():
      time.sleep(1)
      print(123)
  func1()
  ```

#### 6.1.3 带参数的装饰器

```python
#普通装饰器基本格式
def wrapper(func):
    def inner():
        pass
        return func()
    return inner

def func():
    pass
func = wrapper(func)
func()
#带参数装饰器基本格式
def w(counter):
    def wrapper(func):
        def inner(*args,**keargs):
            lis = []
            for i in range(0,counter):
                a=func(*args,**keargs)
                lis.append(a)
            return lis
        return inner
    return wrapper

def func(*args,**keargs):
    return 8
```

```python
#面试题
# 写一个带参数的装饰器，实现：参数是多少，被装饰的函数就要执行多少次，并返回最后一次执行的结果【面试题】
def xxx(counter):
    print('x函数')
    def wrapper(func):
        print('wrapper函数')
        def inner(*args,**kwargs):
            for i in range(counter):
                data = func(*args,**kwargs) # 执行原函数并获取返回值
            return data
        return inner
    return wrapper

@xxx(5)
def index():
    return 8

v = index()
print(v)
```

### 6.2生成器

- 函数中存在yield,那么该函数为生成器函数,调用生成器函数会返回一个生成器,生成器只有被for循环时,生成器函数内部的代码才会执行,每次驯化都会获取yield返回的值.
- 基本格式:

```python
##生成器函数存在yield
def func(*args)
	*args=1
    yield 1
    yield 2   
func('a')
#函数内部代码不会执行,返回一个生成器对象
v = func('a')
#生成器是可以被for循环,一旦开始循环name函数内部代码就会开始执行.
for item in v:
    print(item)    #循环开始,函数运行,yield 返回值,停止第一次循环,再次循环时从上一次yield的位置开始运行
```

- 生成数字:
  - return 可以做终止命令,不会返回值

```python
def func():
    count = 1
    while True:
        yield count
        count += 1
        
val = func()

for item in val:
    print(item)
```

### 6.3迭代器

#### 6.3.1 迭代器

- 迭代:*迭代*是重复反馈过程的活动，其目的通常是为了逼近所需目标或结果。每一次对过程的重复称为一次“*迭代*”

- 迭代器:帮助对某种对象(str/list/tuple类所创建的对象..)中的y元素一一获取.表象:具有``__next__``方法,每次迭代都返回一个值

  - 列表转换成迭代器:
    - lis = iter([1,2,4,2])
    - lis= [1,2,4,2]._ _ iter __()
  - 迭代器想要获取每个值就要反复调用:val = lis.__ next __()
    - 直到报错Stopinteration取到最后一个元素
  - 判断一个对象是否是迭代器:内部是否有__ next __方法.

  ```python
  v1 = [11,22,33,44]
  
  # 列表转换成迭代器
  v2 = iter(v1)
  result1 = v2.__next__()
  print(result1)
  result2 = v2.__next__()
  print(result2)
  result3 = v2.__next__()
  print(result3)
  result4 = v2.__next__()
  print(result4)
  result5 = v2.__next__()
  print(result5)
  """
  # v1 = "alex"
  # v2 = iter(v1)
  # while True:
  #     try:
  #         val = v2.__next__()
  #         print(val)
  #     except Exception as e:
  #         break
  ```

#### 6.3.2 可迭代对象

- 具有```__inter__```方法的就是可迭代对象,并且返回一个迭代器,才成为可迭代对象

  ```python
  v1= [11,22,33,44]
  result= v1.__iter__()
  ```

- 能被for循环的就是可迭代对象

### 6.4总结

- 迭代器，对可迭代对象中的元素进行逐一获取，迭代器对象的内部都有一个 __next__方法，用于以一个个获取数据。
- 可迭代对象，可以被for循环且此类对象中都有 __iter__方法且要返回一个迭代器（生成器）。
- 生成器，函数内部有yield则就是生成器函数，调用函数则返回一个生成器，循环生成器时，则函数内部代码才会执行。
  - 生成器是特殊的迭代器（**）：

## 第七章 模块

```python
#常用内置模块  面试
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
```



- python2,与py3的区别

  - py2:range()  在内存中立即把所有的值都创建,xrange()  不会再内存中立即创建,而是在循环时边环边创建.
  - py3:range()  不会再内存中立即创建,而是在循环时边环边创建.

- sys.exit()   退出程序

- 对于包的定义py2与py3的区别:

  - 包内py2内文件夹中必须有_ _ init _ _ .py
  - py3中不需要
  - *建议所有的包内全部加上_ _ init _ _ .py文件*

- **函数书写时一定要写注释**

- **复杂代码写注释**

- 构造字典和函数的对应关系,避免重复冗余的if ellse

- ```python
  a=1
  b=2
  a,b=b,a#ab值交换
  ```

- 遇到问题解答时一定要问,或者给出多个解答


### 7.1 模块分类

- 内置模块 : py内部提供的功能,直接使用

  ```python
  import sys
  sys.argv()
  ```

- 第三方模块 : 需要从第三方下载/安装使用

  ```python
  #https://pypi.org/
  'pip.exe路径' pip install 需要安装模块的名称			#执行命令
  ```

- 自定义模块:根据需求自我定义模块

  1. 创建文件	file.py
  2. 导入模块        impoort file
  3. 运行模块

### 7.2 内置模块(类库)

- 常用内置模块:**json/time/**          /os/sys
- 模块(类库)可以是py文件,也可以是文件夹
- 定义模块时可以把一个py文件或一个文件夹(包)当做一个模块,方便调用

#### 7.2.1 os模块

- 计算文件大小
- os.urandom()生成随机数
- os.path.basename() 获取文件名称
- os.path.isdir/isfile()判断文件类型
- os.remove() 删除文件
- os.mkedirs()  创建目录和子目录
- os.rename(a,b)  重命名
- os.path.jion(a,b)  连接路径
- os.path.dirname()  文件上一层目录
- os.path.abspath()  绝对路径
- os.path.exists()  路径是否存在
- os.path.getsize()  判断文件大小
- os.listdir()  查看路径内存在文件
- os.walk()  查看路径内所有层级文件

#### 7.2.2 sys模块

- sys.argv(索引)  取用户输入参数

- sys.modules 存储了当前程序中用到的所有模块，反射本文件中的内容

  ```python
  import sys
  ab='s'
  print(getattr(sys.modules[__name__],'ab'))
  ```

  

- sys.path    默认python取导入模块时,会按照sys.path指定的文件夹去寻找
  
  - sys.path.append('目录'), 添加自定义模块读取目录

#### 7.2.3 json   /  pickle模块

```python
json =鸡哥的儿子
鸡哥的儿子是字符串，头衔是：翻译官，擅长翻译列表和字典形式
有两个特殊功能：序列化【dumps】（给别人用）反序列化【loads】（拿来自己用 ）
```

- json    是一个特殊的字符串(长得像列表/字典/字符串/数字混合)

- **json.dumps()    序列化:将列表/字典/字符串/数字转化为json格式的字符串**

- **json.loads()    反序列化:序列化的逆向操作**

- json格式要求:

  - 只能包含int/ str/ list/ dict/ bool/float  不存在元组/集合
  - 最外层必须是一个列表或字典
  - 在json中如果有字符串,必须是双引号"json中的字符串"
  - 真假小写true/false
  - 不能连续load多次

- 字典或者列表中存在中文,序列化的时候或转为UNcode格式,如果想保存中文就需要进行以下操作

  ```python
  v1=[1,2,3,4,'2','大牛']
  val = json.dumps(v1,ensure_ascii=False)
  print(val)     #[1, 2, 3, 4, "2", "大牛"]
  ```

- json:优点:所有语言通用,缺点:只能序列化部分数据类型;不能连续load多次

- pickle;可以序列化多有数据,但是序列化之后只有python识别,能连续load多次

#### 7.2.4 time/datetime模块

- 共有24个时区,每个时区相差一个小时

##### 7.2.4.1 time模块

- time.time()     时间戳stamp    1970-1-1 00:00到现在的秒数

  - UTC/GMT :世界时间
  - 本地时间

  ```python
  #小拓展:1970年1月1日 算 UNIX 和 C语言 生日。 由于主流计算机和操作系统都用它，其他仪器，手机等也就用它了。
  #UNIX系统认为1970年1月1日0点是时间纪元，所以我们常说的UNIX时间戳是以1970年1月1日0点为计时起点时间的。
  ```

- time.sleep('秒数')    等待秒数

- 注意要点:

  ```python
  15555555555243289...
  55555534232214324...
  #看到这种需要想到是不是戳
  ```

##### 7.2.4.2 datetime模块

- 获取datetime格式时间

  - datetime.now()    获取当前本地时间
  - datetime.utcnow()   获取当前utc时间

- 把datetime格式转换成字符串

  - .strftime()

  ```python
  from datetime import datetime
  t1 = datetime.utcnow()
  print(t1,type(t1))		#2019-04-18 07:41:58.725352 <class 'datetime.datetime'>
  val = t1.strftime('%Y-%m-%d %H-%M-%S')
  print(val)	#2019-04-18 07:48:35
  ```

- 把字符串格式的转化为datetime类型

  - datetime.strptime()

    ```python
    from datetime import datetime
    val = t1.strptime('2019-04-18','%Y%m%d')
    print(val)	#2019-04-18 00:00:00
    ```

- 应用场景: 

  - 时间加减

  ```python
  from datetime import datetime,timezone,timedelta
  t1= datetime.strptime('2017-9-9','%Y-%m-%d')
  t2= t1 +timedelta(days=11)
  print(t2)  #2017-09-20 00:00:00
  ```

- 时间戳与datetime的关联

  - 时间戳转datetime

    - datetime.fromtimestamp(t)

    ```python
    from datetime import datetime,timezone,timedelta
    t1 = time.time()
    val = datetime.fromtimestamp(t1)
    print(val)
    ```

  - datetime转时间戳

    - .timestamp()

    ```python
    from datetime import datetime,timezone,timedelta
    t1 = datetime.now()
    val = t1.timestamp()
    print(val)
    ```

#### 7.2.5 shutil模块

- shutil.rmtree()	删除目录

- shutil.move(a,b)       重命名文件或者目录

- shutil.make_archive("压缩后的文件名称","压缩格式","要压缩的文件绝对路径")   压缩文件

  ```python
  shutil.make_archive('zzh','zip','D:\code\s21day16\lizhong')
  ```

- shuti.unpack_archive("要解压文件名称",路径"要解压到的路径",,格式)路径不存在则创建

  ```python
  shutil.unpack_archive('zzh.zip',extract_dir=r'D:\code\xxxxxx\xxxx',format='zip')
  ```

#### 7.2.6 logging模块(日志)

- *快速编写格式*(扩展性不强)

  ```python
  import logging
  import requests
  
  # 日志配置
  logging.basicConfig(
      filename='log.log',
      format=' %(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
      datefmt='%Y-%m-%d %H:%M:%S %p',
      level=logging.ERROR)
  # 异常处理
  try:
      requests.get('http://www.xxx.com')
  except Exception  as e:
      mes = str(e)
      # 日志生成
      logging.error(mes, ext_info=True)    #ext_info=True保存堆栈信息
  ```

- **推荐编写方式**

  ```python
  import logging
  file_hander = logging.FileHandler(filename='log.log',mode='a',encoding='utf-8',)
  logging.basicConfig(
      format=%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
      datafmt='%Y-%m-%d %H:%M:%S %p',
      handlers=[file_handler,],
      level=logging.ERROR
  )
  
  logging.error('你好')
  ```

- **更改调用机制**:

  ```python
  import logging
  from logging import handlers
  
  def get_logger():
      file_handler = handlers.TimedRotatingFileHandler(filename='x3.log', when='s', interval=5, encoding='utf-8')
      logging.basicConfig(
          format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
          datefmt='%Y-%m-%d %H:%M:%S %p',
          handlers=[file_handler, ],
          level=logging.ERROR
      )
      return logging
  logger = get_logger()
  
  
  logging.error('你好')
  ```

- **推荐日志处理方式+日志切割**

  ```python
  import time
  import logging
  from logging import handlers
  
  file_handler = handlers.TimedRotatingFileHandler(filename='x3.log', when='s', interval=5, encoding='utf-8')
  logging.basicConfig(
      format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
      datefmt='%Y-%m-%d %H:%M:%S %p',
      handlers=[file_handler,],
      level=logging.ERROR
  )
  
  for i in range(1,100000):
      time.sleep(1)
      logging.error(str(i))
  ```

- 注意事项

  ```python
  # exc_info=True  在应用日志时，如果想要保留异常的堆栈信息。
  import logging
  import requests
  
  logging.basicConfig(
      filename='wf.log',
      format='%(asctime)s - %(name)s - %(levelname)s -%(module)s:  %(message)s',
      datefmt='%Y-%m-%d %H:%M:%S %p',
      level=logging.ERROR
  )
  
  try:
      requests.get('http://www.xxx.com')
  except Exception as e:
      msg = str(e) # 调用e.__str__方法
      logging.error(msg,exc_info=True)
  ```

  

### 7.3 模块调用

- 在那个文件里运行代码时,系统会自动把文件所在目录下的文件夹和文件自动加载到内存

- 导入模块(第一种)

  - **import  模块名称**
  - 调用:  **模块名称.函数()**
  - 代码从上到下运行,遇见导入的模块时,立即执行导入的模块

- 导入模块(第二种)

  - **from 模块名称 import 函数名 , 函数名 as 别名**		#导入部分函数,as可以在函数名重复时起别名

  - from 模块名称 import *                 #导入模块全部函数

  - **from 模块.模块.模块 import  函数名称**

  - 调用: 函数名/别名()

    ```python
    # 导入模块
    from lizhongwei import func,show
    from lizhongwei import func
    from lizhongwei import show
    from lizhongwei import *
    func()
    ```



- 通俗的理解`__name__ == '__main__'`：假如你叫小明.py，在朋友眼中，你是小明`(__name__ == '小明')`；在你自己眼中，你是你自己`(__name__ == '__main__')`。
- `if __name__ == '__main__'`的意思是：当.py文件被直接运行时，`if __name__ == '__main__'`之下的代码块将被运行；当.py文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行。

### 

**调用小结:**

- 模块和要执行的py文件在同一目录,需要模块中很多个功能,推荐使用import 模块
- 其他推荐from模式导入

### 7.4 异常处理

1. **基本格式**

   ```python
   try:
       pass
   except Exception as e:
       pass
   ```

- finally 只要try的内容终止,最后无论对错都会执行

  ```python
  try:
      int('asdf')
  except Exception as e:
      print(e) # e是Exception类的对象，中有一个错误信息。
  finally:
      print('最后无论对错都会执行')
  ```

  - *主动触发异常*

  ```python
  try:
      int('123')
      raise Exception('阿萨大大是阿斯蒂') # 代码中主动抛出异常
  except Exception as e:
      print(e)
  ```

  ```python
  def func():
      result = True
      try:
          with open('x.log',mode='r',encoding='utf-8') as f:
              data = f.read()
          if 'alex' not in data:
              raise Exception()
      except Exception as e:
          result = False
      return result
  ```

  - *自定义异常*

  ```python
  class MyException(Exception):
      pass
  
  try:
      raise MyException('asdf')
  except MyException as e:
      print(e)
  ```

  ```python
  class MyException(Exception):
      def __init__(self,message):
          super().__init__()
          self.message = message
  
  try:
      raise MyException('asdf')
  except MyException as e:
      print(e.message)
  ```


### 7.5 hashlib  摘要算法模块

- 密文验证
- 校验文件的一致性
- md5加密
- sha加密

```python
#将指定的字符串加密:**

#- 将指定的字符串加密md5加密

​```python
import hashlib
def get_md5(data):
    obj = hashlib.md5()
    obj.update(data.encode('utf-8'))
    result = obj.hexdigest()
    return result
val = get_md5('123')
print(val)
​```

- 加盐

  ```python
  import hashlib
  def get_md5(data):
      obj = hashlib.md5("sxff123ad".encode('utf-8'))#md5(加入任意字符串增加密码复杂度)
      obj.update(data.encode('utf-8'))
      result = obj.hexdigest()
      return result
  val = get_md5('123')
  print(val)
```
### 7.6random随机模块

应用场景

- 验证码
- 抽奖

```python

print( random.randint(1,10) )        # 产生 1 到 10 的一个整数型随机数  
print( random.random() )             # 产生 0 到 1 之间的随机浮点数
print( random.choice('tomorrow') )   # 从序列中随机选取一个元素
print( random.randrange(1,100,2) )   # 生成从1到100的间隔为2的随机整数
print(random.sample([1,2,3,4],3))   # 随机选择三个(一个奖项抽取多个人)
```

### 【补充】dumps,loads,dump,load的区别【json&pickle】

#### 1 json.dumps()

**json.dumps()是将字典类型转化成字符串类型。**

```python
import json
 
name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'} 
jsObj = json.dumps(name_emb)    
 
print(name_emb)
print(jsObj)
```

#### 2 json.dump()

**json.dump()用于将dict类型的数据转成str，并写入到json文件中**

```python
import json  

name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'}  

emb_filename = ('/home/cqh/faceData/emb_json.json')  

# solution 1
jsObj = json.dumps(name_emb)    
with open(emb_filename, "w") as f:  
    f.write(jsObj)  
    f.close()  

# solution 2   
json.dump(name_emb, open(emb_filename, "w"))
```

#### 3 json.loads()

**json.loads()将字符串类型转化成字典类型**

```python
import json

name_emb = {'a':'1111','b':'2222','c':'3333','d':'4444'} 

jsDumps = json.dumps(name_emb)    

jsLoads = json.loads(jsDumps) 

print(name_emb)
print(jsDumps)
print(jsLoads)
```

#### 4 json.load()

**json.load()用于从json文件中读取数据。**

```python
# json_load.py 
strList = json.load(open("listStr.json")) 
print strList 
print strDict 
# {u'city': u'\u5317\u4eac', u'name': u'\u5927\u5218'} 
```

- **pickle常用操作参考json**

- 格式要求:

  - 只能包含int/ str/ list/ dict/ bool/float  不存在元组/集合
  - 最外层必须是一个列表或字典
  - 在json中如果有字符串,必须是双引号"json中的字符串"
  - 真假小写true/false

- 字典或者列表中存在中文,序列化的时候或转为UNcode格式,如果想保存中文就需要进行以下操作

  ```python
  v1=[1,2,3,4,'2','大牛']
  val = json.dumps(v1,ensure_ascii=False)
  print(val)     #[1, 2, 3, 4, "2", "大牛"]
  ```

- json:优点:所有语言通用,缺点:只能序列化部分数据类型;不能连续load多次

- pickle;可以序列化多有数据,但是序列化之后只有python识别,能连续load多次

总结:  dump与json转化时可以结合文件操作,dumps与jsons不能.

## 第八章 面向对象

```
类: 具有相同属性或者相似方法的一类事物
对象/实例: 具有实际属性的类的具体化
实例化:从类到实例的过程
类-->实例化--->对象
```

### 8.1面向对象基本用法

#### 8.1.1基本格式

```python
class 类名:
    def __init__(self,x):
        self.x = x
    def 方法名字 (self):   #函数在类里称为方法,self就是固定参数,必须写self
        print('方法')
        return 
    def 方法名字 (self):   
        print('方法')
        return 
#实例化一个类的对象
v1 = 类(可以传参)
v2.方法()
```

- 单例模式: 无论实例化多少次,都用第一次实例化的对象.

- 标准格式

  - ```__new__```创建一个实例化对象,并且在init之前执行

  ```python
  class Singleton(object):
      instance= None
      def __new__ (cls,*args,**kwargs):
      	if not cls.instance:
              cls.instance= object.__new__(cls)
          return cls.instance
  ```



#### 8.1.2调用方法

1. 创建类的对象(实例化对象)

   ```obj=类名()``` #创建了一个Account类的对象

2. 使用对象调用类的方法

   ```obj.函数名字()```调用时方法是有返回值的,与函数类似

- 应用场景:

```python
 - 函数（业务功能）比较多，可以使用面向对象来进行归类。
 - 想要做数据封装（创建字典存储数据时，面向对象）。
 - 游戏示例：创建一些角色并且根据角色需要再创建人物。
```

#### 8.1.3对象的作用

- 存储一些值,方便自己使用

  ```python
  class file:
      def read(self):
          with open(self.xxx, mode='mode='r', encoding='utf-8') as f:
              data = f.read()
          return data
  
      def write(self,content):
          with open(self.xxx, mode='mode='a', encoding='utf-8') as f:
              f.write(content)
  
  # 实例化一个file的对象
  v1 = file()
  # 在对象中写了一个xxx='test.log'
  v1.xxx = 'test.log'
  # 通过对象调用类中的read方法，read方法中的self就v1:
  #v1.read()
  obj1.write('alex')
  ```

#### 8.1.4类的初始化方法

- 类() 实例化对象，自动执行此类中的 __ init __方法。
- __ init __ 初始化方法(构造方法),为对象内部做初始化
- 如果有一个反复使用的公共值,课可以放到对象中

```python
class Person:
    def __init__ (self,n,a,g):
        self.name=n
        self.age =a
        self.gender= g
    def show(self):
        temp= '我是%s,今年%s,性别%s'%(self.name,self.age,self.gender)
        print(temp)
# 类() 实例化对象，自动执行此类中的 __init__方法。
p1 = Person('bigox',20,'男')
p1 = show()
```

### 8.2.面向对象的三大特性

#### 8.2.1封装

```python
class File:
    def read(self):
        pass
    def write(self):
        pass
```

```python
class Person:
    def __init__(sef,name,age):
        self.name = name
        self.age = age
p = Person('alex',19)
```

#### 8.2.2继承

```python
#基本格式
class Base:
    pass
class Foo(Base):
    pass
```

- 在多个类中如果有公共的方法,可以使用继承,增加代码的重用性.

- 继承关系中查找方法的顺序

  - self是谁?
  - self是哪个类创建的,就从此类开始找,自己没有就找父类

- 多继承就从左往右

  ```python
  # 父类(基类)
  class Base:
      def f1(self):
          pass
  # 子类（派生类）
  class Foo(Base):
      def f2(self):
          pass
  
  # 创建了一个字类的对象
  obj = Foo()
  # 执行对象.方法时，优先在自己的类中找，如果没有就是父类中找。
  obj.f2()
  obj.f1()
  
  # 创建了一个父类的对象
  obj = Base()
  obj.f1()
  ```

- 继承扩展

  - 单继承 :子类可以使用父类的方法

  - 多继承: 查找顺序

    - 深度优先: 对每一个可能的分支路径深入到不能再深入为止，而且每个结点只能访问一次。

    - 广度优先:从上往下对每一层依次访问，在每一层中，从左往右（也可以从右往左）访问结点，访问完一层就进入下一层，直到没有结点可以访问为止。

  ```python
  
      # 新式类和经典类
          # py2 继承object就是新式类
          #     默认是经典类
          # py3 都是新式类，默认继承object
  
          # 新式类
              # 继承object
              # 支持super
              # 多继承 广度优先C3算法
              # mro方法
          # 经典类
              # py2中不继承object
              # 没有super语法
              # 多继承 深度优先
              # 没有mro方法
  ```

#### 8.2.3多态

- 一个类表现出来的多种状态 --> 多个类表现出相似的状态

```python
#面试题,什么是鸭子模型
对于一个函数,python对于参数的类型不会限制,传入参数时就可以是各种类型,但是在函数中如果有类似于索引等特有方法,就会对传入的参数类型有一个限制(类似于字符串的.append.send方法)
类似于上述的函数我们认为只要能呱呱叫的就是鸭子（只要有.send方法，就是我们要想的类型）
# Python
def func(arg):
    v = arg[-1] # arg.append(9)
    print(v)

# java
def func(str arg):
    v = arg[-1]
    print(v)
```

### 8.3面向对象成员

=====================成员====================

- 类成员
  - 类变量
  - 绑定方法
  - 类方法
  - 静态方法
  - 属性
- 实例成员(对象)
  - 实例变量

#### 8.3.1实例变量

- 类实例化后的对象内部的变量

#### 8.3.2类变量

- 类中的变量,写在类的下一级和方法同一级。

- 访问方法:

  - **类.类变量名称(推荐)**
  - 对象.类变量名称

- 面试题:

  - 总结：找变量优先找自己，自己没有找 类 或 基类；修改或赋值只能在自己的内部设置。

  ```python
  class Base:
      x = 1
     
  obj = Base()
  
  print(obj.x) # 先去对象中找，没有再去类中找。
  obj.y = 123  # 在对象中添加了一个y=123的变量。
  print(obj.y)
  obj.x = 123
  print(obj.x)
  print(Base.x)
  ```

#### 8.3.3绑定方法/普通方法

- 定义：至少有一个self参数

- 执行：先创建对象，由对象.方法()。

  ```python
  class Foo:
      def func(self,a,b):
          print(a,b)
          
  obj = Foo()
  obj.func(1,2)
  # ###########################
  class Foo:
      def __init__(self):
          self.name = 123
  
      def func(self, a, b):
          print(self.name, a, b)
  
  obj = Foo()
  obj.func(1, 2)
  ```

#### 8.3.4静态方法

- 定义:
  - @staticmethod装饰器
  - 参数无限制
- 调用:
  - **类.静态方法名()**
  - 对象.静态方法名()不推荐
- 在方法不进行传参时使用

```python
class Foo:
    def __init__(self):
        self.name = 123

    def func(self, a, b):
        print(self.name, a, b)

    @staticmethod
    def f1():
        print(123)

obj = Foo()
obj.func(1, 2)

Foo.f1()
obj.f1() # 不推荐
```

#### 8.3.5类方法

- 定义:

  - @classmethod装饰器
  - 至少有cls一个参数,当前类

- 执行

  - **类**.**方法**()
  - 对象.类方法()不推荐

  ```python
  class Foo:
      def __init__(self):
          self.name = 123
  
      def func(self, a, b):
          print(self.name, a, b)
  
      @staticmethod
      def f1():
          print(123)
  
      @classmethod
      def f2(cls,a,b):
          print('cls是当前类',cls)
          print(a,b)
  
  obj = Foo()
  obj.func(1, 2)
  
  Foo.f1()
  Foo.f2(1,2)
  ```

- 面试题

  ```python
  # 问题： @classmethod和@staticmethod的区别？
  """
  一个是类方法一个静态方法。 
  定义：
  	类方法：用@classmethod做装饰器且至少有一个cls参数。
  	静态方法：用staticmethod做装饰器且参数无限制。
  调用：
  	类.方法直接调用。
  	对象.方法也可以调用。 
  """
  ```

#### 8.3.6属性

- 定义:
  - @property装饰器
  - 只有一个self参数
- 调用:
  - 对象.方法   **不加括号**

```python
class Foo:

    @property
    def func(self):
        print(123)
        return 666

obj = Foo()
result = obj.func
print(result)
```

- 属性的应用

  ```python
  # 属性的应用
  
  class Page:
      def __init__(self, total_count, current_page, per_page_count=10):
          self.total_count = total_count
          self.per_page_count = per_page_count
          self.current_page = current_page
      @property
      def start_index(self):
          return (self.current_page - 1) * self.per_page_count
      @property
      def end_index(self):
          return self.current_page * self.per_page_count
  
  
  USER_LIST = []
  for i in range(321):
      USER_LIST.append('alex-%s' % (i,))
  
  # 请实现分页展示：
  current_page = int(input('请输入要查看的页码：'))
  p = Page(321, current_page)
  data_list = USER_LIST[p.start_index:p.end_index]
  for item in data_list:
      print(item)
  ```

### 8.4成员修饰符

#### 8.4.1 公有

- 公有，所有地方都能访问到。

  ```python
  class Foo:
      def __init__(self, name):
          self.__name = name
  
      def func(self):
          print(self.__name)
  
  
  obj = Foo('alex')
  # print(obj.__name)
  obj.func()
  ```

#### 8.4.2 私有

- 私有，只有自己可以访问到。

  ```python
  class Foo:
      __x = 1
  
      @staticmethod
      def func():
          print(Foo.__x)
  
  
  # print(Foo.__x)
  Foo.func()
  
  #---------------------------------------------
  class Foo:
  
      def __fun(self):
          print('msg')
  
      def show(self):
          self.__fun()
  
  obj = Foo()
  # obj.__fun()
  obj.show()
  ```

#### 8.4.3强制访问私有成员

```python
# 强制访问私有成员

class Foo:
    def __init__(self,name):
        self.__x = name


obj = Foo('alex')

print(obj._Foo__x) # 强制访问私有实例变量
```

#### 8.4.4内容补充py2/3区别

- py2/3区别:

- ```python
  class Foo:
      pass
  
  class Foo(object):
      pass
  
  # 在python3中这俩的写法是一样，因为所有的类默认都会继承object类，全部都是新式类。
  
  
  # 如果在python2中这样定义，则称其为：经典类
  class Foo:
      pass 
  # 如果在python2中这样定义，则称其为：新式类
  class Foo(object):
      pass 
  
  class Base(object):
      pass
  class Bar(Base):
      pass
  ```

#### 8.4.5嵌套

- 类/方法/对象都可以当做变量或嵌套到其他类型中.
- 函数的参数可以是任意类型.
- 可哈希(不可变)数据类型可以做字典的key.
- 类和对象可以做字典的key.

```python
class School(object):
    def __init__(self,title,addr):
        self.title = title
        self.address = addr
        
class ClassRoom(object):
    
    def __init__(self,name,school_object):
        self.name = name
        self.school = school_object
        
s1 = School('北京','沙河')
s2 = School('上海','浦东')
s3 = School('深圳','南山')

c1 = ClassRoom('全栈21期',s1)
c1.name
c1.school.title
c1.school.address
# ############################################
v = [11,22,33,{'name':'山海','addr':'浦东'}]

v[0]
v[3]['name']
```

### 8.5 特殊方法(8个)

- 补充:

  - 类的类变量可以直接操作输出,方法内不可以

    ```python
    class fun:
        print(123)         
        def func(self):
            print(1234)
        class fuu(self):
            print(456)
    #输出     123  456
    ```

- ```__init__ ```   #初始化方法:  用于给对象赋值

  ```python
  class Foo:
      """
      类是干啥的。。。。
      """
      def __init__(self,a1):
          """
          初始化方法
          :param a1: 
          """
          self.a1 = a1
          
  obj = Foo('alex')
  ```

- ```__new__```   #构造方法:  在init之前用于创建对象

  ```python
  class Foo(object):
      def __init__(self):
          """
          用于给对象中赋值，初始化方法
          """
          self.x = 123
      def __new__(cls, *args, **kwargs):
          """
          用于创建空对象，构造方法
          :param args: 
          :param kwargs: 
          :return: 
          """
          return object.__new__(cls)
  
  obj = Foo()
  ```

- ```__call__ ```对象后面加()执行cal方法;

  ```python
  class Foo(object):
      def __call__(self, *args, **kwargs):
          print('执行call方法')
  
  # obj = Foo()
  # obj()
  Foo()()
  ```

- ```__getitem__ ``` ```__setitem__ ``` ```__delitem__ ```

  ```python
  class Foo(object):
  
      def __setitem__(self, key, value):
          pass
  
      def __getitem__(self, item):
          return item + 'uuu'
  
      def __delitem__(self, key):
          pass
  
  
  obj1 = Foo()
  obj1['k1'] = 123  # 内部会自动调用 __setitem__方法
  val = obj1['xxx']  # 内部会自动调用 __getitem__方法
  print(val)
  del obj1['ttt']  # 内部会自动调用 __delitem__ 方法
  ```

- ```__str__```打印一个对像时,str返回什么打印什么

  ```python
  class Foo(object):
      def __str__(self):
          """
          只有在打印对象时，会自动化调用此方法，并将其返回值在页面显示出来
          :return: 
          """
          return 'asdfasudfasdfsad'
  
  obj = Foo()
  print(obj)
  ```

- ```__dict__```

  ```python
  class Foo(object):
      def __init__(self,name,age,email):
          self.name = name
          self.age = age
          self.email = email
  
  obj = Foo('alex',19,'xxxx@qq.com')
  val = obj.__dict__ # 去对象中找到所有变量并将其转换为字典
  print(val)
  ```

- **上下文管理**<<面试题>>

  ```python
  class Foo(object):
      def do_something(self):
          print('内部执行')
  
  class Context:
      def __enter__(self):
          print('进入')
          return Foo()
  
      def __exit__(self, exc_type, exc_val, exc_tb):
          print('推出')
  
  with Context() as ctx:
      print('内部执行')
      ctx.do_something()
  ```

- ```__add__```两个对象相加(面试题)

  ```python
  val = 5 + 8
  print(val)
  
  val = "alex" + "sb"
  print(val)
  
  class Foo(object):
      def __add__(self, other):
          return 123
      
  obj1 = Foo()
  obj2 = Foo()
  val  = obj1 + obj2
  print(val)
  ```

  - 特殊成员：就是为了能够快速实现执行某些方法而生。

### 8.6 内置函数补充

importlib 用字符串的形式导入模块

```python
模块 = importlib.import_module('utils.redis')
```

- 示例:

```python
import importlib

#用字符串的模式导入模块
redis = importlib.import_module("utils.redis")
#用字符串的形式去对象(模块)找到他的成员
getattr(redis,"func")()
```



### 8.7 栈与队列初识

- 栈:类似弹夹,先进后出
- 队列:类似水管,先进先出

```python
class Stack(object):
    """
   	先进后出
    """
    def __init__(self):
        self.data_list=[]
        
    def push(self,val):
        """
        向栈中压入一个数据(入栈)
        """
        self.data_list.append(val)
        
    def pop(self):
        """
       	从栈中拿走一个数据(出栈)
        """
       	return self.data_list.pop()
```

### 8.8 可迭代对象

- 表象:可以被for循环的对象就是可迭代对象

- 在类中实现```__iter__```方法且返回一个迭代器(生成器)

  ```python
  class Foo:
      def __iter__(self):
          return iter([1,2,3,4])
  obj = Foo()
  
  class Foo:
      def __iter__(self):
          yield 1
          yield 2
          yield 3
  
  obj = Foo()                                                               
  ```

### 8.9 约束

- 约束子类内必须使用方法,不然主动异常

  ```python
  class BaseMessage(object):
      def send(self,a1):
          raise NotImplementedError('字类中必须有send方法')
          
  class Msg(BaseMessage):
      def send(self):
          pass
  
  class Email(BaseMessage):
      def send(self):
          pass
  
  class Wechat(BaseMessage):
      def send(self):
          pass
  
  class DingDing(BaseMessage):
      def send(self):
          print('钉钉')
      
  obj = Email()
  obj.send()
  ```

### 8.10反射

- python一切皆对象,所以想要通过字符串的形式操作内部成员都可以通过反射去完成操作.

- py文件 包 类 对象...

- 反射:根据字符串的形式去某个对象操作对象的成员.

  - getattr(对象名,"方法名")    

    - **根据字符串的形式去某个对象中获取对象的成员**.
    - attribute属性

    ```python
    class Foo(object):
        def __init__(self,name):
          self.name = name
        def login(self):
          pass
    obj = Foo('alex')
    
    # 获取变量
    v1 = getattr(obj,'name')
    # 获取方法
    method_name = getattr(obj,'login')
    method_name()
    ```

  - setattr(对象名称,"变量",值 )   

    - **根据字符串的形式去某个对象中设置成员**.

    ```python
    class Foo:
      pass
    obj = Foo()
      obj.k1 = 999
    setattr(obj,'k1',123) # obj.k1 = 123
    
      print(obj.k1)
    ```

  - hasattr(对象名称,"方法名")

    - **根据字符串的形式去某个对象中判断是否含有某成员**.返回布尔类型

    ```python
      class Foo:
          pass
      
      obj = Foo()
      obj.k1 = 999
      hasattr(obj,'k1')
      print(obj.k1)
    ```

  - delattr(对象,"方法名")

    - **根据字符串的形式去某个对象中删除某成员**.

    ```python
    class Foo:
        pass
    
    obj = Foo()
    obj.k1 = 999
    delattr(obj,'k1')
    print(obj.k1)
    ```

补充:

- 模块importlib
  - importlib 用字符串的形式导入模块

  ```python
  模块 = importlib.import_module('utils.redis')
  ```

  - 示例:

  ```python
  import importlib
  
  #用字符串的模式导入模块
  redis = importlib.import_module("utils.redis")
  #用字符串的形式去对象(模块)找到他的成员
  getattr(redis,"func")()
  ```

```python
self.MIDDLEWARE_CLASSES = [
            'utils.session.SessionMiddleware',
            'utils.auth.AuthMiddleware',
            'utils.csrf.CrsfMiddleware',
        ]
for mes in self.MIDDLEWARE_CLASSES:
    module_path,class_name=mes.rsplit('.',maxsplit=1)       #切割路径和类名
    module_object = importlib.import_module(module_path)    #插入模块-字符串操作
    cla=getattr(module_object,class_name)        #根据模块对象找到类名(字符串操作-反射)
    obj = cla()      #实例化对象
	obj.process()      #运行内部函数process
```

## 第九章 正则表达式与re模块

```python
#使用分组注意事项(面试)
1.和findall连用,会优先展示分组内容,取消优先分组展示(?:xxx)
2.和split连用,会保留分组内容
3.分组命名(?P<name>正则表达式)
```

### 9.1 正则表达式regex

- 正则表达式regex是指一规则,匹配字符串的规则,应用:
  - 匹配字符串
  - 表单验证
  - **爬虫**:从网页源码获取一些链接,重要数据
- 原字符
- 量词

#### 9.1.1规则

​	1.1 基本匹配:	本身是哪一个字符,就匹配字符换中的哪一个字符

​	1.2 字符组匹配[字符1字符2]规则:	**一个字符**组就匹配**一个字符**,只要这个字符出现在字符组内就会被匹配到

- 字符组可以使用范围,所有的范围必须遵循ascii码从小到大来指定
- 常用字符组范围**[0-9]/[a-z]/[A-Z]/[a-zA-Z0-9]**

​    1.3 [0-9]==\d  所有的数字

​	1.4 \d 与 [0-9] 与 [\d]无区别

#### 9.1.2元字符

- []字符组  只要在中括号之内的所有字符都符合匹配规则
- [^]非字符组   只要在中括号之内的所有字符都**不**符合匹配规则
- **\d 数字(digit)**
- **\w 标识符(word)表示大小写字母,数字,下划线**
- **\s 空格(space),表示空格,换行符,table制表符**
- \t  (table) 仅仅制表符
- \n (next) 仅仅换行符
- **\D   匹配非数字**
- **\w   匹配非大小写字母,数字,下划线**
- **\S   匹配非空格,换行符,table**
- .  表示除了换行符的任意内容
- \取消转义, \ .表示只匹配 .
- [\d\D] 匹配任意字符
- ^表示一个字符的开始:^s表示只匹配开头的s
- $表示一个字符的结束:$e表示只匹配结束的e
- ^abc$ 同时出现字符串只能是abc
- | 表示或,注意如果两个规则有重叠部分,总是长的在前面,短的在后面
- (|)括号限制|的作用域, 例 :www.(baidu|google).com,只会作用域括号内

```
#帮助记忆#
\d \w \s \t(table) \n(next)
\D \W \S
.
[]   [^]
^    $
|   ()
```

#### 9.1.3量词

- \d{n} 数字n表示该原字符执行次数,且只能匹配这么多次

- \d{n,}数字n表示该原字符至少出现n次

- \d{n,m}数字n表示该原字符至少出现n次,至多出现m次

- \d?    ?表示匹配0次或者1次   ,比如小数点

- \d+   +表示匹配1次或多次

- \d *   *表示匹配0次或多次  ,比如匹配整数或者小数

- 匹配小数

  ```python
  \d+(\.\d+)?
  #例:
  12.3432
  ```

3.1 默认贪婪匹配 ,总是在符合匹配规则的范围内尽可能多的匹配

3.2 非贪婪匹配,(惰性匹配):总是在符合匹配规则的范围内尽可能少的匹配

- **元字符 量词 ? x**

  表示按照原字符规则在量词范围内匹配,一旦遇到x就停止

  - .*?x 匹配任意字符,碰见x立即停止.

- ? 出现在量词之后表示非贪婪匹配

  

#####身份证小练习

```python
# 身份证号
# 15位  全数字 首位不为0
# 18位  前17位全数字 首位不为0  最后一位可能是x和数字
[1-9](\d{14}|\d{16}(\d|x))
[1-9](\d{16}[\dx]|\d{14})
[1-9]\d{14}(\d{2}[\dx])?
```





### 9.2 re模块

- re模块本身是用来操作正则表达式,与正则本身没有关系

- **正则表达式是用来匹配处理字符串的 python 中使用正则表达式需要引入re模块**

  如：

  **import re #第一步，要引入re模块**

- **re.match**(pattern表达式规则, string)

  - 从头开始,等同于re.search加上^号
  - 如果不是起始位置匹配成功的话，match()就返回none.
  - 匹配成功re.match方法返回一个匹配的对象，否则返回None。

  ```python
  import re
  print(re.match('www', 'www.runoob.com'))  # 在起始位置匹配
  print(re.match('com', 'www.runoob.com'))         # 不在起始位置不匹配
  ```

- **re.search("匹配规则", "要匹配的字符串")**

  - 匹配成功re.search方法返回一个匹配的对象，否则返回None。
  - 从头到尾从头匹配字符串中取出**第一个符合条件的项**.
  - re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

  ```python
  import re
  ret = re.serch('\d','alex83')
  print(ret)		
  if ret:
      print(ret.group())		#如果返回对象,用group取值
  ```

- **re.findall("匹配规则", "要匹配的字符串")** 

  - 在字符串中找到正则表达式所匹配的所有子串，并返回一个**列表**
  - 如果没有找到匹配的，则返回**空列表**。

- **re.finditer**

  - 读大文件使用finditer节省内存,结果比较多的时候使用
  - 返回值与findall类似         

```python
import re
ret = re.finditer('\d','asfjsdf12jidfiewjfi'*200000)      #ret是一个迭代器
for i in ret:					#迭代出来的每一项都是一个对象
    print(i.group())		 #通过group取值即可
```

- **re.compile**  编译方法

  - 在同一个正则表达式重复使用多次的时候使用compile能减少使用时间开销

  ```python
  import re
  s= 'asfjsdf12jidfiewjfi'
  ret = re.compile('\d+')
  r1 = ret.seach('alex83')
  r2 = ret.findall('wusir73')
  ```

- 扩展

  ```python
  import re
  # ret = re.search("<(?P<tag_name>\w+)>\w+</w+>","<h1>hello</h1>")
  # print(ret.group('tag_name'))
  # print(ret.group())
  ret = re.search("<(?P<tag_name1>\w+)>\w+</\w+>","<h1>hello</h1>")
  #还可以在分组中利用?的形式给分组起名字
  #获取的匹配结果可以直接用group('名字') 拿到对应的值
  print(ret.group('tag_name1')) #结果 ：h1
  print(ret.group()) #结果 ：<h1>hello</h1>
  ```

#### 9.2.1 re模块分组

-  findall 遇到分组时,优先显示分组中的内容

- ?:    取消分组优先展示

  ```python
  import re
  s1 = '1-2*(60+(-40.35/5)-(-4*3))'
  res=re.compile(r'\d+\.\d+?|(?:\d+)')
  ret=re.findall(res,s1)
  # ret.remove('')
  print(ret)
  ```

- 分组编号

  ```python
  s1 = '<h1>wahaha</h1>'
  s2 = '<a>wahaha ya wahaha</a>'
  import re
  
  ret = re.search('<(\w+)>(.*?)</\w+>',s1)
  print(ret)
  print(ret.group(0))   # group参数默认为0表示取整个正则匹配的结果
  print(ret.group(1))   # 取第一个分组中的内容
  print(ret.group(2))   # 取第二个分组中的内容
  ```

- 分组命名

  - (?P<名字>正则表达式)          

    ```python
    s1 = '<h1>wahaha</h1>'
    s2 = '<a>wahaha ya wahaha</a>'
    ret = re.search('<(?P<tag>\w+)>.*?</(?P<tag2>\w+)>',s1)				#(?P<tag>)
    print(ret.group('tag'))
    ```

- 引用分组

  - 引用分组  (?P=组名)   这个组中的内容必须完全和之前已经存在的组匹配到的内容一模一样

  ```python
  s1 = '<h1>wahaha</h1>'
  s2 = '<a>wahaha ya wahaha</a>'
  ret = re.search('<(?P<tag>\w+)>.*?</(?P=tag)>',s1)				#(?P=tag)
  print(ret.group('tag'))
  ```

```python
#经典例题
#有的时候我们想匹配的内容包含在不相匹配的内容当中，这个时候只需要把不想匹配的先匹配出来，再通过手段去掉
import re
ret=re.findall(r"\d+\.\d+|(\d+)","1-2*(60+(-40.35/5)-(-4*3))")
print(ret)
ret.remove('')
print(ret)
```

#### 9.2.2 flags 

编译标志位，用于修改正则表达式的匹配方式，如：是否区分大小写，多行匹配等。常用的flags有：

| 标志               | 含义                                                         |
| ------------------ | ------------------------------------------------------------ |
| re.S(DOTALL)       | 使.匹配包括换行在内的所有字符                                |
| re.I（IGNORECASE） | 使匹配对大小写不敏感                                         |
| re.L（LOCALE）     | 做本地化识别（locale-aware)匹配，法语等![img](file:///C:/Users/tina/AppData/Local/YNote/data/heoffer@126.com/15ef610b4afd4cf0aea99402f970595e/19c23298f53f40f1b1d0168871156605.jpg) |
| re.M(MULTILINE)    | 多行匹配，影响^和$                                           |
| re.X(VERBOSE)      | 该标志通过给予更灵活的格式以便将正则表达式写得更易于理解     |
| re.U               | 根据Unicode字符集解析字符，这个标志影响\w,\W,\b,\B           |

```python
#示例
re.compile(pattern,flags=0)

pattern: 编译时用的表达式字符串。

flags 编译标志位，用于修改正则表达式的匹配方式，如：是否区分大小写，多行匹配等。
```

### 9.3 转义符

- r或者\为转义符
- 正则表达式中的转义符在python的字符串中也有转义的作用
- 所有的正则都已在工具中的测试结果为结果,在所有结果前加r
- ```\\n```匹配```\n```===>```r'\n'```
- ```\\\\n```匹配```\\n```===>```r'\\n'```

### 9.4 进阶方法

#### 9.4.1 时间复杂度

- 效率

#### 9.4.2 空间复杂度

- 内存占用

#### 9.4.3 用户体验

- 体验

## 第十章 网络编程

### 10.1 项目目录结构

- 通俗的理解`__name__ == '__main__'`：假如你叫小明.py，在朋友眼中，你是小明`(__name__ == '小明')`；在你自己眼中，你是你自己`(__name__ == '__main__')`。
- `if __name__ == '__main__'`的意思是：当.py文件被直接运行时，`if __name__ == '__main__'`之下的代码块将被运行；当.py文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行。

1. 脚本

   - 插入模块:先插入内置模块,然后第三方某块,上短下长!

2. 单可执行文件

   - config

     - 配置相关

   - db  (database)

     - 数据相关

   - lib   (librarie)

     - 公共功能

   - src

   - 业务相关

   - 主文件

     - ```python
       if __name__ == '__main__':
           start()
       ```

   - 日志文件

3. 多可执行文件(参照day23视频)

   - bin
     - 可执行文件,程序入口
     - 注意要点:模块目录添加
   - config
     - 配置相关
   - db  (database)
   
     - 数据相关
   
   - lib   (librarie)
     - 公共功能
   
   - src
     - 业务相关
   
   - log
     - 日志文件

### 10.2 网络基础

#### 10.2.1 B/S和C/S架构

- C/S 	微信,qq,迅雷等需要安装客户端的应用.
  - client 客户端
  - serve 服务端
- B/S   百度,知乎,博客园登不需要客户端,通过一个浏览器即可实现相关服务
  - browser 浏览器
  - server 服务端
- C/S架构与B/S架构的关系
  - B/S架构是一种特殊的B/S架构

#### 10.2.2 基础概念

1 网卡&mac地址

- **网卡是物理硬件**:ethernet规定接入internet的设备都必须具备网卡，发送端和接收端的地址便是指网卡的地址，即mac地址。
- **mac地址**：每块网卡出厂时都被烧制上一个世界唯一的mac地址，长度为48位2进制，通常由12位16进制数表示（前六位是厂商编号，后六位是流水线号）

2 交换机

- 交换机是连接多台机器并帮助通讯的物理设备,
- 普通**交换机只认识mac地址**,交换机通过arp协议识别一台机器
- 交换机进行局域网内的通讯
- 通讯方式: 广播/组播/单播

3 协议

- server和client得到的内容都是二进制,所以每一位代表什么就需要事先规定好,再按照约定进行发送和解析,这个约定就是协议.

3.1 arp协议(重点)

- 地址解析协议，即ARP（Address Resolution Protocol），是根据IP地址获取物理地址的一个TCP/IP协议。
- 由交换机完成:交换机**先广播 再单播**完成通讯
- arp协议:通过ip地址获取mac地址
- 交换机通过arp协议识别一台机器

3.2 IP协议

- 规定网络地址的协议叫ip协议
- 规定网络地址的协议叫ip协议，它定义的地址称之为ip地址，广泛采用的v4版本即ipv4，它规定网络地址32位2进制表示范围0.0.0.0-255.255.255.255
  一个ip地址通常写成四段十进制数，例：172.16.10.1
- IP协议的作用主要有两个，一个是为每一台计算机分配IP地址，另一个是确定哪些地址在同一个子网络。

4 IP地址

- 通过ip地址在网络上定位一台机器
- 规定网络地址的协议叫ip协议，它定义的地址称之为ip地址
  - **ipv4协议** : 用4位的点分十进制(由32位2进制表示),范围0.0.0.0-255.255.255.255
  - **ipv6协议** : 用6位得冒分十六进制,128位2进制表示,范围0:0:0:0:0:0-FFFF:FFFF:FFFF:FFFF:FFFF:FFFF

4.1公网ip

- 每一个ip地址想要被所有人访问到,那么这个ip地址必须申请

4.2内网ip

- 被保留的ip字段

  ```python
  # 192.168.0.0 - 192.168.255.255
  # 172.16.0.0 - 172.31.255.255
  # 10.0.0.0 - 10.255.255.255
  ```

4.3网关ip

- 一个局域网的网络出口,访问局域网之外的区域都需要经过路由器和网关

5 路由器

- 路由器进行局域网间的通讯
- 可以识别ip

6  广播

- 广播,单播,组播
- 主机之间“一对所有”的通讯模式，网络对其中每一台主机发出的信号都进行无条件复制并转发，所有主机都可以接收到所有信息（不管你是否需要）

7 网段

- 指的是一个地址段x.x.x.0 ,x.x.0.0

8 子网掩码

- 所谓”子网掩码”，就是表示子网络特征的一个参数。它在形式上等同于IP地址，也是一个32位二进制数字，它的网络部分全部为1，主机部分全部为0。比如，IP地址172.16.10.1，如果已知网络部分是前24位，主机部分是后8位，那么子网络掩码就是11111111.11111111.11111111.00000000，写成十进制就是255.255.255.0。

9 端口 port

- 范围:0-65535(建议使用8000以上端口)
- port能够在网络上定位一台机器上的一个服务
- ip+port 确认一台机器上的一个应用

### 10.3 ios五层协议

#### 10.3.1TCP协议

```
- tcp协议:是一个面向连接的,流式的(无边界),可靠地,慢的,全双工通信协议.
  应用场景:邮件发送,文件传输,http,web
- udp协议是一个面向数据报的,不可靠,快的,能够完成一对多/多对多/多对一的高效通讯协议
   应用场景:即时聊天工具,视频在线观看
```

- 可靠,速度慢,全双工通信

- 建立连接**三次握手**,断开连接**四次挥手**

- 建立起链接之后,发送每条消息都有回执,为了保证数据的完整性,还有重传机制

- 数据传输:有收必有发,收发必相等

- 长连接:会一直占用对方端口

- IO操作(input/output),IO操作的输入输出时相对内存来说

  - write-send (输出ouput)
  - read-recv (输入input)

  ```python
  #三次握手
  TCP是因特网中的传输层协议，使用三次握手协议建立连接。当主动方发出SYN连接请求后，等待对方回答SYN+ACK[1]，并最终对对方的 SYN 执行 ACK 确认。这种建立连接的方法可以防止产生错误的连接。[1] 
  TCP三次握手的过程如下：
  客户端发送SYN（SEQ=x）报文给服务器端，进入SYN_SEND状态。
  服务器端收到SYN报文，回应一个SYN （SEQ=y）ACK(ACK=x+1）报文，进入SYN_RECV状态。
  客户端收到服务器端的SYN报文，回应一个ACK(ACK=y+1）报文，进入Established状态。
  三次握手完成，TCP客户端和服务器端成功地建立连接，可以开始传输数据了。
                          
  #四次挥手
  (1) 某个应用进程首先调用close，称该端执行“主动关闭”（active close）。该端的TCP于是发送一个FIN分节，表示数据发送完毕。
  (2) 接收到这个FIN的对端执行 “被动关闭”（passive close），这个FIN由TCP确认。
  注意：FIN的接收也作为一个文件结束符（end-of-file）传递给接收端应用进程，放在已排队等候该应用进程接收的任何其他数据之后，因为，FIN的接收意味着接收端应用进程在相应连接上再无额外数据可接收。
  (3) 一段时间后，接收到这个文件结束符的应用进程将调用close关闭它的套接字。这导致它的TCP也发送一个FIN。
  (4) 接收这个最终FIN的原发送端TCP（即执行主动关闭的那一端）确认这个FIN。[1] 
  既然每个方向都需要一个FIN和一个ACK，因此通常需要4个分节。
  ```

  ```python
  #-----------面试-----------!!!!!!!!
  # 三次握手!!!!!!!
      # accept接受过程中等待客户端的连接
      # connect客户端发起一个syn链接请求
          # 如果得到了server端响应ack的同时还会再收到一个由server端发来的syc链接请求
          # client端进行回复ack之后，就建立起了一个tcp协议的链接
      # 三次握手的过程再代码中是由accept和connect共同完成的，具体的细节再socket中没有体现出来
  
  # 四次挥手!!!!!!!
      # server和client端对应的在代码中都有close方法
      # 每一端发起的close操作都是一次fin的断开请求，得到'断开确认ack'之后，就可以结束一端的数据发送
      # 如果两端都发起close，那么就是两次请求和两次回复，一共是四次操作
      # 可以结束两端的数据发送，表示链接断开了
  ```

  

![1557192436715](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1557192436715.png)

#### 10.3.2 UDP协议

- 不需要建立连接,速度特别快,可能会丢消息.

#### 小结(TCP/UDP重点)

- 应用场景
  - TCP:文件上传下载(邮件,网盘)
  - UDP:即时通讯(微信,qq)
- 传输文件长度:
  - TCP 长度无限
  - UDP 能够传输的数据航都是有限的,根据数据传递设备的设置有关系

#### 10.3.3  osi七层模型

- '应表会传网数物'

  ```也叫osi五层模型,专业七层,开发人员掌握五层模型,表示层会话层了解```

  - **应用层**:python代码
  - 表示层
  - 会话层
  - **传输层**:tcp协议 udp协议 端口
  - **网络层**:ipv4/ipv6协议
  - **数据链路层**:mac地址 arp协议
  - **物理层**:

![1557197573064](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1557197573064.png)

**每层运行常见协议和物理设备**---------面试------------

| tcp/ip五层  | 每层运行常见协议                | 每层运行常见物理设备   |
| ----------- | ------------------------------- | ---------------------- |
| 5应用层     | python代码/http/https/ftp/smtp/ |                        |
| 4传输层     | tcp/udp协议 端口                | 四层交换机/四层路由器  |
| 3网络层     | ipv4/ipv6协议                   | 三层路由器/三层交换机  |
| 2数据链路层 | mac地址/arp协议                 | 网卡/交换机/二层交换机 |
| 1物理层     |                                 |                        |

### 10.4 socket模块

TCP协议c/s基本格式:

```python
#server
import socket

sk = socket.socket()
sk.bind(('127.0.0.1',9000))
sk.listen()    # n 允许多少客户端等待
print('*'*20)
while True:   # outer
    conn,addr = sk.accept()    # 阻塞
    print(addr)
    while True:   # inner
        msg = conn.recv(1024).decode('utf-8')
        if msg.upper() == 'Q': break
        print(msg)
        inp = input('>>>')
        conn.send(inp.encode('utf-8'))
        if inp.upper() == 'Q':
            break
    conn.close()
sk.close()

#client
import socket

sk = socket.socket()
sk.connect(('127.0.0.1',9000))
while True:
    inp = input('>>>')
    sk.send(inp.encode('utf-8'))
    if inp.upper() == 'Q':
        break
    msg = sk.recv(1024).decode('utf-8')
    if msg.upper() == 'Q': break
    print(msg)
sk.close()
```

UDP协议c/s基本格式:

```python
#server端

import socket
sk = socket.socket(type = socket.SOCK_DGRAM)
sk.bind(('127.0.0.1',9000))
while True:
    msg,client_addr = sk.recvfrom(1024)
    print(msg.decode('utf-8'))
    msg = input('>>>').encode('utf-8')
    sk.sendto(msg,client_addr)
sk.close()

#client端
import socket

sk = socket.socket(type=socket.SOCK_DGRAM)
while True:
    inp = input('>>>').encode('utf-8')
    sk.sendto(inp,('127.0.0.1',9000))
    ret = sk.recv(1024)
    print(ret)
sk.close()
```

#### 10.4.1socket模块

- 中文名字:套接字
- **Socket是应用层与传输层中间的抽象层，Socket帮助去组织拼接信息数据，以符合指定的协议。**
- socket对于程序员来说,已经是网络操作的底层了                                     ![1557198171665](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1557198171665.png)

#### 10.4.2 socketserver模块

- socketserver实现一对多

- socketserver是socket的再封装，进一步简化了socket sever端的编写

- **固定格式**编写分三步来进行：

  - 先定义一个 处理消息类，继承 socketserver.BaseRequestHandler 类。
  - 重构该类中的 handle() 函数，这是你接收的数据最先到达的函数，同时根据在该类中完善其他的函数。
  - 实例化 socketserver.ThreadingTCPServer()类，将自己构建的类作为参数传递进去。

  ```python
  import socketserver
  
  # 第一步，定义消息处理函数
  class Myserver(socketserver.BaseRequestHandler):
      # 第二步，重写 handle() 类，让其处理消息
      def handle(self):
          while True:
              try:
                  self.data = self.request.recv(1024).strip()
                  print("{} worte:".format(self.client_address[0]))
                  print(self.data)
                  self.request.send(self.data.upper())
               
              except ConnectionResetError as e: # 断开连接通过异常处理
                  print("err",e)
                  break
  
  # 第三步，实例化对象
  server = socketserver.ThreadingTCPServer(('ip', port), Myserver)
  server.server_forever() # 开启服务器
  
  ```

  

### 10.5粘包

#### 10.5.1粘包概念:

- TCP粘包是指发送方发送的若干包数据到接收方接收时粘成一包，从接收缓冲区看，后一包数据的头紧接着前一包数据的尾。
- 粘包可能由发送方造成，也可能由接收方造成。
- 只有TCP有粘包现象，UDP永远不会粘包
- 粘包不一定会发生

**粘包原因:**

```所谓粘包问题主要还是因为接收方不知道消息之间的界限，不知道一次性提取多少字节的数据所造成的。```

- 发送端原因:  由于TCP协议本身的机制（面向连接的可靠地协议-三次握手机制）客户端与服务器会**维持一个连接**（Channel），数据在连接不断开的情况下，可以**持续不断地将多个数据包发往服务器**，但是如果发送的网络数据包太小，那么他本身会启用Nagle算法（可配置是否启用）**对较小的数据包进行合并**（基于此，TCP的网络延迟要UDP的高些）然后再发送（超时或者包大小足够）。那么这样的话，服务器在接收到消息（数据流）的时候就**无法区分哪些数据包是客户端自己分开发送的**，这样产生了粘包.
- 接收端原因:  服务器在接收到数据库后，**放到缓冲区**中，如果**消息没有被及时从缓存区取走**，下次在取数据的时候可能就会出现**一次取出多个数据包**的情况，造成粘包现象。

#### 10.5.2. tcp粘包解决办法

- 在每次使用tcp协议发送数据流时,在开头标记一个数据流长度信息,并固定该报文长度(自定义协议).在客户端接收数据时先接收该长度字节数据,判断客户端发送数据流长度,并只接收该长度字节数据,就可以实现拆包,完美解决tcp粘包问题.

#### 10.5.3struct模块

```python
#struct模块
#该模块可以把一个类型，如数字，转成固定长度为4的bytes类型
import struct
res = struct.pack('i',12345)	#i表示整数int
print(res,len(res),type(res))  #长度是4

res2 = struct.pack('i',12345111)
print(res,len(res),type(res2))  #长度也是4

unpack_res =struct.unpack('i',res2)
print(unpack_res)  #(12345111,)
print(unpack_res[0]) #12345111

```

```python
###################客户端client###################
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import socket
import struct
sock=socket.socket()	
sock.connect(('127.0.0.1', 13459))	

content1='我好'.encode('utf-8')		#要发送消息
content2='他也好'.encode('utf-8')

con1_len=struct.pack('i',len(content1))	# 计算要发送消息(字节)的长度,并使用struct模块转化为长度为4的字节b'\x06\x00\x00\x00'
sock.send(con1_len)						#先把这个4字节的报文发送
sock.send(content1)						#发送内容

con2_len=struct.pack('i',len(content2))
sock.send(con2_len)
sock.send(content2)


sock.close()
```

```python
###################服务端server###################
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import struct
import socket

sock = socket.socket()		#买手机
sock.bind(('127.0.0.1', 13459))		#插卡
sock.listen(10)  	#开机(同时最大连接10)

conn, addr = sock.accept()		#(受)与cilent端connect(攻)对应.
msg = conn.recv(4)				#首先接收4个字节(4个字节由client端struct模块转化)
len_msg= struct.unpack('i',msg)	#struct模块读取报文,判断跟随数据长度.返回值是一个元祖(6,)
size_msg=len_msg[0]					#取值判断跟随数据长度
msg = conn.recv(size_msg)			#接收报文读取长度字节
print(msg.decode('utf-8'))			#解码输出

msg=conn.recv(4)
len_msg=struct.unpack('i',msg)
size_msg=len_msg[0]
msg = conn.recv(size_msg)
print(msg.decode('utf-8'))


conn.close()
sock.close()

```

**----------------------女神总结--------------------------面试**

```python
# tcp协议的粘包现象

# 什么是粘包现象?
    # 发生在发送端的粘包
        # 由于两个数据的发送时间间隔短+数据的长度小
        # 所以由tcp协议的优化机制将两条信息作为一条信息发送出去了
        # 为了减少tcp协议中的“确认收到”的网络延迟时间
    # 发生再接收端的粘包
        # 由于tcp协议中所传输的数据无边界，所以来不及接收的多条
        # 数据会在接收放的内核的缓存端黏在一起
    # 本质： 接收信息的边界不清晰

# 解决粘包问题.
    # 自定义协议1
        # 首先发送报头
            # 报头长度4个字节(struct模块)
            # 内容是 即将发送的报文的字节长度
                # pack 能够把所有的数字都固定的转换成4字节
        # 再发送报文
    # 自定义协议2
        # 我们专门用来做文件发送的协议
            # 先发送报头字典的字节长度
            # 再发送字典（字典中包含文件的名字、大小。。。）
            # 再发送文件的内容
```

#### 10.6验证客户端合法性

- 利用加密算法
- 客户端和服务端都有秘钥
- 服务端给客户端随机分发字符串
- 秘钥+随机字符串验证合法性
  - md5/hamc模块  —— 校验客户端的合法性

## 第十一章 并发编程

### 11.1系统发展史

```操作系统负责调度进程,控制执行顺序,执行时间.负责资源分配```

- **多道程序**设计技术，就是指允许**多个程序**同时进入内存并运行。

  - **当一道程序因I/O请求而暂停运行时，CPU便立即转去运行另一道程序。**

  ```
  多道程序系统的出现，标志着操作系统渐趋成熟的阶段**，先后出现了作业调度管理、处理机管理、存储器管理、外部设备管理、文件系统管理等功能。
  
  由于多个程序同时在计算机中运行，开始有了**空间隔离**的概念，只有内存空间的隔离，才能让数据更加安全、稳定。除了空间隔离之外，多道技术还第一次体现了时空复用的特点，遇到IO操作就切换程序，使得cpu的利用率提高了，计算机的工作效率也随之提高。
  ```

- 分时系统——现在流行的PC，服务器都是采用这种运行模式，即把CPU的运行分成若干时间片分别处理不同的运算请求

- 实时系统——一般用于单片机上、PLC等，比如电梯的上下控制中，对于按键等动作要求进行实时处理 

- **通用操作系统：具有多种类型操作特征的操作系统。可以同时兼有多道批处理、分时、实时处理的功能，或其中两种以上的功能。**

  ```
  # 分时操作系统 + 多道操作系统 + 实时操作系统
      # 多个程序一起在计算机中执行
      # 一个程序如果遇到IO操作，切出去让出CPU
      # 一个程序没有遇到IO，但是时间片到时了，切出去让出CPU
  ```

### 11.2 IO操作

**io操作:**

```python
#io操作指对内存的输入和输出 input/output
```

- i (input) 向内存输入 input/read/recv/recvfrom/accept/connect/close
- o (output) 向内存输出  print/write/send/sendto/accept/close

### 11.3 进程

#### 11.3.1 进程概念

- 进程就是运行中的程序

  ```python
  #程序和进程之间的区别
  程序只是一个文件
  进程是这个文件被CPU运行起来了
  进程是计算机中最小的资源分配单位,在操作系统中的唯一标识符 ：pid
  ```

- 操作系统调度进程的算法

  - 短作业优先算法
  - 先来先服务算法
  - 时间片轮转算法
  - 多级反馈算法

- **同步&异步**

  - ```涉及到IO操作时```
    - 同步，就是发起调用后，被调用者处理消息，必须等处理完才直接返回结果，没处理完之前是不返回的，调用者主动等待结果；
    - 异步，就是发起调用后，被调用者直接返回，但是并没有返回结果，等处理完消息后，通过状态通知或者回调函数来通知调用者，调用者被动接收结果。

- **阻塞&非阻塞**

  -  ```涉及到CPU线程调度时```
    - 阻塞，就是调用结果返回之前，该执行线程会被挂起，不释放CPU执行权，线程不能做其它事情，只能等待，只有等到调用结果返回了，才能接着往下执行；
      - 非阻塞，就是在没有获取调用结果时，不是一直等待，线程可以往下执行，如果是同步的，通过轮询的方式检查有没有调用结果返回，如果是异步的，会通知回调。

- **并发**&**并行**

  - 并发:多个程序轮流使用一个cpu,轮流执行
  - 并行:多个程序,一个程序在一个cpu上运行,多个程序同时进行.

#### 11.3.2 进程的三状态

进程的三个状态:  就绪-->运行-->阻塞

![1557551924469](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1557551924469.png)



#### 11.3.3进程的创建

- multiprocessing模块
  - multiple 多元的   processing 进程
  - os.getpid() 查看进程pid

- 主进程-->子进程
  - pycharm进程里的程序都是charm的子进程
  - 主进程负责回收子进程的资源,如果子进程执行结束,父进程未回收资源,则该子进程变成僵尸进程
  - 主进程和子进程之间数据隔离
  - 主进程的结束逻辑:
    1. 主机进程开启
    2. 所有子进程结束
    3. 给子进程回收资源
    4. 主进程结束
  - 进程进程之间联系是基于文件的

- join方法

  - 阻塞,直到子进程结束就结束
  - 同步阻塞,直到对应的子进程结束之后结束阻塞

- **函数创建子进程**

  ```python
  #子进程开启方法
  import os
  import time
  from multiprocessing import Process
  
  def func(args):
      print('start',args,os.getpid())
      time.sleep(1)
      print('end',args,os.getpid())
  
  if __name__ == '__main__':
      p = Process(target=func,args=(1,))#实例化对象,args元组形式传参
      p.deamon = True		#把p子进程设置成了一个守护进程
      p.start()
      p.join()
      print('main:',os.getpid())
  ```

  - 面向对象创建子进程

  ```python
  #固定格式
  import time
  from multiprocessing import Process
  
  class MyProcess(Process):
      def __init__ (self,x,y):
          self.x=x
          self.y=y
          super().__init__()		#传参数时加上super()的init
      def run(self):		#gai方法的名字一定要写成run,不能变
          print(self.x,self.y)
          for i in range(5):
              print('in son1')
              time.sleep(1)
  
  if __name__=='__main__':
      mp = MyProcess(1,2) #传入参数(1,2)
      mp.daemon = True
      mp.start()
      time.sleep(1)
      print(1)
  ```

```python
#小明自己觉得自己是"main",别人觉得他是小明!
__name__=='__main__'
#执行文件就是__name__所在的文件
__name__=='__文件名__'
#执行的文件时是导入的模块
```

- 守护进程:
  - 参数(**p.daemon()**)可以把一个子进程设置为守护进程
    - 守护进程是随着**主进程的代码结束而结束**的
    - 所有的子进程都必须在主进程结束之前结束,由主进程回收资源
- Process模块
  - start() 开启进程	异步非阻塞
  - is_alive() 进程是否开启
  - **terminate()** 强制结束一个子进程  **异步非阻塞**
    - 存在问题:在terminate告诉操作系统关闭子进程时,继续执行下面代码,强制关闭需要时间消耗,在操作系统还没来得及关闭,立即查看该子进程时还是在运行
    - **最好的诠释了什么是异步非阻塞概念**

```python
import time
from multiprocessing import Process
  
def son1():
      while True:
          print('is alive')
          time.sleep(0.5)
  
  if __name__ == '__main__':
      p = Process(target=son1)
      p.start()      # 异步 非阻塞
      print(p.is_alive())
      time.sleep(1)
      p.terminate()   # 异步 非阻塞
      print(p.is_alive())  # 进程还活着 因为操作系统还没来得及关闭进程
      time.sleep(0.01)
      print(p.is_alive())   # 操作系统已经响应了我们要关闭进程的需求，再去检测的时候，得到的结果是进程已经结束了
```

#### 11.3.4 进程锁

```with 上下文管理```

- 加锁之后能够保障数据的安全性,降低了代码的执行效率
- **with lock** 给缩进下文加锁,与with open 类似,缩进内容结束后自动释放解锁
- 在主进程中实例化  lock=Lock()
- 在子进程中,对需要加锁的代码执行 with lock:
- 加锁应用场景:
  - 操作共享的数据资源(文件,数据库)
  - 对资源进行修改操作

```python
#示例
import time
import json
from multiprocessing import Process,Lock

def buy_ticket(user,lock):
    # with lock:
    # lock.acquire()   # 给这段代码加上一把锁
        time.sleep(0.02)
        with open('ticket_count') as f:
            dic = json.load(f)
            dic['count'] -= 1
    # lock.release()   # 给这段代码解锁

def task(user, lock):
    with lock:				#加锁最佳方式
        buy_ticket(user, lock)

if __name__ == '__main__':
    lock = Lock()
    for i in range(10):
        p = Process(target=task,args=('user%s'%i,lock))#把这把锁传递给子进程
        p.start()
        
        
#了解:
#with lock 与下面功能相同;
#- lock.acquire()   约束,加锁
#- lock.release()   释放,解锁
```

#### 11.3.5进程之间的通信  IPC

- 进程之间数据绝对隔离,如果通信肯定是通过文件或者网络实现通信
- **IPC** (inter process communication)
- Queue 模块```from multiprocessing import Queue```  
- 先进先出
- .put() 放数据当队列为满的时候,队列会阻塞
- .get() 取数据 当队列为空时,get会放生阻塞
- *.put_nowait() 放数据:当队列为满时,会报错,丢失数据*
- .get_nowait()  取数据: 当队列为空时,会报错,不阻塞

```python
#基本格式
import queue

from multiprocessing import Queue
q = Queue(5)
q.put(1)

print(q.get())
```

- Queue
  - 使用了文件家族的 socket 和 pickle 模块
  - 为了数据安全,也使用了 lock 模块,天生有锁,安全
  - *pipe 管道,天生没锁* 了解 

```python
IPC 进程之间通讯

- 常见内置ipc通讯机制:
  - 队列Queue和管道pipe
- 第三方公共提供的IPC机制(**消息中间键**)
  - redis
  - memcache
  - kafka
  - rabbitmq
- 第三方IPC机制与内置IPC机制
  - 第三方IPC机制--并发需求
  - 第三方IPC机制--高可用
  - 第三方IPC机制--断电保存数据
  - 第三方IPC机制--解耦
```

- 进程之间数据共享(了解)
  - 在mulprocessing中有一个manger类
    - manger封装了所有进程相关的 数据共享 数据传递等操作
    - 对字典.列表.这种数据操作的时候会有数据不安全的风险,需要加锁解决问题
    - 尽量减少这种数据共享操作

### 11.4线程

```python
#面试
# 1.什么是GIL
    # 全局解释器锁
    # 由Cpython解释器提供的
    # 导致了一个进程中多个线程同一时刻只能有一个线程访问CPU
```



#### 11.4.1 线程概念

- 线程是进程的一部分
- **所有进程至少有一个线程**,线程进行执行代码,不存储代码,进程是计算机中最小的资源分配单位,主要功能对数据进行隔离
- **进程**是计算机中**最小的资源分配**单位,**线程**是计算机中**能被cpu调度的最小**单位. * * * 
- 线程的创建也需要一些开销.
  - 线程的创建/销毁/切换时间开销都远远小于进程

```python
  #面试...异步非阻塞概念
  **1.```涉及到IO操作时```**
  
  - 同步，就是发起调用后，被调用者处理消息，必须等处理完才直接返回结果，没处理完之前是不返回的，调用者主动等待结果；
  - 异步，就是发起调用后，被调用者直接返回，但是并没有返回结果，等处理完消息后，通过状态通知或者回调函数来通知调用者，调用者被动接收结果。
  
  **2.```涉及到CPU线程调度时```**
  
  - 阻塞，就是调用结果返回之前，该执行线程会被挂起，不释放CPU执行权，线程不能做其它事情，只能等待，只有等到调用结果返回了，才能接着往下执行；
  - 非阻塞，就是在没有获取调用结果时，不是一直等待，线程可以往下执行，如果是同步的，通过轮询的方式检查有没有调用结果返回，如果是异步的，会通知回调。
```

```python
#面试题

# 守护线程t.daemon = True 等待所有的非守护子线程都结束之后才结束
# 非守护线程不结束，主线程也不结束
# 主线程结束了，主进程也结束
# 结束顺序 ：非守护线程结束 -->主线程结束-->主进程结束-->主进程结束--> 守护线程也结束
```

#### 11.4.2 线程创建

- 线程共享内存,数据存在安全隐患
  1. 操作的是全局变量
  2. 做以下操作
     - 先计算再赋值才容易出现数据不安全的问题
     - 包括+=/-=//+/*a (lst[0] += 1  dic['key']-=1)
- 解决线程数据安全问题
- 加锁Lock!
  - 加锁会影响程序的执行效率,保障了数据安全

1. 函数创建线程和面向对象创建子线程

   ```python
   #开启线程
   import os
   import time
   from threading import Thread
   # multiprocessing 是完全仿照这threading的类写的
   def func():
       print('start son thread')
       time.sleep(1)
       print('end son thread',os.getpid())
   # 启动线程 start
   Thread(target=func).start()
   print('start',os.getpid())
   time.sleep(0.5)
   print('end',os.getpid())
   
   ```

2. 守护线程

   ```python
   # 守护线程
   import time
   from threading import Thread
   def son1():
       while True:
           time.sleep(0.5)
           print('in son1')
   def son2():
       for i in range(5):
           time.sleep(1)
           print('in son2')
   t =Thread(target=son1)
   t.daemon = True
   t.start()
   Thread(target=son2).start()
   time.sleep(3)
   # 守护线程一直等到所有的非守护线程都结束之后才结束
   # 除了守护了主线程的代码之外也会守护子线程
   ```

### 11.5锁

#### 1.**互斥锁**Lock

- 互斥锁:
  - 在同一个线程中,不能连续acquire多次

#### 2.**递归锁 RLock**

- 递归锁在同一个进程中,可以连续多次acquire,不会被阻塞

- 递归锁占用了更多的资源,没有互斥锁效率高

  ![img](file:///C:\Users\big cattle\Documents\Tencent Files\694526621\Image\Group\ML}3$2IGV~JDKV20547BCZ1.png)

#### 3.单例加互斥锁

- 基本格式

  ```python
  class Foo(object):
      ____instance = None
      def __new__(cls, *args, **kwargs):
          if not cls.__instance:
                  time.sleep(0.1)
                  cls.__instance = object.__new__(cls)
          return cls.__instance
  obj1=Foo()
  obj2=Foo()
  ```

- **在线程中创建单例存在隐患!最终单例模式格式**!!

  ```python
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
          self.name = name
          self.age = age
  
  def func():
      a = A('alex', 84)
      print(a)
  
  from threading import Thread
  for i in range(10):
      t = Thread(target=func)
      t.start()
  ```

#### 4.死锁现象

- 死锁产生条件:

  1. 线程里有多把锁
  2. 多把锁交替使用

- **死锁解决办法:**

  1. 递归锁解决!

  - 快速解决死锁

    - 处理数据效率差

    ```python
    #递归锁解决死锁的的本质是把多个互斥锁变成了一把互斥锁
    #递归锁也会发生死锁现象!在多把递归锁交替使用的时候!
    ```

  2. 优化代码逻辑(互斥锁解决)!
     - 解决死锁速度慢,逻辑优化困难
     - 数据处理效率高

```python
#互斥锁
互斥锁 在一个线程中不能连续acquire多次、效率高，产生死锁的几率大
#递归锁
递归锁 在一个线程中可以连续acquire多次、效率低，一把锁永远不死锁
#死锁现象
死锁产生条件:
1. 线程里有多把锁
2. 多把锁交替使用
线程一直处在阻塞状态,产生死锁
```

### 11.6生产者消费者模型(fcfc重要)

```python
  生产者消费者模型当中有两大类重要的角色，一个是生产者（负责造数据的任务），另一个是消费者（接收造出来的数据进行进一步的操作）。

为什么要使用生产者消费者模型？
     在并发编程中，如果生产者处理速度很快，而消费者处理速度比较慢，那么生产者就必须等待消费者处理完，才能继续生产数据。同样的道理，如果消费者的处理能力大于生产者，那么消费者就必须等待生产者。为了解决这个等待的问题，就引入了生产者与消费者模型。让它们之间可以不停的生产和消费。

实现生产者消费者模型三要素：
    1、生产者
    2、消费者
    3、队列（或其他的容器，但队列不用考虑锁的问题）

什么时候用这个模型？
程序中出现明显的两类任务，一类任务是负责生产，另外一类任务是负责处理生产的数据的（如爬虫）

用该模型的好处？
1、实现了生产者与消费者的解耦和
2、平衡了生产力与消费力，就是生产者一直不停的生产，消费者可以不停的消费，因为二者不再是直接沟通的，而是跟队列沟通的。
```

#### 11.6.1 队列

```数据安全的数据类型```

- 先进先出队列:
  - Queue
  - 先来先服务server端
- 后进先出队列-栈:
  - LifQueue	(last in first out)
  - 算法应用较多
- 优先级队列:
  - PriorityQueue
  - 自动排序
  - 用户级别
  - 告警级别

#### 11.6.2 生产者消费者示例

- 生产者(**producer**):生产数据
- 消费者(**consumer**):处理数据
- 一个进程就是一个生产者或者消费者
- 生产者与消费者之间的容器就是队列,最安全的数据类型也是队列
- 应用:
  - **爬虫提高效率**



```python
#示例
import time
import random
from multiprocessing import Process,Queue

def producer(q,name,food):
    for i in range(10):				  #限制了队列数据值为10个
        time.sleep(random.random())    
        fd = '%s%s'%(food,i  
        q.put(fd)
        print('%s生产了一个%s'%(name,food))

        
def consumer(q,name):
    while True:						#一直取值
        food = q.get()				
        if not food:break				#取到空时,子进程结束
        time.sleep(random.randint(1,3))
        print('%s吃了%s'%(name,food))


def cp(c_count,p_count):
    q = Queue(10)				#限制队列的值的个数为10个
    for i in range(c_count):		#创建消费者
        Process(target=consumer, args=(q, 'alex')).start()
    p_l = []
    for i in range(p_count):		#创建生产者,并加入列表P-l
        p1 = Process(target=producer, args=(q, 'wusir', '泔水'))
        p1.start()
        p_l.append(p1)
    for p in p_l:p.join()			#结束所有生产者
    for i in range(c_count):#在生产者全部关闭后放入None到队列,当get取到None时,表示进程结束
        q.put(None)
if __name__ == '__main__':
    cp(2,3)						#要生产2个消费者和3个生产者
```

### 11.7池

- 预先开启固定个数进/线程数,当任务来临的时候,直接交给已经开好的进程去执行
  - 节省了额进程和线程的开启,关闭,切换都需要时间
  - 减轻了操作系统调度的负担
- concurrent.futures 模块,开启池

#### 11.7.1 进程池

- ProcessPoolExcutor类
- 进程池里的进程数一般为cpu个数或者加一
- 一个池中的进程个数限制了我们程序的并发个数

```python
#基本格式
from concurrent.futures import ThreadPoolExecutor,ProcessPoolExecutor

def func(i,name):
    print('start',os.getpid())
    time.sleep(random.randint(1,3))
    print('end', os.getpid())
    return '%s * %s'%(i,os.getpid())  #返回值
if __name__ == '__main__':
    p = ProcessPoolExecutor(5)		#池中进程数
    ret_l = []
    for i in range(10):
        ret = p.submit(func,i,'alex')
        ret_l.append(ret)				#提交任务
    for ret in ret_l:		
        print('ret-->',ret.result())  # ret.result()	取返回值, 同步阻塞
    p.shutdown()  ## 关闭池之后就不能继续提交任务，并且会阻塞，直到已经提交的任务完成
    print('main',os.getpid())
```

#### 11.7.2 线程池

- ThreadPoolExcutor类
- **线程池里的线程个数一般为cpu的4到5倍**

```python
#基本格式
#类不同,其余格式相同
from concurrent.futures import ThreadPoolExecutor
def func(i):
    print('start', os.getpid())
    time.sleep(random.randint(1,3))
    print('end', os.getpid())
    return '%s * %s'%(i,os.getpid())
tp = ThreadPoolExecutor(20)
# ret = tp.map(func,range(20))
# for i in ret:
#     print(i)
for i in range(10):
    ret = tp.submit(func,i)
tp.shutdown()
print('main')
```

#### 11.7.3 回调函数

- 执行完子线程任务之后直接调用对应的回调函数

```python
import requests
from concurrent.futures import ThreadPoolExecutor

def func(url):
    """
    线程池
    :return:
    """
    ret=requests.get(url)
    ret=ret.text
    return {'url':url,'ret':ret}
def foo(ret):
    ret=ret.result()
    print(ret['url'])

url=url_lst = [
    'http://www.baidu.com',  
    'http://www.cnblogs.com', 
    'http://www.douban.com',  
    'http://www.alibaba.com',
    'http://www.cnblogs.com/Eva-J/articles/8306047.html',
    'http://www.cnblogs.com/Eva-J/articles/7206498.html',
]
for url in url_lst:
    t=ThreadPoolExecutor(20)
    ret = t.submit(func,url)
    ret.add_done_callback(foo)

```

- 是单独开启线程还是池?
  - 如果只是开一个单独的子线程去做一件事,就可以单独开线程
  - 有大量任务等待去做,要求一定的并发,就需要开启线程池去做
  - 根据程序的IO操作判定是否开进程池:
    - 有大量的阻塞io使用池
    - 爬虫大量用到池
- 是否开启回调函数:
  - 执行完子线程任务之后直接调用对应的回调函数
  - 爬取网页,需要等待数据传输和网络上的响应高IO的不需要回调函数,子线程处理
  - 分析网页,没有什么io操作,这个操作没有必要再子线程完成,交给回调函数

```了解
进程和线程都有锁
-所有线程中能工作的基本都不能在进程中工作
-在进程中能使用的基本在线程中也可以使用
```

### 11.8 协程

```python
面试:进程线程和协程的区别
# 进程  
开销大 数据隔离 能利用多核 数据不安全 操作系统控制
# 线程  
开销中 数据共享 cpython解释器下不能利用多核 数据不安全 操作系统控制
# 协程  
开销小 数据共享 不能利用多核 数据安全 用户控制

```

#### 11.8.1协程的概念

- 能够在一个线程下的多个任务之间回切换,就叫协程

![1558063115300](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558063115300.png)

```进程和线程的切换是由操作系统控制切换```

- ```python
  协程和线程
  #共同点:
      -线程和协程的创建,切换销毁都需要时间开销,
      -在cpython中线程和协程都不能利用多个cpu(只能并发)
  #不同点:
      -多线程线程的切换是由操作系统完成,而协程的切换是通过代码实现,操作系统不可见
      -多线程创建,切换销毁时间开销较大,协程的时间开销很小,携程切换用户可操作性,不会增加操作系统压力
  ```

#### 11.8.2 协程操作(模块)

- 线程切换

  - 两种切换方式

    ```
    两种切换方式
        # 原生python完成   yield  asyncio
        # C语言完成的python模块  greenlet  gevent
    ```

    - 原生python代码实现     (asyncos 模块)- asyncos利用yield记录线程代码执行状态和位置,但是yield不能规避io操作
    - C语言完成的PYthon模块(#greenlet模块)

- 规避IO操作,切换原理(了解)

  ![1558067064860](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558067064860.png)

  

##### 11.8.2.1  gevent 第三方模块

- 基本格式

  ```python
  import time
  import gevent
  from gevent import monkey			
  monkey.patch_all()				#用mokey模块使协程识别多次使用的外部方法,比如time.time()
  def eat():
      print('wusir is eating')
      time.sleep(1)
      print('wusir finished eat')
  
  def sleep():
      print('小马哥 is sleeping')
      time.sleep(1)
      print('小马哥 finished sleep')
  
  # g1 = gevent.spawn(eat)   # 创造一个协程任务
  # g2 = gevent.spawn(sleep)   # 创造一个协程任务
  # print(g1.value)
  # print(g2.value)
  # # g1.join()   # 阻塞 直到g1任务完成为止
  # # g2.join()   # 阻塞 直到g1任务完成为止
  # gevent.joinall([g1,g2,g3])		#知道列表内的协程任务全部终止为止
  g_l = []
  for i in range(10):
      g = gevent.spawn(eat)
      g_l.append(g)
  gevent.joinall(g_l)
  ```

- 接收返回值

  - value

    ```python
    # g1 = gevent.spawn(eat)  # 创造一个协程任务
    # g2 = gevent.spawn(sleep)  # 创造一个协程任务 
    # print(g2.value)
    ```

##### 11.8.2.2 asyncio 内置模块

- *(记住启动一个/多个线程)*

- async-异步  sync-同步

- asyncio 协程基本格式

  ```python
  #起一个任务
  
  import asyncio			#插入asyncio模块
  async def demo():		#创建async函数
        print('start')
        await asyncio.sleep(1)	#阻塞,阻塞必须写入await之后	且使用asyncio模块自己的方法
        print('end')
    loop = asyncio.get_event_loop()   #创建一个第三方事件循环,监测是否IO
    loop.run_until_complete(demo())		#吧demo任务丢到事件循环中去执行
  ```

```python
#起多个任务,且没有返回值

import asyncio
  
async def demo():
    print('start')
    await asyncio.sleep(1)	#阻塞,阻塞必须写入await之后	且使用asyncio模块自己的方法
    print('end')

loop = asyncio.get_event_loop()
wait_obj=asyncio.wait([demo(),demo(),demo()])
loop.run_until_complete(wait_obj)
```

```
  #起多个任务,有返回值(了解)
  
  import asyncio 
  asrnc def demo():
  	print('start')
      await asyncio.sleep(1)	#阻塞,阻塞必须写入await之后	且使用asyncio模块自己的方法
      print('end')
      retunrn 123
  
  loop = asyncio.get-event_loop()
  t1 = loop.create_task(demo())
  t2 = loop.create_task(demo())
  tasks=[t1,t2]
  wait_obj=asyncio.wait([t1,t2])
  loop.run_until_complete(wait_obj)
  for i in easks:
  print(tasks.result())
```

  

- **asyncos是python原生的底层的协程模块**
  - 爬虫,webserver框架
  - 爬虫网络编程的效率和并发效果
- **await 阻塞必须写在await之后,告诉协程函数这里要切换出去,还能保证一会再切回来**
- **await 必须写在async函数里,async是协程函数**
- **loop 时间循环**
  - **所有的协程的执行,调度都离不开这个loop**



### 小结!!面试

1.互斥锁和递归锁异同

```python
互斥锁和递归锁
# 互斥锁 
在一个线程中不能连续acquire多次、效率高，产生死锁的几率大
# 递归锁 
在一个线程中可以连续acquire多次、效率低，一把锁永远不死锁
```

2.进程、线程、协程的区别 以及应用场景

```python
简述 进程、线程、协程的区别 以及应用场景？
#进程  
开销大 数据隔离 能利用多核 数据不安全 操作系统控制
#线程  
开销中 数据共享 cpython解释器下不能利用多核 数据不安全 操作系统控制
#协程  
开销小 数据共享 不能利用多核 数据安全 用户控制
# 在哪些地方用到了线程和协程
  1.自己用线程、协程完成爬虫任务
  2.但是后来有了比较丰富的爬虫框架
            # 了解到 scrapy /beautyful soup/aiogttp爬虫框架 哪些用到了线程，哪些用到了协程？
  3.web框架中的并发是如何实现的
            # 传统框架 ： django 多线程
            #             flask 优先选用协程 其次使用线程
            # socket server ：多线程
            # 异步框架 ：tornado，sanic底层都是协程
```

3.IPC 进程之间的通信机制

```python
# 内置的模块（基于文件） ：Pipe Queue
# 第三方工具（基于网络）: redis rabbitmq kafaka memcache  发挥的都是消息中间件的功能
```



### 11.9 并发汇总

```python
# 操作系统
    # 1.计算机中所有的资源都是由操作系统分配的
    # 2.操作系统调度任务：时间分片、多道机制
    # 3.CPU的利用率是我们努力的指标
# 并发
    # 进程 开销大 数据隔离 资源分配单位 cpython下可以利用多核
        # 进程的三状态：就绪 运行 阻塞
        # multiprocessing模块
            # Process-开启进程
            # Lock - 互斥锁
                # 为什么要在进程中加锁
                    # 因为进程操作文件也会发生数据不安全
            # Queue -队列 IPC机制（Pipe,redis,memcache,rabbitmq,kafka）
                # 生产者消费者模型
            # Manager - 提供数据共享机制
    # 线程 开销小 数据共享 cpu调度单位  cpython下不能利用多核
        # GIL锁
            # 全局解释器锁
            # Cpython解释器提供的
            # 导致了一个进程中多个线程同一时刻只有一个线程能当问CPU -- 多线程不能利用多核
        # threading
            # Thread类 - 能开启线程start，等待线程结束join
            # Lock-互斥锁  不能在一个线程中连续acquire，效率相对高
            # Rlock-递归锁  可以在一个线程中连续acquire，效率相对低
            # 死锁现象如何发生?如何避免？
        # 线程队列 queue模块
            # Queue
            # LifoQueue
            # PriorityQueue
    # 池
        # concurrent.futrues.ThreadPoolExecutor,ProcessPoolExecutor
            # 实例化一个池 tp = ThreadPoolExecutor(num),pp = ProcessPoolExecutor(num)
            # 提交任务到池中，返回一个对象 obj = tp.submit(func,arg1,arg2...)
                # 使用这个对象获取返回值 obj.result()
                # 回调函数 obj.add_done_callback(函调函数)
            # 阻塞等待池中的任务都结束 tp.shutdown()
   #协程全部!
# 概念
    # IO操作
    # 同步异步
    # 阻塞非阻塞

# 1.所有讲过的概念都记住
# 2.数据安全问题
# 3.数据隔离和通信
# 4.会用基本的进程 线程 池
```



## 第十二章 数据库

### 12.1 数据库初识



**1 基本概念**

- 数据库管理系统(**DBMS**): 专门管理数据库文件,帮助用户更简洁的操作数据的软件

- 数据库是cs架构的,操作数据文件

  - 能够帮助我们**解决并发**问题
  - 帮助我们**更简单更快速**的方式完成数的增删改查
  - 能够给我们提供一些**容错机制\高可用机制**
  - 能够**解决权限认证**问题

- 数据 data

- 文件

- 文件夹--数据库 database db

- 数据库管理员--DBA

  

**2  数据库软件分类**

```关系型数据库:字段与字段之间有关联!   主流数据库```

- **mysql**	
  - 开源
  - 小私企/互联网公司/
- oracle    
  - 收费
  - 比较严谨
  - 安全性比较高
  - 国企/事业单位/银行,金融行业
- sql server
- sql lite   框架中存在

```非关联数据库```

- redis
- mongodb

### 12.2 mysql基本语句

1. DDL 数据库定义语言:创建数据库,表,视图..

2. DML 数据库操纵语言:增删改查

3. DCL 数据库控制语言: 控制用户的访问权限

   ```
   mysqld install --安装数据库服务
   net start mysql --启动数据库的server端
   net stop mysql --停止数据库服务(mysql没有重启,只能先关闭再启动)
   mysql -uroot -p	-hip  客户端连入server端-u指定用户名-p指定密码-h指定ip
   ```

   - select user();  --查看当前登录用户**
   - **set password = password(密码); --设置密码**
   - create user '名称'@'192.168.12.%' identified by '123'; --创建用户名,192.168.12.网段的都可以连接
   - **show databases; --查看文件夹**
   - **create databases 名字; --创建文件夹**
   - grant all on day37.* to '名称'@'192.168.12.%';  --把库day37.*授权给'名称'所有权限all
   - **grant all on day37.* to 'bigox'@'192.168.12.%' identified by '123';  -把库day37.*授权给bigox所有权限all,big不存在就创建,密码为123**

#### 12.2.1 创建库

- create database 数据库名; --创建数据库
- show databases; --查看当前有多少个数据库(文件夹下)
- select database(); --查看当前使用数据库
- use demo --切换到demo数据库下
- drop database 数据库名; --删除数据库

#### 12.2.2 创建表

- create table 表名(id int,name char(10)); --创建表,表内字段id,name.(4是约束name长)
- show tables; --查看当前文件夹有多少表
- drop table 表名; --删表
- desc 表名; --查看表结构(descript描述)

### 12.3 表的基本操作

```python
增:insert into
删:delete from
改:update
查:select

# 增加 insert
insert into 表名 values (值....)
    # 所有的在这个表中的字段都需要按照顺序被填写在这里
insert into 表名(字段名，字段名。。。) values (值....),(值....),(值....)
    # 所有在字段位置填写了名字的字段和后面的值必须是一一对应
# 删除 delete
delete from 表 where 条件;
# 更新 update
update 表 set 字段=新的值 where 条件；
# 查询select语句
select * from 表
select 字段,字段.. from 表
select distinct 字段,字段.. from 表  # 按照查出来的字段去重
select 字段*5 from 表  # 按照查出来的字段去重
select 字段  as 新名字,字段 as 新名字 from 表  # 按照查出来的字段去重
select 字段 新名字 from 表  # 按照查出来的字段去重
```

- show variables like '%chara%';    查看chara编码项%engine%查看配置项
- show create table 表名;    查看表结构,全部信息
- describe 表名;    查看表的基本信息
- create table 表名 (id int)engine=innodb; 创建一个引擎为innodb的表

```python
select 后跟函数; #查看相关内容

- database(); 数据库
- user(); 用户
- now(); 时间
```



### 12.4 表的存储

- 存储方式

  1. 方式一(**MyISAM存储引擎**):表结构/表中的数据/索引

     - mysql5.5版本之下默认存储方式使用MyISAM存储引擎

       ```python
       #MyISAM存储引擎
       - 只支持表级锁:  在多用户操作数据事锁定某一张表保护数据安全.
       - 适合做读 ,插入数据比较频繁的,对修改和删除比较少的
       - mysql5.5版本之下默认存储方式使用MyISAM存储引擎
       ```

  2. 方式二(**InnoDB存储引擎**):表结构/表中的数据

     - mysql5.6版本以上默认存储方式使用InnoDB存储引擎

       ```python
       #InnoDB存储引擎
       - 支持 事务:例如在金融转账时保障数据安全
       - 支持 行级锁(也支持表级锁): 在多用户操作数据事锁定某一行保护数据安全.
       - 支持 外键:表与表之间关联
       - 适合并发比较高的数据库,对事物一致性要求较高的,相对更适应频繁的修改删除操作
       - 索引和数据存储在一起
       - mysql5.6版本以上默认存储方式使用InnoDB存储引擎
       ```

        ![1558405344889](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558405344889.png)
            
        ![1558405356448](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558405356448.png)
            
          ![1558405379905](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1558405379905.png)

  3. 方式三(**MEMORY存储引擎**):表结构

     - 存储在内存中
     - 优势:增删改查速度快
     - 劣势:重启数据消失,容量有限

#面试题

```python
# 用什么数据库 ： mysql
# 版本是什么 ：5.6
# 都用这个版本么 ：不是都用这个版本
# 存储引擎 ：innodb
# 为什么要用这个存储引擎：
    -支持事务 
    -支持外键 
    -支持行级锁 能够更好的处理并发的修改问题
```

### 12.5 数据类型

#### 12.5.1**数值类型**

- 整数

  - int()  默认是有符号的,可以不用添加

  - int(8)  能表示数字的范围不被宽度约束,只能约束数字的显示宽度

  - **(id int unsigned)  表示无符号**

    ```python
    # 创建表一个是默认宽度的int，一个是指定宽度的int(5)
    mysql> create table t1 (id1 int,id2 int(5));
    Query OK, 0 rows affected (0.02 sec)
    ```

- 小数

  - **float**

  - double 表示小数位更长

    ```python
    # 创建表的三个字段分别为float，double和decimal参数表示一共显示5位，小数部分占2位
    mysql> create table t2 (id1 float(5,2),id2 double(5,2),id3 decimal(5,2));
    Query OK, 0 rows affected (0.02 sec)
    ```

#### 12.5.2.**日期和时间**

- year 年

- **date** 年月日

- time 时分秒

- **datetime** /timestamp年月日时分秒timestamp不能为空

  ```python
  #时间书写格式
  --now()当前时间
  --values (now(),now(),now())
  --values (19700101080001)
  --values ('2038-01-19 11:14:07')
  
  # timestamp时间的下限是19700101080001
  # timestamp时间的上限是2038-01-19 11:14:07
  ```

#### 12.5.3.**字符串**

- char 定长字符串

  - char(5),如果只写alex四位,但是系统会在alex后加一个空格,补全五位,查询出来自动去掉空格

- varchar  变长字符串

  - varchar (10), alex-->alex4

  ```python
  #char&varchar区别:(面试)!!
  --varchar :节省空间\存取效率相对较低
  --char; 浪费空间,存取效率相对较高,长度变化较小的使用char
  ```

#### 12.5.4.**ENUM(枚举类型)和SET类型**

```ENUM只允许从值集合中选取单个值，而不能一次取多个值。```

```set允许值集合中任意选择1或多个元素进行组合```

- enum(1,2) 只能选择1或者2

- set(1,2,3,4,5) 能选1,12,123,2345....可多选

  ```python
  #enum格式
  # 选择enum('female','male')中的一项作为gender的值，可以正常插入
  mysql> insert into t10 values ('nezha','male');
  Query OK, 1 row affected (0.00 sec)
  ------------------------------------------------------
  #set格式
  #可以任意选择set('抽烟','喝酒','烫头','翻车')中的项，并自带去重功能
  mysql> insert into t11 values ('yuan','烫头,喝酒,烫头');
  Query OK, 1 row affected (0.01 sec)
  ```

### 12.6 表约束



- unsigned  --设置某一个数字无符号

- not null  --某一个字段不能为空

  - create table t(id int **not null**);

  ```python
  #not null 不生效
  
  设置严格模式：
      不支持对not null字段插入null值
      不支持对自增长字段插入”值
      不支持text字段有默认值
  
  直接在mysql中生效(重启失效):
  mysql>set sql_mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION";
  
  配置文件添加(永久失效)：
  sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
  
  not null不生效
  
  ```

  

- default  --给某一个字段设置默认值

  - create table t(id int not null, gender enum('女','男') not null **default '女'**);
    - **insert into t(id) values(1)**;

- unique  --设置某一个字段不能重复

  - create table t(id int **unique**);
  - mysql可以多个字段为空

  ```python
  #联合唯一
  create table t(id int,ip char(15),port int,server char(10),unique(ip,port))
  #ip和port 不能同时重复
  ```

- auto_increment  --设置某一个int类型的字段 自动增加--自增(auto 自动  increment  增量)

  - 自增字段,必须是数字,且必须是唯一unique;
  - 自带:非空且唯一且自增,是数字
  - create table t(id **int unique auto_increment**)

- primary key  --设置某一个字段非空且不能重复

  - primary key 主键
  - 一张表只能设置一个主键
  - 一张表最好设置一个主键
  - 约束这个字段非空(not null)且唯一(unique)
  - 系统自动指定第一个非空且唯一的字段为主键

  ```python
  #id为主键,系统自动指定第一个非空且唯一的字段为主键
  create table t(id int not null unique,name char(12) not null unique);
  #id为主键
  create table t(id int primary key,name char(12));
  
  #联合主键
  create table t(id int,name char(12),gender char(1),primary key(id,name));
  #id,name不能同时重复
  ```

- foreign key  --外键

  - 两张表创建联系
  - 先创建外键表,最后创建主表

  ```python
  #表类型必须是innodb存储引擎，且被关联的字段，即references指定的另外一个表的字段，必 须保证唯一
  create table department(
  id int primary key,
  name varchar(20) not null
  )engine=innodb;
  
  #dpt_id外键，关联父表（department主键id），同步更新，同步删除
  create table employee(
  id int primary key,
  name varchar(20) not null,
  dpt_id int,
  foreign key(dpt_id)
  references department(id)
  on delete cascade  # 级连删除
  on update cascade # 级连更新
  )engine=innodb;
  
  
  #先往父表department中插入记录
  insert into department values
  (1,'教质部'),
  (2,'技术部'),
  (3,'人力资源部');
  
  
  #再往子表employee中插入记录
  insert into employee values
  (1,'yuan',1),
  (2,'nezha',2),
  (3,'egon',2),
  ```

### 12.7 修改表结构(了解)

```python
语法：
#1. 修改表名
      ALTER TABLE 表名 
                      RENAME 新表名;

#2. 增加字段
      ALTER TABLE 表名
                      ADD 字段名  数据类型 [完整性约束条件…],
                      ADD 字段名  数据类型 [完整性约束条件…];
                            
#3. 删除字段
      ALTER TABLE 表名 
                      DROP 字段名;

#4. 修改字段
      ALTER TABLE 表名 
                      MODIFY  字段名 数据类型 [完整性约束条件…];
      ALTER TABLE 表名 
                      CHANGE 旧字段名 新字段名 旧数据类型 [完整性约束条件…];
      ALTER TABLE 表名 
                      CHANGE 旧字段名 新字段名 新数据类型 [完整性约束条件…];

#5.修改字段排列顺序/在增加的时候指定字段位置
    ALTER TABLE 表名
                     ADD 字段名  数据类型 [完整性约束条件…]  FIRST;
    ALTER TABLE 表名
                     ADD 字段名  数据类型 [完整性约束条件…]  AFTER 字段名;
    ALTER TABLE 表名
                     CHANGE 字段名  旧字段名 新字段名 新数据类型 [完整性约束条件…]  FIRST;
    ALTER TABLE 表名
                     MODIFY 字段名  数据类型 [完整性约束条件…]  AFTER 字段名;
```

### 12.8 表关系

- 多对一 

  - 学生表  关联  班级表,  学生表是多,班级表是一
  - 永远在多的表中设置外键(foreign key)

- 一对一

  - 客户关系表+学生表
  - 永远是后出现的那张表创建外键(foreign key unquiet)

- 多对多

  - 出版社+书
  - 产生第三张表,把两个关联关系的字段作为第三张表的外键

  ```python
  #多对多
  三张表：出版社，作者信息，书
  多对多：一个作者可以写多本书，一本书也可以有多个作者，双向的一对多，即多对多
  关联方式：foreign key+一张新的表
  
  create table book(
  id int primary key auto_increment,
  name varchar(20),
  press_id int not null,
  foreign key(press_id) references press(id)
  on delete cascade
  on update cascade);
  
  create table author(id int primary key auto_increment,name varchar(20));
  
  #这张表就存放作者表与书表的关系，即查询二者的关系查这表就可以了
  create table author2book(
  id int not null unique auto_increment,
  author_id int not null,
  book_id int not null,
  foreign key(author_id) references author(id)
  on delete cascade
  on update cascade,
  foreign key(book_id) references book(id)
  on delete cascade
  on update cascade,
  primary key(author_id,book_id));
  
  #插入四个作者，id依次排开
  insert into author(name) values('egon'),('alex'),('yuanhao'),('wpq');
  
  #每个作者与自己的代表作如下
  egon: 九阳神功九阴真经九阴白骨爪独孤九剑降龙十巴掌葵花宝典alex: 
  九阳神功葵花宝典yuanhao:独孤九剑降龙十巴掌葵花宝典wpq:九阳神功
  
  insert into author2book(author_id,book_id) values(1,1),(1,2),(1,3),(1,4),(1,5),(1,6),(2,1),(2,6),(3,4),(3,5),(3,6),
  (4,1);
  ```

### 12.9 单表查询

#### 12.9.1 关键字优先级

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

#### 12.9.2 简单查询

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

#### 12.9.3 where约束

- 比较运算:;

  - 数学运算 ><  >=  <=   <>不等于   != 不等于  
  - 逻辑运算 not and or

- 范围筛选 

  - 逻辑运算与in, between 拼接
  - in(80,90,100) 值是80或90或100 ---多选一
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

#### 12.9.4 group by 分组(!!!)

- 执行优先级从高到低：where > group by > having 
- 总是根据会重复的项来进行分组
- 分组总是和聚合函数一起用

```python
SELECT post FROM employee GROUP BY post;
# 会把字段post里相同的值归为一组,并只显示改组的第一个,但是所有数据仍保存在组内
```

#### 12.9.5 聚合函数(!!!)

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

#### 12.9.6 分组聚合(!!!面试)

```python
#按照岗位分组，并查看每个组有多少人
select post,count(id) as count from employee group by post;
```

#### 12.9.7  HAVING过滤(!!!)

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

#### 12.9.8 ORDER BY 查询排序

- order by 某一个字段 asc;   --默认是升序asc,从小到大
- order by 某一个字段 desc;   --指定降排列,从大到小
- order by 第一个字段 asc,第二个字段 desc;  --指定先根据第一个字段升序排列，在第一个字段相同的情况下，再根据第二个字段排列

#### 12.9.9 limit

- 取前n个  limit n   ==  limit 0,n
- 分页    limit m,n   从m+1开始取n个
- limit n offset m == limit m,n  从m+1开始取n个

### 12.10 多表查询

<https://www.cnblogs.com/Eva-J/articles/9688383.html>

- 子查询和连表查询**优先使用连表查询,效率更高**

#### 12.10.1 连表查询

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

#### 12.10.2 子查询

- 子查询是将一个查询语句嵌套在另一个查询语句中。
- 内层查询语句的查询结果，可以为外层查询语句提供查询条件。
- 子查询中可以包含：IN、NOT IN、ANY、ALL、EXISTS 和 NOT EXISTS等关键字
- 还可以包含比较运算符：= 、 !=、> 、<等

------

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

- **带比较运算符的子查询**

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

### 12.11 索引

#### 12.11.1 基本概念

- 索引概念
  - 建立起的一个在存储表阶段就有的一个**存储结构**,能够**加快查询速度**;
- 索引的重要性
  - 数据库读写比例: 10:1
  - 读的速度至关重要,增加查询速度
- **索引的特点**
  - 能够**加快查询速度**
  - 增加了读时需要的时间

```python
#建表、使用sql语句的时候注意的
    char 代替 varchar
    连表 代替 子查询
    创建表的时候 固定长度的字段放在前面
```



#### 12.11.2 索引的原理

- block 磁盘预读原理;
  - 磁盘预读单位是一个block块,在linux中一个block块大小为4096个字节;
- 硬盘读取的io操作的时间非常长,比cpu执行指令时间长很多,尽量减少io次数,才是读写数据主要解决的问题



#### 12.11.3.数据库的存储方式

##### 	3.1 树---新的数据结构

- 根--节点--分支--叶
- b树(平衡树balance tree)



##### 	3.2 存储方式

- **mysql--innodb--使用b+树存储索引**

  ```python
  #b+树  在b树的基础上进行了改良 - 
  -1. 分支节点和根节点都不再存储实际的数据了,让分支和根节点能存储更多的索引的信息,就降低了树的高度,所有的实际数据都存储在叶子节点中
  -2. 在叶子节点之间加入了双向的链式结构方便在查询中的范围条件
  ```

  

- **mysql中,所有的b+树索引的高度基本控制在3层**

  ```python
  #mysql中,所有的b+树索引的高度基本控制在3层的优点
  1. io操作的次数非常稳定,也很少
  2.有利于通过的范围查询
  ```



##### 3.3 索引效率

- **b+树的高度会影响索引的效率** 

  ```python
  #  b+树的高度会影响索引的效率,树的高度影响因素
  1.选择尽量短的列做索引;
  2.对区分度高的列做索引,重复率超过10%不适合;
  ```



#### 12.11.4. 聚集索引和辅助索引 !!!

##### 4.1 聚集索引和辅助索引概念

- **在innodb中,聚集索引和辅助索引并存,在myisam中,只有辅助索引;**

- **聚集索引:数据直接存储在树结构的叶子**

  - 主键创建的索引是聚集索引
  - 其他键创建的索引都是辅助索引

- **辅助索引:**数据不存在树中

  ```python
  #聚集索引:   主键创建的索引是聚集索引,速度稍快
  #辅助索引   主键之外其他键创建的索引都是辅助索引,速度相对较慢
  ```



##### 4.2 索引的创建

```innodb一定要创建主键,作为聚集索引```

- **关键字primary key 主键,自带索引,且是聚集索引**

  - 约束:非空+唯一
  - 可以创建联合聚集索引,根据两个字段创建联合聚集索引

- **关键字unique 自带辅助索引**

  - 约束:唯一
  - 可以创建联合辅助索引

- **index 创建辅助索引(对一个已经存在的字段加索引)**

  - 格式:    create index 索引名字 on 表(字段);  创建辅助索引
  - 无约束作用

- drop index 索引名字 on 表(字段);  删除索引

  ```python
  #创建单列索引注意事项
  选择一个区分度高的列建立索引,条件中的列不要参与计算,条件的范围尽量小,使用and作为条件的连接符
  #使用or来链接多个条件
   在满足上述条件的基础上,对or相关的所有列分别创建索引.
  ```



##### 4.3 索引不生效的原因

1. **索引列**不能参与计算,参与计算索引不生效

2. **查询的数据范围大**,索引不生效

   - 比较运算 </>/>=/<=----

   - 范围查询 between  and

     ```python
     #分页 面试题
     select * from 表 order by age limit 1000000,5;
     select * from 表 where id between 1000000 and 1000005; #速度更快,分页推荐格式
     ```

   - 模糊查询 like

     - 结果的范围大,索引不生效
     - 结果 abc% 索引生效, %abc索引不生效

3. **如果一列的内容区分度不高,索引不生效**

4. **对两列内容进行条件查询时**

   - and  

     - and条件两端的内容,优先选择一个有索引的,并且树形结构更好的进行查询;
     - 两个条件先完成范围较小的,缩小压力

     ```python
     #两个条件都要成立
     select * from 表 where id = 1000000 and email = '123@163.com';
     ```

   - **or or条件的不会进行优化,索引不生效**

     - 条件中有or的想要命中索引的,or两边的条件都要有索引

     - ```python
       select * from 表 where id = 1000000 or email = '123@163.com';
       ```



#### 12.11.5. 联合索引(面试!!!!)

```联合索引使用场景:用a对abc进行条件索引```

- 在联合索引当中如果**使用了or条件,索引不生效;**

- **最左前缀原则**:在联合索引查询时,查询条件中必须含有创建联合索引时的第一个索引列(**a**,b,c),--查询条件中必须含有a;

- **在整个条件中,从开始出现模糊匹配的那一刻,索引就失效了;**

  ```python
  create index ind_new on s1(id,email);
  select * from 表 where id = 1000000 or email = '123@163.com'; #不生效
  
  
  最左前缀原则
  # (a,b,c,d)
                  # a,b
                  # a,c
                  # a
  ```

  

#### 12.11.6.覆盖索引

- 如果我们使用索引作为条件查询,查询完毕后不需要回表查,就是**覆盖索引**

  ```explain select id from s1 where id = 1000000;```

#### 12.11.7. 索引合并

- 对两个字段分别创建索引，由于sql的条件让两个索引同时生效了，那么这个时候这两个索引就成为了合并索引

#### 12.11.8. 执行计划

- explain 执行计划

- ```python
  #执行计划 : 如果你想在执行sql之前就知道sql语句的执行情况，那么可以使用执行计划
   # 30000000条数据
   # sql 20s
  explain sql   --> 并不会真正的执行sql，而是会给你列出一个执行计划
  ```

#### 12.11.9. 数据备份

- 数据备份

  ```python
  #语法：
  # mysqldump -h 服务器 -u用户名 -p密码 数据库名 > 备份文件.sql
  
  #示例：
  #单库备份
  mysqldump -uroot -p123 db1 > db1.sql
  mysqldump -uroot -p123 db1 table1 table2 > db1-table1-table2.sql
  
  #多库备份
  mysqldump -uroot -p123 --databases db1 db2 mysql db3 > db1_db2_mysql_db3.sql
  ```

- 数据恢复

  ```python
  mysql> source /root/db1.sql
  ```

#### 12.11.10. 事务

```python
begin;  # 开启事务
select * from emp where id = 1 for update;  # 查询id值，for update添加行锁；
update emp set salary=10000 where id = 1; # 完成更新
commit; # 提交事务
```

#### 12.11.11.sql注入

- 在pymysql中操作数据库时使用固定输入格式,防止sql注入:

  ```python
  #示例
  import pymysql
  
  conn = pymysql.connect(host = '127.0.0.1',user = 'root',
                         password = '123',database='day41')
  cur = conn.cursor()
  username = input('user >>>')
  password = input('passwd >>>')
  sql = "select * from userinfo where name = %s and password = %s"
  cur.execute(sql,(username,password))
  print(cur.fetchone())
  cur.close()
  conn.close()
  ```

### 12.12 pymysql模块

<https://www.cnblogs.com/Eva-J/articles/9772614.html>

- 第三方模块,操作数据库

```python
import pymysql

db = pymysql.connect("数据库ip","用户","密码","数据库" ) # 打开数据库连接
cursor = db.cursor(pymysql.cursors.DictCursor)       ## 创建一个游标对象cursor,pymysql.cursors.DictCursor使输出内容变成字典
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
   db.commit()          # 提交结果,增删改操作使用
except:
   db.rollback()        # 发生错误时回滚
```



## 第十三章 前端开发

### 13.1 HTML

1.HTML

- html--超文本标记语言
- html特点
  - 对换行和空格不敏感
  - 空白折叠:对空格和换行都折叠成一个空格
- 标签:也叫标记
  - 双闭合标签  <span></span>
  - 单闭合标签  <meta />
- 注释:<!--注释内容-->

2. head标签

- meta 基本网站元信息标签
- **title 标题标签**,每个标签都有
- link 链接css文件 
- script 链接JavaScript
- **style  内嵌样式,style属性,每个标签都有**

3. body标签

- h1.h2....h6 标题标签

- p 段落标签(&nbsp ;空格,要加分号)--控制分行并添加间隙

- **br 换行标签,无间隙**

- u 下划线标签

- **a 锚点(anchor) 超链接标签**,a标签属性有

  - href：目标URL

  - title：悬停文本

  - style:  行内样式,去除下划线..

  - name：主要用于设置一个锚点的名称

  - text-decoration : none 去除下划线

  - target：告诉浏览器用什么方式来打开目标页面。target属性有以下几个值：

    ```python
    _self：在同一个网页中显示（默认值）
    _blank：在新的窗口中打开。
    #如果不写target=”_blank”那么就是在相同的标签页打开，如果写了target=”_blank”，就是在新的空白标签页中打开。
    ```

- **img 图片标签 属性有:**

  - src 图片路径,图片资源
  - title 标题

- alt  图片加载失败之后显示alt='内容'的内容

- hr 分割线标签

- strange 加粗标签

- em 斜体标签

- i 斜体标签

- u 下划线标签

- sup / sub 角标标签

  ------

3.列表与表格标签

- ol (ordered list ) 有序标签

  - li

- ul 无序标签

  - li
  - 默认实心圆
  - square 是新方点
  - circle 空心圆

- table 表格标签

  - th 表头内容

- tr 行

  - td 内容

  - ```
    border="1" cellspacing="0"表格线,间隙为0
    ```

4.表单标签 

- form 表单标签

  - action  动作(指定要提交到的服务器网址)

  - method  内容体哦叫方法

    - get	上传文件限制大小2k
    - post 上传大小没有限制,用post提交账号密码

  - input  输入框

    - text 文本输入

    - password 密码输入

    - submit 提交按钮

      ```
      <input type="submit" value="登录">
      ```

    - radio 单选

      - checked 默认选择
      - 产生互斥效果，给每个单选按钮设置相同的name属性值

    - checkbox  多选

      - checked 默认选择

    - select 下拉列表多选

      - selected 默认选择
      - mutiple 展开多选

  - time/datetime-local 时间

  - textarea 多行输入

5.其他

- div 盒子标签 +span
  - 把网页分割成不同的独立的逻辑区域,每个区域的内容独立,互不影响
- span  一行显示,与不加没有区别
- lable  创建关联
  - 点击登录按钮进入文本输入框

```html
<form action="">
		<label for="username">用户名：</label>
		<input type="text" id="username">
		<label for="pwd">密码：</label>
		<input type="text" id="pwd">
	</form>
```



```python
#1.在一行内显示的标签有哪些？
b/strong/i/em/a/img/input/td/span
```

```python
#2.独占一行的标签有哪些？
h1~h6/ul/ol/li/form/table/tr/p/div
```

- div 盒子标签 +span
  - 把网页分割成不同的独立的逻辑区域,每个区域的内容独立,互不影响
- span  一行显示,与不加没有区别
- lable  创建关联
  - 点击登录按钮进入文本输入框

```html
<form action="">
		<label for="username">用户名：</label>
		<input type="text" id="username">
		<label for="pwd">密码：</label>
		<input type="text" id="pwd">
	</form>
```

### 13.2 CSS

1.css的三种引入方式

`/*我是css注释*/`

```css中文名字:层叠样式表```

- 行内样式

  - 优先级最高

  ```html
  <div id="box" style="color:red;">
  ```

- 内嵌式

  ```html
  <style>
          #box{
              background-color: greenyellow;
          }
  </style>
  ```

- 外接式

  ```html
  <link rel="stylesheet" href="要导入css文件路径">
  ```

  ```行内样式大于内嵌式和外接式,内嵌式和外接式同时存在时谁在后面谁生效```

2.css的三种选择器

2.1块级标签和行内标签

```python
#1.在一行内显示的标签有哪些？
b/strong/i/em/a/img/input/td
#2.独占一行的标签有哪些？
h1~h6/ul/ol/li/form/table/tr/p/div

#块级标签(block):
--独占一行;
--可以设置宽高,如果不设置宽高,默认是父标签的100%宽度;

#行内标签(inline):
-- 在一行内显示;
-- 不可以设置宽高,如果不设置宽高,默认尺寸是字体大小;
	#行内块标签(inline-block):是行内标签的一种(input,img)可以设置宽高,行内转行内块很常用;
    
```

2.2 css三种选择器

- 基础选择器

  - id选择器:id是唯一的`#id`
  - 类选择器:class可以重复,可以设置多个`.类名`
  - 标签选择器:选择标签直接写标签名字

- 高级选择器

  - 后代选择器

    ```
    div p{
    	color: red;
    }
    ```

  - 子代选择器

    ```
    div>p{
    	color:red;
    }
    ```

  - 组合选择器

    ```
    div,p,body,html,ul,ol....{
        padding: 0;
        margin: 0;
    }
    ```

  - 交集选择器

    ```
      
    div.active{
    }
    ```

- 伪类选择器

  ```对于a标签的的样式是不能继承的,想要设置样式必须作用于a标签```

  - 爱恨准则 LoVe HAte

    ```css
    /*没有被访问过a标签的样式*/
    a:link {
        color: green;
    }
    /*访问过后a标签的样式*/
    a:visited {
        color: yellow;
    }
    /*鼠标悬浮时a标签的样式*/
    a:hover {
        color: red;
    }		
    /*鼠标摁住的时候a标签的样式*/
    a:active {
        color: blue;
    }
    
    /*注意：在页面中使用a的时候，一定按照顺序Link->visited->hover>active。
    对于hover来说，不仅仅可以应用在a上，也可以应用在其他标签，比如div,p,li等等*/
    ```

- 属性选择器

  ```
  input[type='text']{
      background-color: red;
  }
  input[type='checkbox']{
  
  }
  input[type='submit']{
  
  }
  ```

- 伪元素选择器

  ```python
  p::first-letter{	#p标签内的第一个字(元素)
      color: red;
      font-size: 20px;
      font-weight: bold;
  }
  p::before{			#在p标签第一个位置加@
      content:'@';		
  }
  /*解决浮动布局常用的一种方法*/
  p::after{
      /*通过伪元素添加的内容为行内元素*/
      content:'$';
  }
  ```

2.3 标签嵌套关系

- 块级标签可以嵌套块级标签及行内块
- p标签很特殊,
  - p标签不要包div,也不要包p标签
  - 可以放置a标签,img和表单标签

3.css的盒模型

- width:内容宽度

- height:内容的高度

- padding: 内边距,border到内容的距离

  - 一个值上下左右
  - 两个值:上下,左右
  - 三个值:上,左,右
  - 四个值:上右下左(顺时针)
  - 单独设置:paddingleft,paddingtop...

- border;边框

  ```python
  边框有三要素： 粗细 线性样式 颜色
  border-style: solid dotted dashed double;#实线 点线  虚线 双线
  border-color: black purple red blue;
  border-top: 1px solid #000;
  ```

- margin:外边距

  - body标签默认8px的margin值

4.css的层叠性和继承性

4.1 继承性

- css中有些属性继承父类的属性 : 

  - color继承
  - text-align 继承
  - line-height 继承
  - font_size 继承

  `继承性的存在就是为了节约代码`

4.3层叠性&权重

- 行内样式的权重值为1000

- 权值计算方式

  | 选择方式 | 权重值 |
  | -------- | ------ |
  | id选择   | 100    |
  | 类选择   | 10     |
  | 标签选择 | 1      |

  ```python
  #权重比较；
          1.数选择器数量： id 类 标签  谁大它的优先级越高，如果一样大，后面的会覆盖掉前面的属性
          2.选中的标签的属性优先级用于大于继承来的属性，它们是没有可比性,继承来的属性权重为0;
          3.同是继承来的属性
              3.1 谁描述的近，谁的优先级越高
              3.1 描述的一样近，这个时候才回归到数选择器的数量
          4.!important 优先展示,但是大不过内嵌样式;
  ```

```python
#常用属性
width: ;宽度
height: ;高度
font-size;字体大小
font-width: bold;字体加粗
text_align:center; 水平居中
height:40px;	盒子高度
line-height: 40px;	行高------行高等于盒子的高度时内容垂直居中
border-radius: 4px; 设置圆角4px圆角半径
border:			边框线
border:none;   去除边框线
outline:none;	去除输入框外线
display: inline; (显示方式)块级标签转行内标签
display: block;  (显示方式)行级标签转块级标签
color:  ; 设置颜色
text-decoration:  ;  文本修饰(none无修饰, underline下划线, linethough 删除线
```

### 13.3格式化排版

```<!-- margin 在垂直方向上会出现外边距合并现象，塌陷。以设置的最大的magrin距离为基准-->```

- 常用格式化排版

  - 字体属性

    - 字体样式

    ```python
    #为网页中的文字设置字体为微软雅黑
    body{font-family:'微软雅黑'}
    body{font-family:"Microsoft Yahei"}
    #备选字体
    body{font-family:'Microsoft Yahei','宋体','黑体'}
    ```

    | 属性值  | 字体样式描述(font-style: italic;)                            |
    | ------- | ------------------------------------------------------------ |
    | normal  | 默认的，文本设置为普通字体                                   |
    | italic  | 如果当前字体的斜体版本可用，那么文本设置为斜体版本；如果不可用，那么会利用 oblique 状态来模拟 italics。常用 |
    | oblique | 将文本设置为斜体字体的模拟版本，也就是将普通文本倾斜的样式应用到文本中。 |

    | 属性值  | **字体粗细**描述(font-weight) |
    | ------- | ----------------------------- |
    | normal  | 普通的字体粗细，默认          |
    | bold    | 加粗的字体粗细                |
    | lighter | 比普通字体更细的字体          |
    | bolder  | 比bold更粗的字体              |
    | 100~900 | 400表示normal                 |

  - 文本属性

    - 文本修饰```text-decoration```

      | 属性值       | 描述                               |
      | ------------ | ---------------------------------- |
      | none         | 无文本的修饰                       |
      | underline    | 文本下划线                         |
      | overline     | 文本上划线                         |
      | line-through | 穿过文本的线，~~可以模拟删除线~~。 |

    - 文本缩进```text-indent:32px```

      我们希望整段文章描述，首行空两格，那么首先要知道字体大小是多少。比如字体大小默认是16px，那么我需要给它设置`text-indent:32px;`才能实现效果。

    - **行间距**```line-height```

- line-height:2em; 表示2倍行间距;

- text-indent:2em;(em需要子设置)缩进;

- **中文字间距、字母间距**

  ```
  ​```python
  p{
      /*文字之间的距离*/
      letter-spacing:5px; 
      /*调整英文单词之间的距离*/
      word-spacing: 10px;
  }
  ```

- 文本对齐```text-align```

  ```
  | 属性值 | 描述             |
  | ------ | ---------------- |
  | left   | 文本左对齐，默认  |
  | right  | 文本右对齐       |
  | center | 中心对齐        |
  ```

**- 补充-css中单位em和rem的区别**

```python
#在css中单位长度用的最多的是px、em、rem，这三个的区别是：

1.px是固定的像素，一旦设置了就无法因为适应页面大小而改变。

2.em和rem相对于px更具有灵活性，他们是相对长度单位，意思是长度不是定死了的，更适用于响应式布局。
3.em子元素字体大小的em是相对于父元素字体大小
元素的width/height/padding/margin用em的话是相对于该元素的font-size

#对于em和rem的区别一句话概括：**em相对于父元素，rem相对于根元素。**
rem中的r意思是root（根源），这也就不难理解了。

```



### 13.4浮动

```python
vertical-align: top;  #强制在最顶部显示
```

13.4.1 浮动概念

- 如果想实现网页中排版布局，比如一行内显示对应的标签元素，可以使用浮动属性。浮动可以实现元素并排。

- 块转行内日块也可以实现一行显示,不过存在空白折叠现象

- **float 浮动**

  | 属性值  | 描述                                         |
  | ------- | -------------------------------------------- |
  | none    | 表示不浮动，所有之前讲解的HTML标签默认不浮动 |
  | left    | 左浮动                                       |
  | right   | 右浮动                                       |
  | inherit | 继承父元素的浮动属性                         |

  ```python
  #当一个元素浮动之后，它会被移出正常的文档流，然后向左或者向右平移，一直平移直到碰到了所处的容器的边框，或者碰到另外一个浮动的元素
  ```

- ###### 浮动的现象

  我们之前说浮动的设计初衷是为了做”文字环绕效果“。那么我们就来看一下如果对盒子设置了浮动，会产生什么现象？

  - 浮动的元素脱离了标准文档流，即`脱标`
  - 浮动的元素互相贴靠
  - 浮动的元素会产生”字围“效果
  - 浮动元素有收缩效果

- ###### 标准文档流

  文档流指的是元素排版布局过程中，元素会**默认**自动从左往后，从上往下的流式排列方式。

  即不对页面进行任何布局控制时，浏览器默认的HTML布局方式，这种布局方式从左往右，从上往下，有点像流水的效果，我们称为`流式布局`。

13.4.2 浮动带来的问题

- 浮动的元素**脱离了标准文档流**，即`脱标`
- 浮动的元素**互相贴靠**
- 浮动的元素会产生”**字围**“效果
- 浮动元素有**收缩效果**
  - 当浮动元素没有设置尺寸,会适应浮动元素内的子元素尺寸
- 浮动的元素**不占尺寸**,标准流元素可以撑起父级块的尺寸,浮动元素不可以
- 浮动元素设置margin_top**不会发生塌陷现象**
  - 给标准流元素设置margin-top时会发生margin塌陷

13.4.3 清除浮动问题的方式

- 给浮动元素添加父级块,并设置父级块高度;

  - 不宜维护,不灵活
  - 应用:万年不变导航栏,固定栏;

- 内墙法: 给最后一个浮动元素的后面添加一个空的块级标签,并且设置标签的属性为 clear:both;

  - 冗杂,浪费资源(不推荐)

- **伪元素清楚法(推荐)**

  - 在盒子上末尾加一个占位空的块级标签;

  ```html
  .pa::after{
      content:''
      display: block;
      clear: both;
  }
  ```

- **overflow: hidden;清楚法 (常用)**

  ```python
   # overflow: hidden(超出隐藏) vidible(默认-可见) scroll(滚动显示)
      在父级盒子上设置一个overflow: hidden;
          #注意:hidden(超出隐藏)!!
  ```

13.4.4 BFC

- BFC生成规则

  ```
  1.内部的Box会在垂直方向，一个接一个地放置。
  2.Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
  3.每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
  4.BFC的区域不会与float 元素重叠。
  5.BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
  6.计算BFC的高度时，浮动元素也参与计算
  ```

- 哪些元素会生成BFC

  ```
  1.根元素
  2.float属性不为none
  3.position为absolute或fixed
  4.display为inline-block
  5.overflow不为visible
  ```

### 13.5 定位

**position**属性用于指定一个元素在文档中的定位方式。`top`，`right`，`bottom`，`left`属性则决定了该元素的最终位置。

| 属性值   | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| static   | **默认。静态定位**， 指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 `top`, `right`, `bottom`, `left` 和 `z-index`属性无效。 |
| relative | **相对定位**。 元素先放置在未添加定位时的位置，在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白） |
| absolute | **绝对定位**。不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并 |
| fixed    | **固定定位**。 不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变 |

- **static  静态定位**,“元素默认显示文档流的位置”。没有任何变化。

- **relative 相对定位**: 相对定位的元素是在文档中的正常位置的偏移，但是不会影响其他元素的偏移。

  - **参考点**

    以自身原来的位置进行定位，可以使用`top,left,right,bottom`对元素进行偏移

  - **现象**

    1. 不脱离标准文档流，单独设置盒子相对定位之后，如果不用`top,left,right,bottom`对元素进行偏移，那么与普通的盒子没什么区别。
    2. 有压盖现象。用`top,left,right,bottom`对元素进行偏移之后，明显定位的元素的层级高于没有定位的元素

  - **应用**

    相对定位的盒子，一般用于`子绝父相`布局模式的参考

  - 留了一个"坑",影响页面布局,原来的位置不会被替换

  - **absolute 绝对定位**

    - **参考点**

      相对于最近的非static祖先元素定位，如果没有非static祖先元素，那么以页面左上角进行定位。

      ```
      ##### 如果单独设置一个盒子为绝对定位，
      ​```
      以top描述，它的参考点是以body的（0，0）为参考点
      以bottom描述，它的参考点是以浏览器的左下角为参考点
      ​```
      ##### 如果设置了,子绝父相
      ​```
      以最近的父辈元素的左上角为参考点进行定位
      ​```
      ##### 
      ```

    - **现象**

      - 脱离了标准文档流，不在页面中占位置
      - 层级提高，做网页压盖效果

    - **应用**

      ​	网页中常见的布局方案：`子绝父相`。

    ```
    注意：子绝父绝，子绝父固，都是以最近的非static父辈元素作为参考点。父绝子绝，子绝父固，没有实战意义，布局网站的时候不会出现父绝子绝。
    
    因为绝对定位脱离标准流，影响页面的布局。
    
    相反`父相子绝`在我们页面布局中，是常用的布局方案。因为父亲设置相对定位，不脱离标准流，子元素设置绝对定位，仅仅的是在当前父辈元素内调整该元素的位置。
    ```

- 固定定位

  - 特点:
    - 脱标
    - 固定定位
    - 提高层级,遮盖
  - 参考点:
    - 以页面的显示范围左上角为参考点.

### 13.6 z-index&背景图

**1层级有限设置 z-index**

- 只作用于定位元素上;z-index:auto;

- 取值为整数

  ```
  z-index只应用在定位的元素，默认z-index:auto;
  z-index取值为整数，数值越大，它的层级越高
  如果元素设置了定位，没有设置z-index，那么谁写在最后面的，表示谁的层级越高。(与标签的结构有关系)
  从父现象。通常布局方案我们采用子绝父相，比较的是父元素的z-index值，哪个父元素的z-index值越大，表示子元素的层级越高。
  ```

2.背景图 

```css
/*在盒子设置背景图*/
background-image: url("图片路径")
background-repeat: no- repeat;  
#repeat 平铺,repeat-x x轴平铺,repeat_y y轴平铺
/*调整背景图的位置*/
background-position: -164px -106px;  #(精灵图技术,雪碧图技术)
```

- cursor: pointer
- background: url("图片路径")  no-repeat  top; (综合写法不能单独设值)
- CSS雪碧图技术：即CSS Sprite,也有人叫它CSS精灵图，是一种图像拼合技术。该方法是将多个小图标和背景图像合并到一张图片上，然后利用css的背景定位来显示需要显示的图片部分。来举几个例子。如图

3.圆切割

border-radius

​    - **实现一个无边框圆**

html部分：

```
<div class="circle"></div>
```

css部分：

```
.circle{    width: 200px;    height: 200px;    background-color: #843172;    border-radius: 50%;}
```

4.阴影



语法：

```
box-shadow: h-shadow v-shadow blur color inset;
```

| 值       | 描述                                   |
| -------- | -------------------------------------- |
| h-shadow | 必需。水平阴影的位置。允许负值         |
| v-shadow | 必需。垂直阴影的位置。允许负值。       |
| blur     | 可选。模糊距离。                       |
| *color*  | 可选。阴影的颜色。                     |
| inset    | 可选。将外部阴影 (outset) 改为内部阴影 |

5.居中

- **行内元素水平居中显示**

  行高等于盒子告诉

- **块级元素水平垂直居中**

  - 第一种/*position+margin*/

  ```css
  /*position+margin*/
  子绝父相
  .father{
              width: 200px;
              height: 200px;
              background-color: red;
              position: relative;
          }
          .child{
              position: absolute;
              width: 100px;
              height: 100px;
              background-color: green;
              margin: auto;
              left:0;
              right: 0;
              top: 0;
              bottom: 0;
          }
  
  ```

  - 第二种**纯position**

  ```css
   .father{
              width: 200px;
              height: 200px;
              background-color: red;
              position: relative;
          }
          .child{
              width: 100px;
              height: 100px;
              background-color: green;
              position: absolute;
              left: 50%;
              top: 50%;
              margin-left: -50px;
          }
  ```

6.大背景图设置

- 大文件居中显示属性设置 background: url("图片路径")  no-repeat   center top ;

7.其他

- css命名规范

  参考此链接<http://www.divcss5.com/jiqiao/j4.shtml#no2> 不需要花大量的时间背。

- 项目目录规范

  - 项目名称
    - css 网页中的css文件
    - fonts 网页中的字体图标
    - images 网页中的图片文件
    - js 网页中的脚本文件
    - index.html文件 首页启动文件

### 13.7 javaScript

1. #### js的引入方式

- 内嵌式

- 外接式

  ```js
  //内嵌式
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <script type="text/javascript">
      //编写Javascript代码
  </script>
  </body>
  </html>
  
  //外接式
  1.首先，在刚才的html文件所在的目录下创建一个名为script.js的新文件。请确保文件扩展名为.js,只有这样才能被识别为Javascript代码。
  
  2.将script元素替换为
  <script src="script.js" async></script>
  
  3.在script.js文件中，我们就可以编写Javascript代码
  ```

2. #### 测试语句

- web三层 

  ```1层.html  2层.css  3层.javascript```

- 注释

  - //单行注释
  - /**/多行注释(ctrl+shift+/多行注释)

- **每一句Javascript代码都以;作为结束当前语句。**

- console.log('要打印内容');  --安慰

  - '要打印内容'+.log+table  ==console.log('要打印内容');

- alert('弹出框显示内容');  --alert 警报

- var 声明一个变量

3. #### 数据类型

 **3.1. 基础数据类型**

1. 整形 number
2. 字符串 string
   - 字符串和整形可以拼接,自动将数字转为字符串
   - 如果字符串内部既包含`'`又包含`""`怎么办？可以用转义字符`\`来标识
3. 布尔类型 Boolean
4. undefined 未定义的
5. **null 空对象**

**3.2 引用数据类型**

1. Array 数组--(py里的列表)

2. Object 对象--(跟py字典类似)

   - 定义在对象中的函数叫做对象的方法

3. function 函数 

   - js中有两个作用域,全局作用域和函数作用域

   

```js
   //函数定义基本格式
   function add(参数){
       函数作用域
   }
   add(参数)
   
   //作用域是非常重要的一个概念。当你创建函数时，函数内定义的变量和其它东西都在它们自己的单独的范围内，意味着它们被锁在自己独立的房间内。
```

- 函数表达式

  var add2 = function(){   } 

- 自执行函数

  - function(形参){函数作用域})(实参)

4. ### 方法

1. **整型**number
   - a++  就是a+=1  即 x = x + 1;
   - b=a++ 先把a赋值给b,再对a进行+1
   - b=++a 先计算a+1再把结果复制给b

------

2. **string字符串**

- 拼接

  - +号拼接类似py

  - join拼接类似py

  - 反引号``+${}进行拼接(格式化)

    ```
    var name='liu' , age=18;
            var a=`${name}今年${age}岁了`;
            console.log(a);
    ```

- 字符方法 

  - .charAt() 以单字符字符串的形式返回给定位置的那个字符

- 字符串操作方法

  - concat() 用于将一或多个字符串拼接起来， 7 返回拼接得到的新字符串。

    ```
    var stringValue = "hello ";
    var result = stringValue.concat("world"); alert(result); //"hello world" alert(stringValue); //"hello"
    ```

  - **slice** 切片，有一个参数，从当前位置切到最后，两个参数，顾头不顾尾

  - **substring** 

  - **substr**  切片，有一个参数，从当前位置切到最后，两个参数，第二个参数表示取值的个数

- 位置方法(py索引取值)

  - indexof() / lastindexof()

    - **indexOf()方法从字符串的开头(位置 0)开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找,**

    - 返回要查找的那项在字符串中的索引，或者在没找到的情况下返回-1

      ```js
      var stringValue = "hello world";
      alert(stringValue.indexOf("o"));             //4
      alert(stringValue.lastIndexOf("o"));         //7
      alert(stringValue.indexOf("o", 6));         //7
      alert(stringValue.lastIndexOf("o", 6));     //4
      ```

- trim方法--(py里的strip),删除字符串的前后空格.

- **大小写转换**

  - `toLowerCase()`和 `toUpperCase()`与py中upper,lower使用相同

------

3. **数组**Array

- **for循环遍历**

- Array.isArray( )  检测数组

- . tostring() 转换方法

  - 会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。

- 分割字符串

- 栈方法-push-pop

  - **.push()方法**可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。
  - **.pop()方法**从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项 。

- 队列方法shift-unshift

  - 栈数据结构的访问规则是 `LIFO(后进先出)`，而队列数据结构的访问规则是 `FIFO(First-In-First-Out， 先进先出)`。

  - **.shift()**  移除数组中的第一个项并返回该项

  - **`.unshift()`与`.shift()**`的用途相反: 它能在数组前端添加任意个项并返回新数组的长度

     

- **splice方法**

  1. **删除**：可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的个数。例如`splice(0,2)`会删除数组中的前两项

  2. **插入**：可以向指定位置插入任意数量的项，只需提供3个参数：**起始位置**、**0（要删除的个数）**和**要插入的项**。如果要插入多个项，可以再传入第四、第五、以至任意多个项。例如，`splice(2,0,'red','green')`会从当前数组的位置2开始插入字符串`'red'`和`'green'`。

  3. **替换**：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数:**起始位置**、**要删除的项数**和**要插入的任意数量的项**。插入的项数不必与删除的项数相等。例如，`splice (2,1,"red","green")`会删除当前数组位置 2 的项，然后再从位置 2 开始插入字符串`"red"`和`"green"`。

     ```
     splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项(如果没有删除任何 项，则返回一个空数组)。
     ```

     

- **slice 切片**

  - 一个参数的情况下，slice()方法会返回从该参数指定位置开始到当前数组默认的所有项

  - 两个参数的情况下，该方法返回起始和结束位置之间的项—---顾头不顾尾。

    ```js
    var colors = ['red','blue','green','yellow','purple'];
    colors.slice(1);//["blue", "green", "yellow", "purple"]
    colors.slice(1,4);// ["blue", "green", "yellow"]
    ```

- 重排序

  - reverse翻转数组项的顺序
  - `sort()`方法按升序排列——即最小的值位于最前面，最大的值排在最后面

- concat() 数组合并方法

  - 参数为一个或多个数组，则该方法会将这些数组中每一项都添加到结果数组中。
  - 参数不是数组，这些值就会被简单地添加到结果数组的末尾

  ```js
  var colors = ['red','blue','green'];
  colors.concat('yello');//["red", "blue", "green", "yello"]
  ```

- 位置方法(py索引取值)

  - indexof()/lastindexof()
    - **indexOf()方法从数组的开头(位置 0)开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找,**
    - 返回要查找的那项在数组中的位置，或者在没找到的情况下返回-1

- 迭代方法 foreach

  - li , arguments不是数组,伪数组,但是有索引,能遍历  

    ```js
    var numbers = [1,2,3,4,5,4,3,2,1];
    numbers.forEach(function(item, index, array){
    });
    
    //item是数组元素,index是数组索引, array是数组
    
    ```

- date 内的方法 --toLocaleString

  ```document.write('在网页中要显示内容')```

- 字符串和数值之间的转换

  - 字符串转数值

    - `console.log(parseInt(str));`
    - `console.log(parseFloat(str));`

  - 数值转字符串

    ```js
    var num  = 1233.006;
    // 强制类型转换
    console.log(String(num));
    console.log(num.toString());
    // 隐式转换
    console.log(''.concat(num));
    // toFixed()方法会按照指定的小数位返回数值的字符串 四舍五入
    console.log(num.toFixed(2));
    
    ```

**5.流程控制(条件判断与循环)**

1. for循环

   ```js
   1.初始化循环变量;
   2.循环条件;
   3.更新循环变量;
   
   for(var i=0;i<5;i++){
               console.log(i);
           }
   
   ```

2. while循环

   ```js
   初始化条件
   while(判断循环结束条件){
       //code to run
       递增条件
   }
   
   ```

   

3. 条件判断

   - if else

     ```js
     var shoppingDone = false;
     if (shoppingDone === true) {
       var childsAllowance = 10;
     } else {
       var childsAllowance = 5;
     }
     
     ```

   - switch  case

     ```js
     var weather = 'sunny';
     switch(weather){
         case 'sunny':
             //天气非常棒，可以出去玩耍了
             break;							//必须添加break
         case 'rainy':
            //天气下雨了，只能在家里呆着
             break;
         case 'snowing':
             //天气下雪了，可以出去滑雪了
             break;
         default:
             //哪里也不出去
     }
     
     ```

     

4. 逻辑运算

   - ! ---not
   - ||---or
   - &&---and

5. ==,===

   - `===` 和 `!==` ——判断一个值是否严格等于，或不等于另一个。(判断内存地址)
   - `==`和`!=`—— 判断一个值是否等于，或不等于另一个(判断数值)
   - `<` 和 `>` ——判断一个值是否小于，或大于另一个。
   - `<=` 和 `>=` ——判断一个值是否小于或等于，或者大于或等于另一个。

6. 三元运算
   1 > 3 ? '真的' : '假的';

**6.函数**

```js
function sum(a,b){
    return a+b
}
var result = sum(4,5);
//通过在函数体中return关键字 将计算的结果返回，调用函数之后你会得到该返回值，并用变量result接收。

```

- 处理函数时，**作用域**是非常重要的一个概念。当你创建函数时，函数内定义的变量和其它东西都在它们自己的单独的范围内，意味着它们被锁在自己独立的房间内。

**7.对象**

- **对象的属性**：它是属于这个对象的某个变量。比如字符串的长度、数组的长度和索引、图像的宽高等
- **对象的方法**：只有某个特定属性才能调用的函数。表单的提交、时间的获取等。

7.1 对象的创建

1. 字面量创建方式

   ```js
   var person = {
       name : 'jack';
       age : 28,
       fav : function(){
           console.log('泡妹子');
       }
   }
   
   ```

   - 点语法访问person对象中的age属性和fav**方法**

     ```js
     person.name; //jack
     person.fav();//泡妹子
     
     ```

   - 括号表示法

     另外一种访问属性的方式使用括号表示法，代替这样的代码

     ```
     person.name; //jack
     
     ```

     使用如下所示的代码

     ```
     person['name'];
     
     ```

2. 构造函数

   ```js
   //创建对象
   var person = new Object();
   //给对象添加name和age属性
   person.name = 'jack';
   person.age = 28;
   //给对象添加fav的方法
   person.fav = function(){
       console.log('泡妹子');
   }
   
   ```

8.其他

- ##### 日期对象(data)

  ![1559640434002](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1559640434002.png)

- ##### setInterval() 定时器函数

- ##### globle对象 全局对象 window对象

- var a = NaN
  isNaN(a)

  Infinity 无限大的

- ##### Math 方法

  - .max() /.min()min()和 max()方法用于确定一组数值中的最小值和最大值

    ```
    var max = Math.max(3, 54, 32, 16);
    alert(max);    //54
    var min = Math.min(3, 54, 32, 16);
    alert(min);    //3
    
    ```

  - call(obj,实参) 改变this指向

  - apply(obj,数组) 改变this指向

    ```js
    var values = [1,2,36,23,43,3,41];
    var max = Math.max.apply(null, values);
    //取到了最大值
    
    ```

    把 Math 对象作为 apply()的第一个参数，从而正确地设置 this 值。然后，可以将任何数组作为第二个参数。

  - 舍入方法

    - `Math.ceil()`--天花板--执行**向上舍入**，即它总是将数值向上舍入为最接近的整数;
    - `Math.floor()`-- 地板--执行**向下舍入**，即它总是将数值向下舍入为最接近的整数;
    - `Math.round()`  -- **四舍五入**-- 执行标准舍入，即它总是将数值四舍五入为最接近的整数(这也是我们在数学课上学到的舍入规则)。

  - 随机数 random

    - `Math.random()`方法返回大于等于 0 小于 1 的一个随机数

      - **获取min到max的范围的整数**

        ```js
        function random(lower, upper) {    return Math.floor(Math.random() * (upper - lower)) + lower;}
        
        ```

      - **获取随机颜色**

        ```js
        /** 
        * 产生一个随机的rgb颜色 
        * @return {String}  
        返回颜色rgb值字符串内容，如：rgb(201, 57, 96) */
        function randomColor() {    
            // 随机生成 rgb 值，每个颜色值在 0 - 255 之间    
            var r = random(0, 256),       
                g = random(0, 256),       
                b = random(0, 256);    
            // 连接字符串的结果    
            var result = "rgb("+ r +","+ g +","+ b +")";    
            // 返回结果    
            return result;
        }
        
        ```

      - 获取随机验证码

        ```js
        function createCode(){
            //首先默认code为空字符串
            var code = '';
            //设置长度，这里看需求，我这里设置了4
            var codeLength = 4;
            //设置随机字符
            var random = new Array(0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R', 'S','T','U','V','W','X','Y','Z');
            //循环codeLength 我设置的4就是循环4次
            for(var i = 0; i < codeLength; i++){
                //设置随机数范围,这设置为0 ~ 36
                var index = Math.floor(Math.random()*36);
                //字符串拼接 将每次随机的字符 进行拼接
                code += random[index]; 
            }
            //将拼接好的字符串赋值给展示的Value
            return code
        }
        
        ```

        

#### 13.7.BOM

BOM 

- BOM 的核心对象是 window
- 在浏览器中，window 对象有双重角色， 它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象

1. **系统对话框方法**
   - alert('警告框')  

- confirm('你确定要离开网站?')  确认框

  ```js
  var a = window.confirm('你确定要离开网站?');
  console.log(a);
  
  //如果点击确定,a的值返回true,点击取消,a的值返回false
  ```

  - prompt('弹出框','弹出框内默认显示内容') 

    ```js
    var name = window.prompt('请输入你早晨吃了什么?','mjj');
    console.log(name);
    
    //prompt()方法接收两个参数,第一个参数是显示的文本,第二个参数是默认的文本,如果点击了确定,则name的结果为mjj,点击取消,name为null
    ```

2. **定时方法()**
   - 一次性任务 setTimeout()
     - 表示一次性定时任务做某件事情,它接收两个参数,第一个参数为执行的函数,第二个参数为时间(毫秒计时:1000毫秒==1秒)

- 异步非阻塞
- 周期循环 setInterval()
  - 它接收的参数跟`setTimeout()`方法一样.
  - 清除定时器clearInterval()

3. reload()

#### 13.7.DOM

DOM  ***

- DOM是”**Document Object Model**“(文档对象模型)的首字母缩写
- 像`<body>`、`<p>`、`<ul>`之类的元素这些我们都称为叫元素节点

1. 获取节点对象三种方式

   - 通过id获取单个节点对象

     - 返回一个与那个给定id属性值的元素节点相对应的对象。

     - ##### getElementById()方法

       ```js
       console.log(typeof oUl)
       console.dir(typeof oUl)  //打印所有信息
       ```

   - 通过标签名来获取节点对象

     - ##### getElementsByTagName()方法

     - 这个方法将返回一个**元素对象集合**。

   - 通过类名来获取节点对象

     - ##### getElementsByClassName()方法

     - 这个方法将返回一个**元素对象集合**。

2. 事件

   - onclick 点击事件
   - onmouseover()  悬浮事件
   - onmouseout()  离开事件

3. 对样式操作

   ```js
   <script type="text/javascript">
   			// 1.获取事件源对象
   			var box = document.getElementById('box');
   			
   			// 2.绑定事件
   			box.onmouseover = function (){
   			// 3.让标签的背景色变绿
   				
   			box.style.backgroundColor = 'green';
   			box.style.fontSize = '30px';		
   			}
   			box.onmouseout = function (){
   			// 鼠标离开
   			box.style.backgroundColor = 'red';
   			box.style.fontSize = '16px';
   
   			} 
   		</script>
   ```

   ```js
   var isRed = true;
   			box.onclick = function(){
   				if(isRed){
   					this.style.backgroundColor = 'green';
   					isRed = false;
   				}else{
   					this.style.backgroundColor = 'red';
   					isRed = true;
   				}
   ```

4. 对属性设置

   - getAttribute()  方法只接收一个参数——你打算查询的属性的名字。

   - setAttrbute()  方法传递两个参数。第一个参数为**属性名**，第二个参数为**属性值**

     ```js
     var classList = document.getElementById('classList');
     classList.setAttribute('title','这是我们最新的课程');
     ```

   - p.className='newname' ; 修改类名

   - p.classtitle='newtitle';修改标题名称

   - 自定义属性必须使用setAttrbute()方法

   - `firstChild`属性返回`childNodes`数组中的第一个子节点,如果选定的节点没有子节点，则该属性返回NULL。

   - `lastChild`属性返回`childNodes`数组中的最后一个子节点。如果选定的节点没有子节点，则该属性返回NULL。

   - parentNode属性,节点只能有一个

5. 节点创建

   - 创建节点 createElement('元素名');

   - innerText = '<p></p>';设置节点内容(标签不生效,只设置文本)

   - innerHTML = '<p></p>';标签生效,技能设置文本,又能渲染标签

   - values=''表单值设置

     ```js
     // 创建节点
     			var li1 = document.createElement('li');
     			var li2 = document.createElement('li');
     			// innerText 只设置文本
     			li1.innerText  = '<a href="#">123</a>';
     			li1.innerHTML = '<a href="#">123</a>';
      //注意：如果想获取节点对象的文本内容，可以直接调用innerText或者innerHTML属性来获取
     ```

6. 插入节点

   - appendChild( new ) 插入节点--在指定的节点的最后一个子节点之后添加一个新的子节点

   - `insertBefore(new,node)`方法可在已有的子节点前插入一个新的子节点

     - new:要插入的新节点

       node:指定此节点前插入节点

7. 删除节点

   - removeChild()方法从子节点列表中删除某个节点。如果删除成功，此方法可返回被删除的节点，如失败，则返回NULL

8. 替换节点

   - replaceChild实现子节点(对象)的替换。返回被替换对象的引用



### 13.8 jQuery

```js
//补充：获取文档,body和html
console.log(document);  //获取文档
console.log(document.body);  //获取body
console.log(document.documentElement);  //获取html
```

#### 1.jQuery 初识

- 定义:快速,小巧,功能丰富的JavaScript库
- 通过易用的API在浏览器中运行使得HTMl文档遍历/操作/事件处理/动画/Ajax(局部刷新)变得更加简单
- 操作
  - 获取节点元素对象/属性操作/样式操作/类名/节点创建/删除/增加/替换
- 核心:write less,do more;

#### 2.jQuery使用

```button```按钮,行级标签

```js
//在jQuery中
if ( !noGlobal){
    window.jQuery = window.$ = jQuery;
}
```

2.1 导入jQuery模块

```js
<script src="js/jquery.js" type="text/javascript" charset="utf-8"></script>
```

2.2 jquery的选择器

```jquery获取系欸但元素对象,直接在$()括号内写入css的选择器样式即可```

- 基础选择器(jq与js对象转换)

  ```js
  // 选择器 基础选择器
  console.log($('.box'));//jquery对象 伪数组
  // jquery对象转换js节点对象
  console.log($('#box')[0]);
  // js节点对象转换jq对象
  var box = document.getElementById('box');
  $(box);
  ```

- 高级选择器

- 属性选择器

- 基本过滤选择器

  - :eq() 选择一个 索引从0开始

    ```js
    console.log($('ul li:eq(1)'));
    ```

  - **:first 获取第一个**

  - **:last 获取最后一个**

    ```js
    console.log($('ul li:first'));
    console.log($('ul li:last'));
    ```

  - :odd  获取奇数

  - :even 获取偶数

    - **过滤的方法**

    - .eq() 选择一个 索引从0开始

    - **.children() 获取亲儿子**

      ```js
      // 只选择亲儿子
      console.log($('ul').children());
      ```

    - **.find() 获取的后代**

      ```js
      // 即选择儿子又选择孙子....
      console.log($('ul').find('li a'));
      ```

    - .parent() 获取父级对象

    - **.siblings() 获取除它之外的兄弟元素**

      ```js
      //js选项卡小功能实例----let声明的变量只在局部作用域有效
      
      for(let i = 0;i < btns.length; i++){	
          btns[i].onclick = function (){
              for(var j = 0; j < btns.length; j++){
                  btns[j].className = '';
                  ops[j].className = '';
              }
              //改变button的样式
              this.className = 'active';
              //改变p标签的样式
              ops[i].className = 'active';
          }
      }
      ----------------------------------------------
      // jquery实现选项卡
      
      $('button').click(function(){
          // 链式编程
          //第二个按钮 索引1
          console.log($(this).addClass('active'));
          $(this).addClass('active').siblings('button').removeClass('active');
          // 获取当前点击的元素的索引
          console.log($(this).index());
          $('p').eq($(this).index()).addClass('active').siblings('p').removeClass('active');
      })
      ```

2.3 jqurey修改样式

- 在jq中,this指向当前的js节点对象

- $(this)  this指向修改为jq对象

- 通过调用.css()方法
  如果传入一个参数，看一下这个参数如果是一个字符串表示获取值，如果是对象，表示设置多少属性值，如果是两个参数，设置单个属性值

  ```js
  $('#box .active').click(function(){
  				// 样式操作
  				console.log(this);
  				// this.style
  				// 设置单个样式属性
  				$(this).css('color','red');
  				$(this).css('fontSize','20px');
  				// 设置多个属性
  				 $(this).css({
  					 'color':'red',
  					 "font-size":40
  				 })
  				 console.log($(this).css('color'));
  			})
  ```

#### 3.动画

动画参考博客<https://www.cnblogs.com/majj/p/9113627.html>

3.1普通动画

- show() 显示动画()内写入过程时间

  ```js
   //show(毫秒值，回调函数;
      $("div").show(5000,function () {
          alert("动画执行完毕！");
      });
  ```

- hide()  隐藏动画  $(selector).hide(1000, function(){});

- toggle()  切换,开关.(可以用作动画显示)  $('#box').toggle(3000,function(){});

3.2 卷帘门动画

- slideDown()  **滑入动画效果**：（类似于生活中的卷帘门）

  ```js
  $(selector).slideDown(speed, 回调函数);
  ```

- slideUp()  **滑出隐藏动画效果：** 

  ```js
   $(selector).slideUp(speed, 回调函数);
  ```

- slideToggle()  **滑入滑出切换动画效果：**

  ```js
   $(selector).slideToggle(speed, 回调函数);
  ```

3.3 淡入淡出效果

- fadeIn()  淡入动画效果：

  ```js
   $(selector).fadeIn(speed, callback);
  ```

- fadeOut() 淡出动画效果：

  ```
  $(selector).fadeOut(1000);
  ```

- fadeToggle()  淡入淡出切换动画效果：

  ```js
   $(selector).fadeToggle('fast', callback);
  ```

3.4 自定义动画

- .animate({params},speed,callback)

  作用：执行一组CSS属性的自定义动画。

  - 第一个参数表示：要执行动画的CSS属性（必选）

  - 第二个参数表示：执行动画时长（可选）

  - 第三个参数表示：动画执行完后，立即执行的回调函数（可选）

    ```js
    <html>
    <head lang="en">
        <meta charset="UTF-8">
        <title></title>
        <style>
            div {
                position: absolute;
                left: 20px;
                top: 30px;
                width: 100px;
                height: 100px;
                background-color: green;
            }
        </style>
        <script src="jquery-3.3.1.js"></script>
        <script>
            jQuery(function () {
                $("button").click(function () {
                    var json = {"width": 500, "height": 500, "left": 300, "top": 300, "border-radius": 100};
                    var json2 = {
                        "width": 100,
                        "height": 100,
                        "left": 100,
                        "top": 100,
                        "border-radius": 100,
                        "background-color": "red"
                    };
    
                    //自定义动画
                    $("div").animate(json, 1000, function () {
                        $("div").animate(json2, 1000, function () {
                            alert("动画执行完毕！");
                        });
                    });
    
                })
            })
        </script>
    </head>
    <body>
    <button>自定义动画</button>
    <div></div>
    </body>
    </html>
    ```

### 13.9jquery 属性/文档操作

- 文档加载

```js
//用jq时//文档加载完成之后 调用回调函数中代码;事件无覆盖现象;
$(function(){	
    })
// 在js中
window.onload = function(){
} //事件有覆盖现象;
```

#### 1.操作属性

- attr()  一个参数获取属性对象的值,两个参数属性赋值;

- removeAttr() 移除属性

  ```js
  //添加一个或者多个属性
      $('p').attr('title','mjj');
      $('p').attr({'a':1,'b':2});
      console.log($('p').attr('id'));
  
  //移除
      $('p').removeAttr('title id a');
  ```

- *prop/removeprop 与attr类似,但是仅用于input获取内部属性*

  ```js
  attr只能设置标签内的属性值,prop可以设置对象内部的属性
  ```

- audio标签MP3标签

  ```
  <audio src="static/海贼王%20-%20ビンクスの酒(独唱).mp3" controls="">
  ```

#### 2.操作文档

- 插入

  - **父元素.append('要插入元素')**

    - 追加某元素，在父元素中添加新的子元素。子元素可以为：stirng | element（js对象） | jquery元素

    - 如果追加的是jquery对象那么这些元素将从原位置上消失。简言之，就是一个移动操作。

      ```
      var oli = document.createElement('li');
      oli.innerHTML = '哈哈哈';
      $('ul').append('<li>1233</li>');
      $('ul').append(oli);
      $('ul').append($('#app'));
      ```

  - 子元素.prependTo(父元素)；

    - 解释：前置添加， 添加到父元素的第一个位置

      ```javascript
       $('<a href="#">路飞学诚</a>').prependTo('ul')
      ```

  - 父元素.prepend(子元素)；

    - 解释：前置添加， 添加到父元素的第一个位置

      ```js
      $('ul').prepend('<li>我是第一个</li>')
      ```

  - 子元素.prependTo(父元素)；

    - 解释：前置添加， 添加到父元素的第一个位置

      ```js
       $('<a href="#">路飞学诚</a>').prependTo('ul')
      ```

  - 兄弟元素.before(要插入的兄弟元素);

  - 要插入的兄弟元素.inserBefore(兄弟元素)；

    - 在匹配的元素之后插入内容 

    ```
    $('ul').before('<h3>我是一个h3标题</h3>')
    $('<h2>我是一个h2标题</h2>').insertBefore('ul')
    ```

- 修改

  - $(要替换的).replaceWith(新内容);

    将所有匹配的元素替换成指定的string、js对象、jquery对象。

    ```js
    //将所有的h5标题替换为a标签
    $('h5').replaceWith('<a href="#">hello world</a>')
    //将所有h5标题标签替换成id为app的dom元素
    $('h5').replaceWith($('#app'));
    ```

  - ## $('<p>哈哈哈</p>')replaceAll('h2');

    - 替换所有。将所有的h2标签替换成p标签。

    ```
    $('<br/><hr/><button>按钮</button>').replaceAll('h4')
    ```

- 删除

  - $(selector).replaceWith(content);

    - 删除节点后，事件也会删除（简言之，删除了整个标签）

      ```js
      $('ul').remove();
      ```

  - $(selector).detach(); 

    - 删除节点后，事件会保留

      ```javascript
       var $btn = $('button').detach()
       //此时按钮能追加到ul中
       $('ul').append($btn)
      ```

  - $(selector).empty(); 

    - //清空掉ul中的子元素，保留ul
      $('ul').empty()

#### 3.事件

*参考jq思维导图*

3.1鼠标事件

3.2表单事件from 

- javascript:; 阻止默认事件

  preventDefinit(); 阻止默认事件

- submit  提交事件

#### 4.ajax(了解)

- 局部刷新

```js
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="box">
			
		</div>
		<script src="js/jquery.js" type="text/javascript" charset="utf-8"></script>
		<script type="text/javascript">
			$(function(){
				// 获取首页的数据
				$.ajax({
					url:'https://api.apeland.cn/api/banner/',
					methods:'get',
					success:function(res){
						console.log(res);
						if(res.code === 0){
							var cover = res.data[0].cover;
							var name = res.data[0].name;
							console.log(cover);
							$('#box').append(`<img src=${cover} alt=${name}>`)
						}
					},
					error:function(err){
						console.log(err);
					}
				})
			})
		</script>
	</body>
</html>

```



### 13.10jQuery插件bootstrap框架

- cmd  tree 显示结构命令

#### 1.jq22插件的使用

- ```**https://www.jq22.com``` 插件库地址**
- **jq库免费手扒,导入(非下载)**
  - 检查元素-->sources-->在pycharm创建文件夹(名字建议一致)-->创建文件-->拷贝代码

#### 2.bootstrap框架(重点)

#### 2.1基本使用

- bootstrap 可视化库
- chart 图标库
- 不要修改固有的类名样式,尽可能自己添加类名去操作样式
- **bootstrap导入**
  1. 下载bootstrap3.3.7中文生产文档
  2. 解压到工作环境目录
  3. 同目录下新建html文件index
  4. 拷贝<https://v3.bootcss.com/getting-started/#download> 起步-->基本模板--代码到新建的index.html文件中
  5. 修改link标签中bootstrap引用本地源

#### 2.2 全局的csss

- 参考地址:<https://v3.bootcss.com/css/>
- 重点内容:
  - **栅格/排版/代码/表格/表单/按钮/图片/辅助类**

```
.container 固定宽度容器
.container-fluid 100%宽度的容器
 栅格系统
.row
.col-lg- .col-md- .col-sm- .col-xs

文本颜色
text-muted
text-primary
text-success
text-danger
text-waring
text-info

背景颜色
bg-primary
bg-success
....

按钮
btn btn-default
btn btn-link
btn btn-success
btn btn-primary
....

对齐
.text-left
.text-right
.text-center
.text-justify 两端对齐 适应于英文

图片设置
.img-rounded
.img-circle
.img-thumbnail 

三角符号
.caret
关闭按钮
<button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
显示和隐藏内容
show/hidden

快速浮动
.pull-left 左浮动
.pull-right 右浮动
清除浮动
.clearfix
内容块居中
.center-block


表格
给table添加.table的类。默认给表格赋予少量的内补和边框
.table-striped 条纹状的
.table-bordered 带边框
.table-hover 状态类

表单
form
每组表单控件都会添加一个.form-group类中，表单控件通常都由.form-control
```

#### 2.3 组件

- 参考地址<https://v3.bootcss.com/components/>
- 重点内容
  - Glyphicons 字体图标
  - 下拉菜单
  - 按钮组
  - 按钮式下拉菜单
  - 输入框组
  - 导航
  - 导航条
  - 路径导航
  - 分页
  - 标签
  - 徽章
  - 巨幕
  - 页头
  - 缩略图
  - 警告框
  - 进度条
  - 媒体对象
  - 列表组
  - 面板

#### 2.4 bootstrap插件

- 参考地址<https://v3.bootcss.com/javascript/>
- 重点内容
  - 模态框
  - 下拉菜单
  - 滚动监听
  - 标签页
  - 弹出框/警告框
  - collapse 折叠
  - carousel

#### 3.后台管理页面模板

- lte地址<http://adminlte.la998.com/>

- 使用方法

  1. http://adminlte.la998.com/documentation/index.html>下载准备版文档

  2. 解压到工作目录

  3. atarter是模板文件

  4. 根据需求更改样式增加内容

     

### 13-11 问题汇总:

- display:none; 不显示
- cursor:pointer; 悬停鼠标箭头转小手
- vertical-align: top;  #强制在最顶部显示

1.字体标签包含有哪些？
h1~h6 ， b , strong , i , em
2.超链接标签a标签中的href属性有什么用？

- 链接到一个新的地址
- 回到顶部
- 跳转邮箱
- 下载文件

3.img中标签中src和alt属性有什么用？

- 链接图片的资源
- 图片失败的时候显示的标题

4.如何创建一个简易的有边框线的表格？

```html
<table border='1' cellspacing=0>
	<th>	
		<td>id</td>
		<td>name</td>
	</th>
	<tr>
		<td>1</td>
		<td>mjj</td>
	</tr>
</table>
```

5.form标签中的action属性和method属性的作用？

- action:提交到服务器的地址，如果是空的字符串，它表示当前服务器地址
- method:提交的方式。get和post
  - get:明文不安全，地址栏只允许提交2kb的内容，提交的内容会在地址上显示 。显示的方式http://127.0.01:8080/index.html?name=value&name2=value2
  - post:密文提交安全，可以提交任意内容
  - 后期要把HTTP协议的内容（重点）

6.在form标签中表单控件input中type类型有哪些？并分别说明他们代指的含义

- type

  - text 单行文本输入框
  - password 密码输入框
  - radio 单选框
    - 产生互斥效果，给每个单选按钮设置相同的name属性值
    - 如何默认选中，给单选按钮添加checked属性
  - checkbox 多选框
    - 默认选中添加checked属性
  - submit 提交按钮
  - file 上传文件
  - datetime

- 下拉列表

  - multiple 产生多选的效果

    ```html
    <select>
        <option name value selected >抽烟</option>
        <option name value selected >喝酒</option>
        <option name value selected >烫头</option>
    </select>
    ```

- 多行文本输入框 textarea

  - cols 列
  - rows 行

7.表单控件中的name属性和value属性有什么意义？

```
name属性值：提交到当前服务器的名称
value属性值：提交
以后给服务器使用
```

1.伪类选择器

```css
a:link{} 没有被访问过时a标签的样式
a:visited{} 访问过后的
a:hover{} 悬浮时
a:active{} 摁住的时候
```

2.如何在p标签的后面添加''&'内容？

```html
<style>
    p::after{
        /*行内元素*/
        content:'&',
        color:red;
        font-size: 20px;    }
</style>
<p>wusir</p>
```

3.设置网页字体使用哪个属性，备选字体如何写？

```css
font-family:'宋体','楷体';
```

4.如何设置文字间距和英文单词间距？

```css
文字间距:letter-spacing
英文单词间距:word-spacing
```

5.字体加粗使用哪个属性，它的取值有哪些？

```css
font-weight:lighter| normal | bold |bolder| 100~900 字体加粗
font-style:italic;/*斜体*/
```

6.文本修饰用哪个属性，它的取值有哪些？

```css
text-decoration:none| underline | overline | line-through
```

7.分别说明px，em,rem单位的意思

```
px: 绝对单位  固定不变的尺寸
em和rem :相对单位     font-size
	em:相对于当前的盒子
	rem:相对于根元素（html）
```

8.如何设置首行缩进，一般使用什么单位？

```
text-indent:2em;(字体大小的2em)
```

9.文本水平排列方式是哪个属性，它的取值有？

```css
text-align:left | center | right | justify(仅限于英文，两端对齐)
```

10.如何让一个盒子水平居中？

```css
盒子必须有宽度和高度，再设置margin: 0 auto;
让文本水平居中： text-align:center;
让文本垂直居中：line-height = height
```

11.margin在垂直方向上会出现什么现象？

```css
外边距合并，“塌陷”
尽量避免出现塌陷问题，只要设置一个方向的margin
```

**1.浮动有哪些现象？**

```
1.脱离标准文档刘
2.贴边
3.收缩
4.文字环绕
```

**浮动带来问题**：不去计算浮动元素的高度，导致撑不起父盒子的高度

**2.清除浮动的方式？**

```
1.给父盒子添加固定高度
2.内墙法：给最后一个浮动元素添加一个空的块级标签，设置该标签的属性为clear:both;
3.伪元素清除
 给父元素添加一个类
 .clearfix::after{
 	content:'',
 	display:block;
 	clear:both
 }
4.overflow:hidden; BFC区域
```

**3.overflow:hidden和overflow:scroll属性的作用？**

```
overflow:hidden;超出部分隐藏
overflow:scroll;出现滚动条

清除浮动
```

**4.定位有哪几种？**

```
position: static | relative | absolute | fixed
```

**5.相对定位的元素有哪些特征？它的参考点是谁？**

```
1.给一个标准文档流下的盒子单纯的设置相对定位，与普通的盒子没有任何区别
2.top|bottom|left|right

参考点：以原来的位置为参考点
应用：1.微调元素 2.做“子绝父相”
```

**6.绝对定位的元素由哪些特征？它的参考点？**

```
现象：
    1.脱标
    2.压盖现象
参考点：
是否有定位（相对定位，绝对定位，固定定位）的祖先盒子进行定位，如果没有定位的祖先盒子，以body为参考点

重要： “子绝父相”
```

**7.阐述一下，“子绝父相”布局方案的好处**

要浮动一起浮动，有浮动清除浮动，浮动带来的好处：实现元素并排



1.列出至少5个以上数组的常用方法，并说明它们的含义
	2.列出数学对象Math常用的三个方法，并说明它们的含义
		Math.ceil() 向上取整
		Math.floor() 向下取整
		Math.random()
		Math.round()
	3.函数对象中，可以通过哪两个方法改变函数内部this的指向？
		function fn(){
			console.log(this);//this指向了window
		}
		fn.call(obj);
		fn.apply(obj)
	4.javascript的基本数据类型和引用数据类型有哪些？
		基本数据类型：number,string,boolean,undefined,null
		引用数据类型：Array,Object,Function,Date
	5.对DOM的理解
		D:document 文档
		O:object 对象
		M:model 模型
	6.获取节点对象的三种方式
	  var b = document.getElementById()
	  document.getElementsByTagName()
	  var a = document.getElementsByClassName('active')
	  b.setAttribute();
	7.如何设置节点对象的样式，属性，类？
		设置样式
		obj.style
		设置属性
		obj.setAttribute(name,value);
		obj.getAttribute(name);
		obj.className
		obj.title
	8.节点对象的创建，添加，删除分别用什么方法？
	  var op =  document.createElement('p');
	  box.appendChild(op);
	  box.insertBefore(newNode,oldNode)
	  box.removeChild(op);
	9.列出你所知道的js中的事件？
	 onclick
	 onmouseover
	 onmouseout
	 

	 onchange
	 onselect
	 onsubmit
	 onload
	10.定时方法有哪两个？写出对应的方法，并说明它们的含义
	setTimeout(callback,毫秒) 一次性任务，延迟操作，异步
	setInterval(callback,毫秒) 周期性循环任务  动画 css  transtion tranform
	
	11.设置值的操作
	   innerText 只设置文本
	   innerHTML 即设置文本，又渲染标签
	   
	   针对与表单控件
	   inputText.value = '123';
1.jquery对象和js节点对象之间的转换;

```js
// 选择器 基础选择器
console.log($('.box'));//jquery对象 伪数组
// jquery对象转换js节点对象
console.log($('#box')[0]);
// js节点对象转换jq对象
var box = document.getElementById('box');
$(box);
```

2.jquery 的基础选择器和高级选择器有哪些?

3.属性选择器

4.设置样式jq

5.给元素设置类名和移除类名用哪个方法?

- addClass('active abc rtt');  添加多个类
- removeClass();
- toggleClass();  开关式的切换类名

6.动画

7.获取当前索引的方法:

- $(ths).index()

8.jq**对值的操作的方法**

- text()
  - 传入一个参数,给元素赋值文本
  - 不传入参数 获取当前文本
- html() 
  - 用法与text类似
- val()
  - 用法与text类似,但是只用于表单操作



## 第十四章 Django 

**前端知识补漏**

- select下拉选择框标签

  - 选择项需要option标签包裹

    - option里id属性,多个option设置同样id形成单选效果
    - option里values属性,是要选择提交的值,会赋值到select标签里的name属性

    

### 14.1 web框架

**1. web框架的本质**

- tcp/osi五层模型
  - 5.应用层
  - 4.传输层
  - 3.网络层
  - 2.数据链路层
  - 1.物理层
- socket
  - 中文名字:套接字
  - **Socket是应用层与传输层中间的抽象层，Socket帮助去组织拼接信息数据，以符合指定的协议。**
  - socket对于程序员来说,已经是网络操作的底层了                                     ![1557198171665](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1557198171665.png)
- 网络应用开发构架
  - C/S 	
    - client 客户端
    - serve 服务端
  - B/S   
    - browser 浏览器
    - server 服务端

**1.1 web框架初识**

- web框架的本质就是实现socket服务端的实现

- tcp协议 socket基本格式

  ```python
  import socket
  
  sk = socket.socket()
  sk.bind(('127.0.0.1',8000))
  sk.listen(10)
  
  while True:
      conn,addr = sk.accept()
      conn.recv(1024)
      conn.send(b'abc')
      conn.close()
  sk.close()
  ```



**2. http协议**

- 超文本传输协议（英文：Hyper Text Transfer Protocol，HTTP）是一种用于分布式、协作式和超媒体信息系统的**应用层协议**,双工通信。
- HTTP是一个客户端终端（用户）和服务器端（网站）**请求和应答**的标准（TCP）;**即规定发送与接受的数据格式;**
- 默认端口为80;

**2.1 HTTP协议工作原理**

<https://www.cnblogs.com/maple-shaw/articles/9060408.html>

- HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。

- http适用tcp协议通信

  ```python
  #在浏览器地址栏键入URL,按下回车之后会经历以下流程:(面试)
  
  1.浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;
  2.解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立TCP连接;
  3.浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 TCP 三次握手的第三个报文的数据发送给服务器;
  4.服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;
  5.释放(断开) TCP连接;
  6.浏览器将该 html 文本并显示内容; 
  ```

2.2 HTTP请求方法

- **八种方法(get/post是重点 head put delete options trace connect)**
- **get 获取数据;向指定的资源发出“显示”请求。** *使用GET方法应该只用在读取数据，而不应当被用于产生“副作用”的操作中，例如在Web Application中。其中一个原因是GET可能会被网络蜘蛛等随意访问*。
- **post 向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。** *数据被包含在请求本文中。这个请求可能会创建新的资源或修改现有资源，或二者皆有*。

**2.3 http状态码**

- *所有HTTP响应的第一行都是状态行，依次是当前HTTP版本号，3位数字组成的状态代码，以及描述状态的短语，彼此由空格分隔。*
- 状态代码的第一个数字代表当前响应的类型：
  - **1xx消息**——请求已被服务器接收，继续处理
  - **2xx成功**——请求已成功被服务器接收、理解、并接受
  - **3xx重定向**——需要后续操作才能完成这一请求
  - **4xx请求错误**——请求含有词法错误或者无法被执行404(资源不存在)403(权限不够)402
  - *5xx服务器错误——服务器在处理某个正确请求时发生错误*500服务器内部错误

**2.4 URL**

- 超文本传输协议（HTTP）的统一资源定位符将从因特网获取信息的五个基本元素包括在一个简单的地址中：
  - **传送协议。**
  - 层级URL标记符号(为[//],固定不变)
  - 访问资源需要的凭证信息（可省略）
  - **服务器**。（通常为域名，有时为IP地址）
  - **端口号**。（以数字方式表示，若为HTTP的默认值“:80”,https默认端口(443)可省略）
  - **路径**。（以“/”字符区别路径中的每一个目录名称）
  - **查询**。（GET模式的窗体参数，以“?”字符为起点，每个参数以“&”隔开，再以“=”分开参数名称与数据，通常以UTF8的URL编码，避开字符冲突的问题）
  - 片段.锚点。以“#”字符为起点

**2.5 HTTP请求与响应格式(重点)**

- http请求格式(request请求,浏览器给服务器发送的消息)
  - get请求没有请求数据
  - ![1560219223260](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560219223260.png)
- http响应格式(response 响应,服务器返回给浏览器的信息)
  - ![1560219236823](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560219236823.png)

**3. web框架功能**

3.1 **收发消息**

3.2 **不同的路径返回不同内容**

```python
#根据URL中不同的路径返回不同的内容--函数进阶版 

import socket  
  
sk = socket.socket()  
sk.bind(("127.0.0.1", 8080))  # 绑定IP和端口  
sk.listen()  # 监听  
  
  
# 将返回不同的内容部分封装成不同的函数  
def index(url):  
    s = "这是{}页面XX！".format(url)  
    return bytes(s, encoding="utf8")  
  
  
def home(url):  
    s = "这是{}页面。。！".format(url)  
    return bytes(s, encoding="utf8")  
  
  
# 定义一个url和实际要执行的函数的对应关系  
list1 = [  
    ("/index/", index),  
    ("/home/", home),  
]  
  
while True:  
    # 等待连接  
    conn, add = sk.accept()  
    data = conn.recv(8096)  # 接收客户端发来的消息  
    # 从data中取到路径  
    data = str(data, encoding="utf8")  # 把收到的字节类型的数据转换成字符串  
    # 按\r\n分割  
    data1 = data.split("\r\n")[0]  
    url = data1.split()[1]  # url是我们从浏览器发过来的消息中分离出的访问路径  
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')  # 因为要遵循HTTP协议，所以回复的消息也要加状态行  
    # 根据不同的路径返回不同内容  
    func = None  # 定义一个保存将要执行的函数名的变量  
    for item in list1:  
        if item[0] == url:  
            func = item[1]  #确保函数被定义
            break  
    if func:  
        response = func(url)  
    else:  #函数未定义报错
        response = b"404 not found!"  
  
    # 返回具体的响应消息  
    conn.send(response)  
    conn.close()  
```

3.3 **返回动态html页面**

- 读取html文件,替换动态内容,模板渲染,发送

**4. 服务器程序和应用程序(了解)**

- WSGI（Web Server Gateway Interface）就是一种规范，它定义了使用Python编写的web应用程序与web服务器程序之间的接口格式，实现web应用程序与web服务器程序间的解耦。
- Python标准库提供的独立WSGI服务器叫**wsgiref**，Django开发环境用的就是这个模块来做服务器。
- 测试使用wsgiref模块,线上使用uwsgi
- jinja2 字符串替换模块

### 14.2django下载安装环境配置

- ###### django模块下载安装使用

  - 安装  pip3 install django==1.11.21

  - 创建django项目(在项目文件夹里打开终端) django-admin startproject 项目名称

    - (使用pycharm环境创建与创建普通py项目相同)
    - 项目目录结构

    ![1560226802020](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560226802020.png)

  - 运行Django项目   

    - 命令行  python3 manage.py runserver ip:端口号   `python3 manage.py runserver 127.0.0.1:80  `
    - pycharm启动urls文件(启动前确定端口未被占用)

  - **Django基础必备三件套**:

    - HttpResponse 内部传入一个字符串参数，返回给浏览器。

      ```python
      from django.shortcuts import HttpResponse
      def index(request):
          # 业务逻辑代码
          return HttpResponse("OK")
      ```

    - render   除request参数外还接受一个待渲染的模板文件和一个保存具体数据的字典参数。

      将数据填充进模板文件，最后把结果返回给浏览器。（类似于我们上面用到的jinja2）

      ```python
      from django.shortcuts import render
      def index(request):
          # 业务逻辑代码
          return render(request, "index.html", {"name": "alex", "hobby": ["烫头", "泡吧"]})
      ```

    - redirect  接受一个URL参数，表示跳转到指定的URL。

      ```python
      from django.shortcuts import redirect
      def index(request):
          # 业务逻辑代码
          return redirect("/home/")
      ```

**1. 静态文件配置和使用**

- settings.py写入以下代码，static可以多选；

  ```python
  STATIC_URL = '/static/'   # 别名
  STATICFILES_DIRS = [
      os.path.join(BASE_DIR, 'static1'),
      os.path.join(BASE_DIR, 'static'),
      os.path.join(BASE_DIR, 'static2'),
  ]
  ```

  ```python
  <link rel="stylesheet" href="/static/css/login.css">   # 别名开头
  ```

  按照STATICFILES_DIRS列表的顺序进行查找。

  - html文件引入时导入的link标签路径/static/文件名称

- **在项目根目录创建static文件夹，包含常见的css,,js,,img,,plugins(插件)**

  ## 

**2.简单的登录实例**

```目前为了避免403错误,提交post请求时,把settings中MIDDLEWARE的csrf行注释掉```

- from表单使用时注意要点:
  1. form属性
     - action  要提交的地址 (不写内容表示提交到当前地址)
     - method  数据请求的方式
  2. imput属性要有name
  3. 表单中要有一个input=submit,或者有一个buttom按钮
- **request.method** 获取数据请求方法,返回字符串'POST'/'GET';
- **request.POST**  获取数据from表单POST提交的数据,返回的是类字典,通过键直接取到字符串值;

**3.app 配置**

1. app创建

   - 命令行  python manage.py startapp app名称
   - pycharm工具 tools-->run manage.py task-->startapp  app名称(自动注册)

2. 注册app

   - settings

   ```python
   INSTALLED_APPS = [
       ···
       'app01.apps.App01Config',  # 推荐写法
   ]
   ```

3. app文件目录及功能

   - app_test
     - migrations  (文件夹)
     - admin.py   --Django后台管理工具
     - apps.py    
     - models.py   --orm相关内容
     - tests.py
     - **views.py   --python函数**

**4.ORM 配置**

- 对象关系映射（Object Relational Mapping，简称ORM）模式是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。
- ORM是通过使用描述对象和数据库之间映射的元数据，将程序中的对象自动持久化到关系数据库中。
- ORM提供了对数据库的映射，不用直接编写SQL代码，只需操作对象就能对数据库操作数据。

**4.1 orm与数据库对应关系**

![1560321847605](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560321847605.png)

**4.2 Django中mysql数据库使用**

1. 创建一个mysql数据库; create database 

2. 在setting中配置,Django 链接Mysql数据库loc

   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',    # 引擎	
           'NAME': 'day53',						# 数据库名称
           'HOST': '127.0.0.1',					# ip地址
           'PORT':3306,							# 端口
           'USER':'root',							# 用户
           'PASSWORD':'123'						# 密码
       }
   }
   ```

3. 在与settings同级目录下的init文件夹中写入代码:

   - 使用pymysql替代django自带的mysqldb,因为mysqldb只支持python2

   ```python
   import pymysql
   pymysql.install_as_MySQLdb()
   ```

4. 创建表(在app下的models.py中写类)

   ```python
   from django.db import models
   
   # Create your models here.
   class User(models.Model):
       username = models.CharField(max_length=32)  # username varchar(32)
       password = models.CharField(max_length=32)
   ```

5. 执行命令

   - python manage.py makemigrations   #  检测每个注册app下的model.py   记录model的变更记录
   - python manage.py migrate    #  同步变更记录到数据库中

**4.3 orm操作**

```python
# 获取表中所有的数据
ret = models.User.objects.all()  # QuerySet 对象列表  【对象】
# 获取一个对象（有且唯一）
obj = models.User.objects.get(username='alex')   # 获取不到或者获取到多个对象会报错
# 获取满足条件的对象
ret = models.User.objects.filter(username='alex1',password='dasb')  # QuerySet 对象列表
```



### 14.3 django - - 模板系统

##### 14.3 .1MVC框架和MTV框架

(面试:区别)

- MVC

  - model 模型

  - view 视图--HTML

  - controller 控制器 (路由,控制指令,业务逻辑)

    ![1560825001584](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560825001584.png)

- MTV(django中的)

  - model 模型 ORM

  - tempalte 模板 --HTML

  - view 业务逻辑

    ![1560825031980](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560825031980.png)

##### 14.3.2.Django模板

**2.1 变量**

- {{ 变量 }}

- 取list中的第一个参数

  ```
  {{ list.0 }} 
  ```

- 取字典中key的值

  ```python
  {{ dic.name }}  
  
  #还可以调用字典的方法:.keys/.values/.items,获取相应的值
  ```

- 取对象的name属性

  ```
  {{ obj.name }}
  ```

- .操作只能调用不带参数的方法

  ```
  {{ obj.func }}
  ```

```python
#当模板系统遇到一个（.）时，会按照如下的顺序去查询：
1. 在字典中查询
2. 属性或者方法
3. 数字索引
```

**2.2标签tag**

- {% 逻辑相关 %}

- **{{for}} {{endfor}}**

  ```python
  #for循环的一些参数
  forloop.counter	当前循环的索引值（从1开始）
  forloop.counter0	当前循环的索引值（从0开始）
  forloop.revcounter	当前循环的倒序索引值（到1结束）
  forloop.revcounter0	当前循环的倒序索引值（到0结束）
  forloop.first	当前循环是不是第一次循环（布尔值）
  forloop.last	当前循环是不是最后一次循环（布尔值）
  forloop.parentloop	本层循环的外层循环
  
  # for ... empty
  循环对象为空时,显示empty的内容
  
  <ul>
  {% for user in user_list %}
      <li>{{ user.name }}</li>
  {% empty %}
      <li>空空如也</li>
  {% endfor %}
  </ul>
  ```

- **{{if}}  {{endif}}**

  - if语句支持 and 、or、==、>、<、!=、<=、>=、in、not in、is、is not判断

  - 在不同的语言当中,如果连续判断结果是不易=一样的

    ```python
    #在python中
    10>5>1  -->   10>5  and 5>1   -->true 
    #在js中
     10>5>1  -->   10>5  --> true   -->   1>1  false
    #在djang模板中,不支持连续判断,也不算数运算
    ```

- **with**  定义一个中间变量(起别名)

  ```python
  # 变量只在标签内有效
  {% with total=business.employees.count %}
      {{ total }} employee{{ total|pluralize }}
  {% endwith %}
  ```

- 注释 {#...#}

**2.3 Filters 内置过滤器**

- 用来修改变量的显示结果

- 语法： {{ value|filter_name:参数 }}  **':'左右没有空格没有空格没有空格**

- 过滤器

  - **default** 未定义/空/None/False的时候显示默认自定义内容

    ```python
    {{ value|default:"nothing"}}
    #如果value值没传的话就显示nothing
    #seetings中TEMPLATES的OPTIONS可以增加一个选项：string_if_invalid：'找不到'，可以替代default的的作用。
    ```

  - **filesizeformat** 将值格式化为一个 “人类可读的” 文件尺寸 （例如 '13 KB', '4.1 MB', '102 bytes', 等等）。例如：

    ```python
    {{ value|filesizeformat }}
    #如果 value 是 123456789，输出将会是 117.7 MB。
    ```

  - **add** 给变量加参数

    ```python
    {{ value|add:"2" }}
    #value是数字4，则输出结果为6。
    {{ first|add:second }}
    # 如果first是 [1,.2,3] ，second是 [4,5,6] ，那输出结果是 [1,2,3,4,5,6] 
    ```

  - **lower**小写

    ```
    {{ value|lower }}
    ```

  - **upper**大写

    ```
    {{ value|upper}}
    ```

  - **title**标题

    ```
    {{ value|title }}
    ```

  - **ljust**左对齐

    ```
    "{{ value|ljust:"10" }}"
    ```

  - **rjust**右对齐

    ```
    "{{ value|rjust:"10" }}"
    ```

  - **center**居中

    ```
    "{{ value|center:"15" }}"
    ```

  - **length**返回value的长度

    ```
    {{ value|length }}
    ```

    返回value的长度，如 value=['a', 'b', 'c', 'd']的话，就显示4.

  - **slice**切片

    ```
    {{value|slice:"2:-1"}}
    ```

  - **first**取第一个元素

    ```
    {{ value|first }}
    ```

  - **last**取最后一个元素

    ```
    {{ value|last }}
    ```

  - **join** 使用字符串拼接列表。同python的str.join(list)

    ```
    {{ value|join:" // " }}
    ```

  - **truncatechars**

    如果字符串字符多于指定的字符数量，那么会被截断。截断的字符串将以可翻译的省略号序列（“...”也占位）结尾

    参数：截断的字符数

    ```
    {{ value|truncatechars:9}}
    
    ```

  #truncatewords  只针对英文

  ```
  - **date** 日期格式化
  
    ```python
    {{ value|date:"Y-m-d H:i:s"}}
    
  #可以在django项目settings中全局格式化datatime时间:
    USE_L10N = False
  DATATIME_FORMAT = "Y-m-d H:i:s"
    
    #DATA
    DATAE_FORMAT = "Y-m-d"
  #TIME
    TIME_FORMAT = "H:i:s"
  ```

  - **safe** 告诉django,此数据安全,不需要转义

    ```
    {{ value|safe}}
    
    #{{value|add:123|save}}
    ```

    ```python
    #帮助理解
    Django的模板中会对HTML标签和JS等语法标签进行自动转义，原因显而易见，这样是为了安全。但是有的时候我们可能不希望这些HTML元素被转义，比如我们做一个内容管理系统，后台添加的文章中是经过修饰的，这些修饰可能是通过一个类似于FCKeditor编辑加注了HTML修饰符的文本，如果自动转义的话显示的就是保护HTML标签的源文件。为了在Django中关闭HTML的自动转义有两种方式，如果是一个单独的变量我们可以通过过滤器“|safe”的方式告诉Django这段代码是安全的不必转义。
    ```

  - **add** 加法/减法

    ```python
    #加法
    {{value|add:10}}
    note:value=5,则结果返回15
    #减法
    {{value|add:-10}}
    note:value=5,则结果返回-5，加一个负数就是减法了
    ```

  - **widthratio** 乘法/除法

    ```python
    #乘法
    {% widthratio 5 1 100%}
    note:等同于：(5 / 1) * 100 ，结果返回500，withratio需要三个参数，它会使用参数1/参数2*参数3的方式进行运算，进行乘法运算，使「参数2」=1
    #除法
    {% widthratio 5 100 1%}
    note:等同于：(5 / 100) * 1,则结果返回0.05,和乘法一样，使「参数3」= 1就是除法了。
    ```

**2.4 自定义Filters过滤器**

- ##### 创建

  1. 在app下创建一个名为**templatetags**的python包(名称不能变)

  2. 在templatetags 创建py文件 自定义名称 my_tags.py(名称自定义)

  3. 在py文件中写入:

     ```python
     from django import template
     
     register = template.Library()  # register也不能变
     ```

  4. 写函数+装饰器

     ```python
     @register.filter
     def add_xx(value, arg):  # 最多有两个
     
         return '{}-{}'.format(value, arg)
     ```

- ##### 使用

  5. 使用

     ```python
     {% load my_tags %}
     {{ 'alex'|add_xx:'dsb' }}
     ```

     

##### 14.3.3 csrf验证/模板/组件/静态文件

1. ### csrf

- 跨站请求伪造 Cross-site request forgery

2. ### csrf_token

- 这个标签用于跨站请求伪造保护。

  ```
  #在页面的form表单里面写上
  {% csrf_token %} 
  ```

3. ### 母版和继承

- 模板就是一个普通的html页面

- 提取到多个页面的公共部分

- 定义block块

  ```
  {% block title %}
  
  {% endblock %}
  ```

- 继承:

  ```
  1. {% extends ‘base.html’ %}
  2. 重写block块   —— 写子页面独特的内容
  ```

**#注意要点**

```
1. 在继承时,{% extends 'base.html' %} 写在第一行,前面不要有内容,有内容会显示
2. {% extends 'base.html' %}  'base.html' 加上引号   不然当做变量去查找
3. 把要显示的内容写在block块中
4. 定义多个block块，定义 css  js 块
```

4. ### 组件

- 组件就是一小段HTML代码段 
- 自定义nav.html 
- 在html文件中写入 {% include ‘nav.html ’  %}即可引入自定义组件
  - inculde 包含

5.静态文件

```python
{% load static %}

 <link rel="stylesheet" href="{% static '/plugins/bootstrap-3.3.7/css/bootstrap.c ss' %}">
 <link rel="stylesheet" href="{% static '/css/dsb.css' %}">

{% static '/plugins/bootstrap-3.3.7/css/bootstrap.css' %}  
{% get_static_prefix %}   ——》 获取别名
```

6. ### 自定义标签

- **filter 自定义过滤器**

  - 创建

    1. 在app下创建一个名为**templatetags**的python包(名称不能变)

    2. 在templatetags 创建py文件 自定义名称 my_tags.py(名称自定义)

    3. 在py文件中写入:

       ```python
       from django import template
       
       register = template.Library()  # register也不能变
       ```

    4. 写函数+装饰器

       ```python
       @register.filter
       def add_xx(value, arg):  # 最多有两个
       
           return '{}-{}'.format(value, arg)
       ```

  - ##### 使用

    5. 使用

       ```python
       {% load my_tags %}
       {{ 'alex'|add_xx:'dsb' }}
       ```

       

- **simpletag**   标签(返回内容自定义)

  - 和自定义filter类似，只不过接收更灵活的参数。

  - 定义注册simple tag

    ```python
    @register.simple_tag(name="plus")
    def plus(a, b, c):
        return "{} + {} + {}".format(a, b, c)
    ```

  - 使用自定义simple tag

    ```python
    {% load app01_demo %}
    
    {# simple tag #}	
    {% plus "1" "2" "abc" %}
    ```

- **inclusion_tag**  标签(函数内返回内容必须是一个字典,第三个文件去渲染,然后代码段返回给html文件)

  示例：

  1. **templatetags/my_inclusion.py** (定义标签函数)

  ```python
  from django import template
  
  register = template.Library()
  
  
  @register.inclusion_tag('result.html') #结果返回给result.html渲染
  def show_results(n):
      n = 1 if n < 1 else int(n)
      data = ["第{}项".format(i) for i in range(1, n+1)]
      return {"data": data}
  ```

  2. **templates/result.html** (以键取值,循环)

  ```python
  <ul>
    {% for choice in data %}
      <li>{{ choice }}</li>
    {% endfor %}
  </ul>
  ```

  3. **templates/index.html**

  ```python
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="x-ua-compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>inclusion_tag test</title>
  </head>
  <body>
  
  {% load my_inclusion %}
  
  {% show_results 10 %}  #调用自定义函数名,得到渲染后的结果
  </body>
  </html>
  ```

  - 处理流程图解

  ![1560929029828](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560929029828.png)



### 14.4 django - - 视图view

- view处理逻辑

- MTV模型

  ![1561011720513](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561011720513.png)

##### 14.4.1 .CBV和FBV

- FBV  -- function based view 函数视图
- CBV  -- class based view  类视图

1. **CBV应用**

   ```python
   #定义
   from django.views import View
   
   class AddPublisher(View): 
       #在views中创建好类,在url中调用的时需要在类名之后加上.as_view()方法
       
       def get(self,request):
           """处理get请求"""
           return response
       
       
       def post(self,request):
           """处理post请求"""
           return response
       
   #使用
   url(r'^add_publisher/', views.AddPublisher.as_view()),
   ```

2. **as_view()方法的执行的流程**

   #源码as_view()

   ```python
   1.项目启动时,加载url.py时,执行 类.as_view() ,返回一个view函数 #类中包含view函数;
   2.请求到来的时候执行view函数:
       (1)函数实例化自定义的类,并赋值给self  # self = cls(**initkwargs)
       	并执行  # self.request = request
       (2)view函数调用dispatch方法, #self.dispatch(request, *args, **kwargs)
       (3)在dispatch中,判断请求方式是否存在(被允许):
          - 允许(存在):,通过反射获取到与请求方式对应的方法,然后赋值给handler
          - 不允许(不存在):handler = self.http_method_not_allowed
       (4)在disparch执行完毕后执行handler #handler()
       	- 允许(存在):执行对应的函数(get/post)
         	- 不允许(不存在):执行http_method_not_allowed,返回报错信息
   ```

3. **FBV视图加装饰器**

- 直接加在对应的函数方法之上,无特殊性

4. **CBV加装饰器**

   `使用method_decorator之前首先要导入from django.utils.decorators import method_decorator模块,使用方法:@method_decorator(装饰器)`

   - 加在方法上

     ```python
     from django.utils.decorators import method_decorator  # 导入模块   --decorator 装饰
     
     @method_decorator(timer)
     def get(self, request, *args, **kwargs):
         """处理get请求"""
     ```

   - 加在dispatch上

     ```python
     #加在自定义的dispatch方法之上
     @method_decorator(timer)
     def dispatch(self, request, *args, **kwargs):		#类中自定义dispatch方法
         # print('before')
         ret = super().dispatch(request, *args, **kwargs) # 找到父级dispatch方法运行
         # print('after')
         return ret
     ```

   - 加载类上(推荐)

     - 加在父类上

       ```python
       #直接加在父类的dispath方法之上!!!
       @method_decorator(timer,name='dispatch')		# decorator 装饰
       class AddPublisher(View):
       ```

     - 加在本身类上

       ```python
       @method_decorator(timer,name='post')
       @method_decorator(timer,name='get')
       class AddPublisher(View):
           def get(self,request)...
           def post(self,request)...
       ```

       

   ```python
   #使用method加装饰器和不使用时加装饰器的区别(了解)
   
   #不适用method_decorator
   func   ——》 <function AddPublisher.get at 0x000001FC8C358598>
   args  ——》 (<app01.views.AddPublisher object at 0x000001FC8C432C50>, <WSGIRequest: GET '/add_publisher/'>)  #第一个参数不是request
   
   #使用method_decorator之后：
   func ——》 <function method_decorator.<locals>._dec.<locals>._wrapper.<locals>.bound_func at 0x0000015185F7C0D0>
   args ——》 (<WSGIRequest: GET '/add_publisher/'>,) #第一个参数是request
   ```

##### 14.4.2. request对象

- 属性

  ```python
  # 属性
  request.methot   获取当前请求方式  GET
  request.GET    获取url上携带的参数
  request.POST   获取POST请求提交的数据
  request.path_info   获取URL的路径    不包含ip和端口  不包含参数
  request.body    获取请求体  b''
  request.FILES   获取上传的文件
  request.META    获取请求头 
  
  request.COOKIES  cookie		(待讲解)
  request.session	 session	(待讲解)
  ```

- 方法

  ```python
  # 方法
  request.get_host()    获取主机的ip和端口
  request.get_full_path()   获取URL的路径   不包含ip和端口 包含参数
  request.is_ajax()    判断是都是ajax请求
  ```

- from表单上传文件注意事项:

  - 把input标签的type设置为file,并设置name

  - from表单中加入属性 enctype="multipart/form-data"

    ```html
    <form action="" method="post" enctype="multipart/form-data">
        {% csrf_token %}
        <input type="file" name="f">
        <bottun>上传</bottun>
    </form>
    ```

  - 在服务器获取

    - 使用request.FILE.get('name'),方法获取上传的文件对象

    - 使用f.chunks()获取文件对象的值

    - 文件对象具有name属性

      ```python
      class Upload(View):
      
          def get(self, request):
              return render(request, 'upload.html')
      
          def post(self, request):
              f = request.FILES.get('f')   #获取文件对象
              with open(f.name, mode='wb')as F:
                  for i in f.chunks():	#读取内容
                      F.write(i)
              return render(request, 'upload.html')
      ```

##### 14.4.3. response 对象

#需要事先导入模块 from django.shortcuts import render, redirect, HttpResponse

- **HttpResponse('字符串')**    -默认属性-(**content-type='text/html')
  - 返回字符串
- **render(request,'模板的文件名',{k1:v1})** 
  - 返回一个完整的HTMl文件
- **redirect('重定向的地址')**
  - 重定向 ,本质是返回一个   location: 地址    --响应头地址

------

#需要事先导入模块from django.http.response import JsonResponse

- **JsonResponse**  把字典或者列表序列化之后再返回
  - JsonResponse({})  字典序列化返回  -默认属性-(**content-type='application/json')**
  - JsonResponse([],safe=False)  列表序列化返回,要把safe的值设为False
    - 'In order to allow non-dict objects to be serialized set the ' 'safe parameter to False.''
    - 为了允许非dict对象被序列化，将''安全参数设置为False。



### 14.5 django - - 路由系统

1. #### 正则表达式

- r  转义符
- ^  以什么开头
- $  以什么结尾
- \d  数字
- \w 标识符(word)表示大小写字母,数字,下划线
- .  表示除了换行符的任意内容
- \d{n} 数字n表示该原字符执行次数,且只能匹配这么多次
- \d{n,}数字n表示该原字符至少出现n次
- \d{n,m}数字n表示该原字符至少出现n次,至多出现m次
- \d?    ?表示匹配0次或者1次   ,比如小数点
- \d+   +表示匹配1次或多次
- \d *   *表示匹配0次或多次  ,比如匹配整数或者小数

2. #### URLconf配置

```python
from django.conf.urls import url

urlpatterns = [
     url(正则表达式, views视图，参数，别名),
]

#在url传参时要以字典形式传参
urlpatterns = [
    url(r'^blog$', views.year_archive, {'foo': 'bar'}),
]
```

- 正则表达式：一个正则表达式字符串

- views视图：一个可调用对象，通常为一个视图函数

- 参数：可选的要传递给视图函数的默认参数（字典形式）

- 别名：一个可选的name参数

  ```python
  #Django2.0版本路由系统的写法
  
  from django.urls import path，re_path
  urlpatterns = [
      path('articles/2003/', views.special_case_2003),
      path('articles/<int:year>/', views.year_archive),
      path('articles/<int:year>/<int:month>/', views.month_archive),
      path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
  ]
  ```

3. #### 分组匹配

```每个在URLconf中捕获的参数都作为一个普通的Python字符串传递给视图，无论正则表达式使用的是什么匹配方式```

- 分组

  - 正则表达式中加一个()表示分组

    ```python
    url(r'^blog/([0-9]{4})/$', views.blogs),  #分组:将捕获的参数以元组的形式按位置传参传递给视图函数----捕获的都是字符串
    ```

- 命名分组

  - (?P<名称>正则表达式)

    ```python
    url(r'^blog/(?P<year>[0-9]{4})/$', views.blogs),#命名分组:将捕获的参数按关键字以字典传参传递给视图函数----捕获的都是字符串
    ```

4. #### 路由分发include

- 在协同合作开发时,每个人开发的功能不一样,就可以自定义app,运用include,灵活调用

```include  包含```

1. 首先定义--项目settings同目录下的urls.py中的内容

   ```python
   from django.conf.urls import include, url
   
   urlpatterns = [
   	url(r'^admin/', admin.site.urls),
   	url(r'^app_func1/', include('app_func1.urls')),  # 这种写法会指定到app_func文件夹下的urls文件
       url(r'^app_func2/', include('app_func2.urls')), 
   ]    
   ```

2. 然后再---自定义app_func中的urls.py中的内容

   ```python
   #到app_func中找到urls文件进行匹配
   
   from django.conf.urls import url
   from . import views
   
   urlpatterns = [
       # url(r'^admin/', admin.site.urls),
       url(r'^blog/$', views.blog),
       url(r'^blog/(?P<year>[0-9]{4})/(?P<month>\d{2})/$', views.blogs),
   ]
   ```

- #首先加载settings同目录下的urls.py中的内容,匹配app_func,然后去匹配include的内容
- #由include中的内容匹配二层地址
- 可以创建多个urls.py文件

5. #### URL的命名和反向解析

`通过自定义别名name取到完整的url路径`

- 命名的别名只能用在模板和py文件中,不能用在url中

**5.1静态路由**

- **命名**

  ```python
  url(r'^blog/$', views.blog, name='blog_name'),
  ```

- **反向解析**(在模板和py文件中用到反向解析)--**通过自定义别名name取到完整的url路径**

  - 模板中调用反向解析获取的url路径

    ```python
    {% url'blog_name'%}    #获取到blog的完整路径  /blog_name/
    ```

  - py文件中调用反向解析获取的url路径

    ```python
    from django.urls import reverse
    reverse('blog_name')   #获取到blog的完整路径 '/blog_name/'
    ```

**5.2 动态路由**

- 分组完成动态路由

  - 自定义别名

    ```python
    url(r'^blog/([0-9]{4})/(\d{2})/$', views.blogs, name='blogs'),
    ```

  - 反向解析

    - 在模板中

      ```python
      {% url 'blogs' '2222' '12' %}"   ——》  /blog/2222/12/
      ```

    - py文件中

      ```python
      from django.urls import reverse
      reverse('blogs',args=('2019','06'))   ——》 /app01/blog/2019/06/ 
      ```

- 命名分组

  ```python
  url(r'^blog/(?P<year>[0-9]{4})/(?P<month>\d{2})/$', views.blogs, name='blogs'),
  ```

  - 反向解析

    - 模板

      ```python
      #两种方式
      
      {% url 'blogs' '2222' '12' %}"   ——》  /blog/2222/12/ 
      
      {% url 'blogs' year=2222 month=12 %}"   ——》  /blog/2222/12/
      ```

  ```
    
    - py文件中
    
      ```python
      #两种方式
      
      from django.urls import reverse
      reverse('blogs',args=('2019','06'))   ——》 /app01/blog/2019/06/ 
      
      reverse('blogs',kwargs={'year':'2019','month':'06'})   ——》 /app01/blog/2019/06/ 
  ```

6. #### 命名空间模式namespace

`作用:即使不同的APP使用相同的URL名称，URL的命名空间模式也可以反向解析命名的URL。`

- 定义时在setting同目录下的urls.py中写入

  ```python
  urlpatterns = [
      # url(r'^admin/', admin.site.urls),
      url(r'^app01/', include('app01.urls', namespace='app01')),
      url(r'^app02/', include('app02.urls', namespace='app02')),
  ]
  ```

- 调用时在别面馆前面加上namespace 名称

  - 在模板中

    ```python
    {% url 'app01:blogs' 2222 12 %}
    ```

  - 在py文件中

    ```python
    reverse('app01:blogs',args=('2019','06'))
    ```

    

### 14.6 django - - orm  

##### 1.orm的字段和参数

- orm字段--orm -----Object Relational Mapping

  ```python
  AutoField   #设置自增主键
  IntegerField  #设置整数
  CharField    #设置字符串
  BooleanField  #设置布尔值  
  TextField    #设置文本
  DateTimeField  #设置日期时间
  	auto_now_add=True    # DateTime参数  --表示新增数据的时候会自动保存当前的时间
      auto_now=True        # DateTime参数  --表示新增、修改数据的时候会自动保存当前的时间
  
  DateField  #设置日期时间
      auto_now_add=True    # DateTime参数  --表示新增数据的时候会自动保存当前的时间
      auto_now=True        # DateTime参数  --表示新增、修改数据的时候会自动保存当前的时间
  DecimalField   #十进制的小数
  	max_digits       # DecimalField属性  小数总长度   5
      decimal_places   # DecimalField属性  小数位长度   2
  ```

- 自定义字段(了解)

- 字段参数 

  ```python
  null = Ture    # 数据库中字段可以为空
  blank=True    #form表单填写时可以为空
  default       # 字段的默认值
  db_index=True   # 创建索引
  unique     #值唯一
  edittable=True  #可编辑
  hrlptext='提示信息'
  choices=((0, '女'), (1, '男'))    #可填写的内容和提示
  ```

```python
class Person(models.Model):
    pid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    age = models.IntegerField(default=18)
    birth = models.DateTimeField(auto_now_add=True)
    phone = models.CharField(max_length=11,null=True,blank=True)
	gender= models.BooleanField('性别',choices=((0, '女'), (1, '男')))   #选择性别,默认为0
```

##### 2.表的参数(了解)

```python
class Person(models.Model):
    pid = models.AutoField(primary_key=True)
    class Meta:
    db_table = "person"  # 表名

    verbose_name = '个人信息'

    verbose_name_plural = '个人信息'

    index_together = [
        ("name", "age"),  # 联合索引,应为两个存在的字段
    ]
    unique_together = (("name", "age"),)   # 应为两个存在的字段
```

##### 3. ORM查询(13条)   必知必会13条

```python
#all()   获取所有的数据   ——》 QuerySet   对象列表
ret = models.Person.objects.all()

# get()  获取满足条件的一个数据   ——》 对象
#  获取不到或者多个都报错
ret = models.Person.objects.get(pk=1)

# filter()    获取满足条件的所有数据   ——》 QuerySet   对象列表
ret = models.Person.objects.filter(pk=1)

# exclude()    获取不满足条件的所有数据   ——》 QuerySet   对象列表
ret = models.Person.objects.exclude(pk=1)

# values()拿到对象所有的字段和字段的值 QuerySet  [ {} ,{} ]
# values('字段')  拿到对象指定的字段和字段的值 QuerySet  [ {} ,{} ]
ret = models.Person.objects.values('pid','name')

# values_list()    拿到对象所有的字段的值    QuerySet  [ () ,() ]
# values('字段')   拿到对象指定的字段的值      QuerySet  [ {} ,{} ]
ret = models.Person.objects.values_list('name','pid')

# order_by  排序   - 降序加一个负号    ——》 QuerySet  [ () ,() ]
ret = models.Person.objects.all().order_by('age','-pid') #传多个参数

# reverse  反向排序   只能对已经排序的QuerySet进行反转
ret = models.Person.objects.all()
ret = models.Person.objects.all().reverse()

# distinct 去重  完全相同的内容才能去重
ret = models.Person.objects.values('age').distinct()

#  count()  计数
ret = models.Person.objects.all().count()

# first  取第一元素   没有元素 None
# last  取最后一元素   没有元素 None
ret = models.Person.objects.filter(pk=1).values().first()

# exists 查询的数据是否存在
ret = models.Person.objects.filter(pk=1000).exists()
```

- 帮助记忆

  ```python
  #返回queryset
  all()    
  filter()
  exclude()
  values()  
  values_list() 
  order_by() 
  reverse() 
  distinct()
  
  #返回对象
  get() 
  first()
  last()
  
  #返回数字
  count()
  
  #返回布尔值的
  exists()
  ```



小结:

```python
#Django ORM基本增删改查
from app01 import models  # 导入models模块
def orm(request):
# 创建数据
    # 第一种方式
     models.UserInfo.objects.create(username="root",password="123")
    # 第二种方式
     obj = models.UserInfo(username='fzh', password="iajtag")
     obj.save()
    # 第三种方式
     dic = {'username':'fgf', 'password':'666'}
     models.UserInfo.objects.create(**dic)

# 查询数据
    # result = models.UserInfo.objects.all()  # 查询所有，为QuerySet类型，可理解成列表
    # result = models.UserInfo.objects.filter(username="fgf",password="666")  # 列表
    # result = models.UserInfo.objects.filter(username="fgf").first()  # 对象
    # 条件查询。filter 相当于where查询条件，里面的"，"会组成and条件
    # for row in result:  # 打印查询到数据。
    #     print(row.id,row.username,row.password)

    # 查看QuerySet类型具体做了什么事情，可以： print(result.query)

# 删除数据
    # models.UserInfo.objects.all().delete()  # 删除所有
    # models.UserInfo.objects.filter(id=4).delete()  # 删除所有

# 更新数据
    # models.UserInfo.objects.all().update(password=8888)
    # models.UserInfo.objects.filter(id=3).update(password=888888)

    return HttpResponse('orm')
```





##### 4.单表双下划线

```python
ret = models.Person.objects.filter(pk=1)   # 等于1的
ret = models.Person.objects.filter(pk__gt=1)   # gt  greater than   大于1的
ret = models.Person.objects.filter(pk__lt=3)   # lt  less than   小于3的
ret = models.Person.objects.filter(pk__gte=1)   # gte  greater than equal    大于等于1的
ret = models.Person.objects.filter(pk__lte=3)   # lte  less than  equal  小于等于

ret = models.Person.objects.filter(pk__range=[2,3])   # range  范围
ret = models.Person.objects.filter(pk__in=[1,3,10,100])   # in  成员判断

ret = models.Person.objects.filter(name__contains='A')	#内容中包含A的
ret = models.Person.objects.filter(name__icontains='A')   # 忽略大小写

ret = models.Person.objects.filter(name__startswith='a')  # 以什么开头
ret = models.Person.objects.filter(name__istartswith='A') # 忽略大小写


ret = models.Person.objects.filter(name__endswith='a')  # 以什么结尾
ret = models.Person.objects.filter(name__iendswith='I') # 忽略大小写


ret  = models.Person.objects.filter(birth__year='2019')	#取年份
ret  = models.Person.objects.filter(birth__contains='2018-06-24')
ret  = models.Person.objects.filter(phone__isnull=False) #取不为空的
```



##### 5.外键的操作

- models.ManyToManyField('要关联的表')

  - 多表关联,会自动生成第三张表,然后根据另外两张表的表明生成'表名_id'字段

- 多表关联

  ![1560750307389](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1560750307389.png)

- 在多表关联时

```python
print(author.books,type(author.books)) # author.books是关系管理对象
print(author.books.all(),type(author.books.all()))  #获取所有book相关的对象,返回的是一个对象列表
```

```python
#!!!!!!
for author in all_authors:
    print(author)
    print(author.pk)
    print(author.name)
    print(author.books,type(author.books)) # author.books是关系管理对象
    print(author.books.all(),type(author.books.all()))  #获取所有book相关的对象,返回的是一个对象列表
    print('*'*30)
```

- forloop.counter 计数

- forloop.last  判断是不是最后一次循环,返回布尔类型

- getlist()

  ```python
  #select选择框多选时
   request.POST.getlist('books')  #返回值是一个列表
  ```

- set

  - 多对多关系时,每次直接重新设置

    ```python
    # 多对多的关系
    author_obj.books.set(books)  #  每次重新设置
    ```

- datafield

------

- 在多表关联后,产生外键.

  ```python
  class Publisher(models.Model):
      name = models.CharField(max_length=32, verbose_name="名称")
  
  class Book(models.Model):
      title = models.CharField(max_length=32)
      pub = models.ForeignKey(Publisher, related_name='books',related_query_name='xxx',on_delete=models.CASCADE)
  
  ```

- 正反向查询

  - 从一查多查询时通过类名_set()查询
  - 从多查一的时候,直接通过外键查询

  ```python
  # 基于对象的查询
  # 正向
  book_obj = models.Book.objects.get(title='菊花怪大战MJJ')
  # print(book_obj.pub)
  
  # 反向
  pub_obj = models.Publisher.objects.get(pk=1)
  # print(pub_obj.book_set.all())  # 类名小写_set   没有指定related_name
  # print(pub_obj.books.all())   #  指定 related_name='books'
  
  
  
  # 基于字段的查询
  ret = models.Book.objects.filter(title='菊花怪大战MJJ')
  #  查询老男孩出版的书
  ret = models.Book.objects.filter(pub__name='老男孩出版社')
  
  
  # 查询出版菊花怪大战MJJ的出版社
  # 没有指定related_name   类名的小写
  ret= models.Publisher.objects.filter(book__title='菊花怪大战MJJ')
  # related_name='books' 
  ret= models.Publisher.objects.filter(books__title='菊花怪大战MJJ')
  
  
  # related_query_name='xxx'(了解)
  ret= models.Publisher.objects.filter(xxx__title='菊花怪大战MJJ')
  ```

##### 6.多对多查询

```python
class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=6, decimal_places=2)  # 9999.99
    sale = models.IntegerField()
    kucun = models.IntegerField()
    
    def __str__(self):
        return self.title


class Author(models.Model):
    name = models.CharField(max_length=32, )
    books = models.ManyToManyField(Book)

    def __str__(self):
        return self.name
```

```python
# 基于对象的查询
mjj = models.Author.objects.get(pk=1)
# print(mjj.books)  #  ——》  关系管理对象
# print(mjj.books.all())

book_obj = models.Book.objects.filter(title='桃花侠大战菊花怪').first()
# 不指定related_name
# print(book_obj.author_set)  #  ——》  关系管理对象
print(book_obj.author_set.all())
# related_name='authors'
# print(book_obj.authors)  #  ——》  关系管理对象
print(book_obj.authors.all())

#跨表字段查询
ret  =  models.Author.objects.filter(books__title='菊花怪大战MJJ')
# print(ret)

# 不指定related_name
ret = models.Book.objects.filter(author__name='MJJ')
# 指定related_name='authors'
ret = models.Book.objects.filter(authors__name='MJJ')
# related_query_name='xxx' 优先级最高,了解
ret = models.Book.objects.filter(xxx__name='MJJ')
# print(ret)
```

##### 7.关系管理对象的方法

```python
mjj = models.Author.objects.get(pk=1)

all()  所关联的所有的对象

# print(mjj.books.all())
set  设置多对多的关系  set()内可以放 [id,id],也可以是[ 对象，对象 ]
# mjj.books.set([1,2])
# mjj.books.set(models.Book.objects.filter(pk__in=[1,2,3]))


add  添加多对多的关系   (id,id)   (对象，对象),如果存在的话不会再次新增
# mjj.books.add(4,5)
# mjj.books.add(* models.Book.objects.filter(pk__in=[4,5])) ()不能是qureset,加*打散

remove 删除多对多的关系  (id,id)   (对象，对象)
# mjj.books.remove(4,5)
# mjj.books.remove(* models.Book.objects.filter(pk__in=[4,5]))

clear()   清除所有的多对多关系
# mjj.books.clear()

create()
# obj = mjj.books.create(title='跟MJJ学前端',pub_id=1)
# print(obj)
# book__obj = models.Book.objects.get(pk=1)
#
# obj = book__obj.authors.create(name='taibai')
# print(obj)
```

##### 8.分组聚合

- aggregate  合计的
- annotate  注释

```python
#聚合函数
from app01 import models

from django.db.models import Max, Min, Avg, Sum, Count  #avg平均数
#具体用法
ret = models.Book.objects.filter(pk__gt=3).aggregate(Max('price'),avg=Avg('price'))
print(ret)


# 分组
# 统计每一本书的作者个数
ret = models.Book.objects.annotate(count=Count('author')) # annotate 注释


# 统计出每个出版社的最便宜的书的价格
# 方式一
ret = models.Publisher.objects.annotate(Min('book__price')).values()
# 方式二
ret = models.Book.objects.values('pub_id').annotate(min=Min('price'))

```

##### 9.F和Q

- F

```python
from django.db.models import F

# 比较两个字段sale和kuncun的值
ret=models.Book.objects.filter(sale__gt=F('kucun'))

# 只更新sale字段
models.Book.objects.all().update(sale=100)

# 取某个字段的值进行操作
models.Book.objects.all().update(sale=F('sale')*2+10)
```

- Q(条件)
  - |   或
  - &   与 
  - ~   非

```
from django.db.models import Q

ret = models.Book.objects.filter(Q(Q(pk__gt=3) | Q(pk__lt=2)) & Q(price__gt=50))
print(ret)
```



### 14.7 django - -cookie&session

参考博客<https://www.cnblogs.com/maple-shaw/articles/9502602.html>

##### 14.7.1.cookie

- 定义:

  - 保存在**浏览器本地**上一组组键值对

- 意义:

  - 不是python中都独有的,因为http协议是无状态的,每次http请求都是对立的,互相之间无关联,用cookie保存状态.

- 特点:

  1. **由服务器让浏览器进行设置的**
  2. 浏览器把cookie保存在在浏览器本地
  3. 下次重新访问时自动携带该cookie信息

- 应用:

  1. 登录验证  
  2. 保存浏览器设置习惯
  3. **存储简单的信息**

- django中设置cookie(都是操作响应头)

  - 通过response 对象设置

    ```python
    ret = redirect('/index')
    ret.set_cookie(key,value)  #在响应头中是--  set-cookie:key=value
    ```

  - 设置加密cookie

    ```python
    ret.set_signed_cookie(key,value,salt='加密盐',...)
    ```

  - 参数:

    - key, 键  

    - value='', 值 

    - max_age=None, 超时时间(默认是None,关闭浏览器cookie失效)

      ```python
      ret.set_signed_cookie('is_login', '1', 's21',max_age=10000,)
      ```

    - path='/', Cookie生效的路径，/ 表示根路径，特殊的：根路径的cookie可以被任何url的页面访问

    - domain=None, Cookie生效的域名

    - secure=False, https传输  状态为True时,只能http传输

- django中获取cookie

  - get方法获取

    ```python
    #获取未加密cookie
    ret = request.COOKIES.get('key')  
    #获取加密cookie
    is_login = request.get_signed_cookie('is_login', salt='s21', default='')
    ```

- 删除cookie

  - rep.delete_cookie("user")  # 删除用户浏览器上之前设置的user的cookie值

  ```python
  def logout(request):
      ret = redirect("/login/")
      ret.delete_cookie("user")  # 删除用户浏览器上之前设置的user的cookie值
      return rep
  ```

##### 14.7.2 session

- 定义:

  - 保存在**服务器**上的一组组键值对(session超时时间默认是14天)

- **为什么存在session:**

  - cookie保存在浏览器本地不安全
  - cookie数量大小都受到一定限制

- **session流程分析**(面试)

  ![1561533605263](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561533605263.png)

  - 在浏览器(用户)首次请求(登陆)时没有cookie, 服务器会根据用户信息设置一个session表,记录用户状态,值等相关数据;session表中有字段session_key,session_data,和超时时间exprie_data
  - 在服务器响应时,会把session表中的session_key以键值对session_id:xxxxxx的形式放到cookie中
  - 下次浏览器再次访问时,会携带含有session_id信息的cookie,然后服务器根据session_id去寻找该用户记录的session_data
  - 所以session不能单独存在,必须依赖cookie使用!,cookie起到中间媒介的作用.

- 运用

  - django默认把session放在一张数据表中
  - *在pycharm中把项目目录里的db.sqlite3拖到右侧数据库管理中,执行数据迁移命令python manage.py maration,生成表django session*

- 具体操作

  - expried过期

  ```python
  #设置session,看成字典的键值对
  request.session[key]  = value
  
  #获取session
  is_login = request.session.get(key)  
  
  #删除seession
  del request.session[key]
  request.session.pop(key)
  request.seddion.delete()  # 删除当前会话的所有Session数据,不会删除cookie
  
  # 删除当前的会话数据并删除会话的Cookie
  request.session.flush() 
      这用于确保前面的会话数据不可以再次被用户的浏览器访问
      例如，django.contrib.auth.logout() 函数中就会调用它。
      
  ----------------------------------------------------
  # 检查会话session的key在数据库中是否存在
  request.session.exists("session_key")
  
  #获取数据库中的session_key
  request.session.session_key
  
  # 将所有Session失效日期小于当前日期的数据删除
  request.session.clear_expired()
  
  # 设置会话Session和Cookie的超时时间
  request.session.set_expiry(value)
  	* 如果value是个整数，session会在些秒数后失效。
      * 如果value是个datatime或timedelta，session就会在这个时间后失效。
      * 如果value是0,用户关闭浏览器session就会失效。
      * 如果value是None,session会依赖全局session失效策略。
  ```

- django中的session配置(记住最好)

  ```python
  1. 数据库Session
  SESSION_ENGINE = 'django.contrib.sessions.backends.db'   # 引擎（默认）
  
  2. 缓存Session
  SESSION_ENGINE = 'django.contrib.sessions.backends.cache'  # 引擎
  SESSION_CACHE_ALIAS = 'default'                            # 使用的缓存别名（默认内存缓存，也可以是memcache），此处别名依赖缓存的设置
  
  3. 文件Session
  SESSION_ENGINE = 'django.contrib.sessions.backends.file'    # 引擎
  SESSION_FILE_PATH = None                                    # 缓存文件路径，如果为None，则使用tempfile模块获取一个临时地址tempfile.gettempdir() 
  
  4. 缓存+数据库
  SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'        # 引擎
  
  5. 加密Cookie Session
  SESSION_ENGINE = 'django.contrib.sessions.backends.signed_cookies'   # 引擎
  
  其他公用设置项：
  SESSION_COOKIE_NAME ＝ "sessionid"                       # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
  SESSION_COOKIE_PATH ＝ "/"                               # Session的cookie保存的路径（默认）
  SESSION_COOKIE_DOMAIN = None                             # Session的cookie保存的域名（默认）
  SESSION_COOKIE_SECURE = False                            # 是否Https传输cookie（默认）
  SESSION_COOKIE_HTTPONLY = True                           # 是否Session的cookie只支持http传输（默认）
  SESSION_COOKIE_AGE = 1209600                             # Session的cookie失效日期（2周）（默认）
  SESSION_EXPIRE_AT_BROWSER_CLOSE = False                  # 是否关闭浏览器使得Session过期（默认）
  SESSION_SAVE_EVERY_REQUEST = False                       # 是否每次请求都保存Session，默认修改之后才保存（默认）
  ```

  - from django.conf import global_settings
    - 配置都在全局配置中

**3.登陆示例**

```python
#登陆示例
class Login(View):
    def get(self, request, *args, **kwargs):
        return render(request, 'login.html')

    def post(self, request, *args, **kwargs):
        username = request.POST.get('username')
        pwd = request.POST.get('pwd')
        if username == 'alex' and pwd == '123':
            url = request.GET.get('return_url')
            if url:
                ret = redirect(url)
            else:
                ret = redirect('/index/')
            # ret['Set-Cookie'] = 'is_login=100; Path=/'
            # ret.set_cookie('is_login', '1')  # Set-Cookie: is_login=1; Path=/
            # ret.set_signed_cookie('is_login', '1', 's21',max_age=10000,)  # Set-Cookie: is_login=1; Path=/
            # 设置设session
            request.session['is_login'] = 1
            # request.session.set_expiry(0)
            return ret
        return render(request, 'login.html', {'error': '用户名或密码错误'})


def login_required(func):
    def inner(request, *args, **kwargs):
        # is_login = request.COOKIES.get('is_login')
        # is_login = request.get_signed_cookie('is_login', salt='s21', default='')
        is_login = request.session.get('is_login')
        print(is_login)
        url = request.path_info
        if is_login != 1:
            return redirect('/login/?return_url={}'.format(url))
        # 已经登录
        ret = func(request, *args, **kwargs)

        return ret

    return inner
```



### 14.8 django - - form组件

- **form组件的主要功能如下:**
  - 生成页面可用的HTML标签
  - 对用户提交的数据进行校验
  - 保留上次输入内容

##### 14.8.1.定义form组件

- 在views.py中

  - 先定义好一个RegForm类：

    ```python
    from django import forms
    
    # 按照Django form组件的要求自己写一个类
    class RegForm(forms.Form):
        name = forms.CharField(label="用户名")
        pwd = forms.CharField(label="密码")
    ```

  - 再写一个视图函数：

    ```python
    # 使用form组件实现注册方式
    def register2(request):
        form_obj = RegForm()
        if request.method == "POST":
            # 实例化form对象的时候，把post提交过来的数据直接传进去
            form_obj = RegForm(data=request.POST)
            # 调用form_obj校验数据的方法
            if form_obj.is_valid():
                return HttpResponse("注册成功")
        return render(request, "register2.html", {"form_obj": form_obj})
    
    
    
    ----------------------------------------------
    form_obj = RegForm()    
    form_obj = RegForm(request.POST)   # 实例化form对象的时候，把post提交过来的数据直接传进去
    form_obj.is_valid():  # 对数据进行校验
    print(form_obj.cleaned_data)  # 通过校验的数据
    
    return render(request, 'reg2.html', {'form_obj': form_obj})
    ----------------------------------------------
    ```

- 在html文件中

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>注册2</title>
  </head>
  <body>
      <form action="/reg2/" method="post" novalidate autocomplete="off">
          {% csrf_token %}
          <div>
              <label for="{{ form_obj.name.id_for_label }}">{{ form_obj.name.label }}</label>
              {{ form_obj.name }} {{ form_obj.name.errors.0 }}
          </div>
          <div>
              <label for="{{ form_obj.pwd.id_for_label }}">{{ form_obj.pwd.label }}</label>
              {{ form_obj.pwd }} {{ form_obj.pwd.errors.0 }}
          </div>
          <div>
              <input type="submit" class="btn btn-success" value="注册">
          </div>
      </form>
  </body>
  </html>
  
  
  ===================================================
  {{ form_obj.as_p }}         生产一个个P标签  input  label
  {{ form_obj.errors }}         form表单中所有字段的错误
  {{ form_obj.username }}       一个字段对应的input框
  {{ form_obj.username.label }}        该字段的中午提示
  {{ form_obj.username.id_for_label }}       该字段input框的id
  {{ form_obj.username.errors }}        该字段的所有的错误
  {{ form_obj.username.errors.0 }}      该字段的第一个错误的错误
  ========================================================
  
  ```

##### 14.8.2.常用字段

- CharField    字符串
- ChoiceField    单选框
- MultipleChoiceField    多选框

##### 14.8.3.常用字段参数

```python
error_messages  #重写错误信息
widget=forms.widgets.PasswordInput   #密码不明文显示
-----------------------------
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三",
        error_messages={
            "required": "不能为空",
            "invalid": "格式错误",
            "min_length": "用户名最短8位"
        }
    )
    pwd = forms.CharField(min_length=6, label="密码")
```

```python
widget=forms.widgets.Select()  #单选select
widget=forms.widgets.SelectMultiple()   #多选Select
widget=forms.widgets.CheckboxInput()  #单选checkbox
widget=forms.widgets.CheckboxSelectMultiple()   #多选checkbox
widget=forms.widgets.RadioSelect()   #列表选择

required=True,               #是否允许为空
widget=None,                 #HTML插件
label=None,                  #用于生成Label标签或显示内容
initial=None,                #初始值
error_messages=None,         #错误信息 {'required': '不能为空', 'invalid': '格式错误'}
validators=[],               #自定义验证规则
disabled=False,              #是否可以编辑
```

- **关于choice的注意事项：**!!!(面试)

  在使用选择标签时，需要注意choices的选项可以从数据库中获取，但是由于是静态字段 ***获取的值无法实时更新***，那么需要自定义构造方法从而达到此目的。

  - 方式一:

    - ```python
      from django import forms
      from django.forms import fields
      
      def __init__(self, *args, **kwargs):
          super(MyForm,self).__init__(*args, **kwargs)
          # self.fields['user'].choices = ((1, '上海'), (2, '北京'),)
          # 或
          self.fields['user'].choices = 		       	models.Classes.objects.all().values_list('id','caption')
      ```

  - **方式二:**

    - ```python
      from django import forms
      from django.forms import fields
      from django.forms import models as form_model
      
       
      class FInfo(forms.Form):
          authors = form_model.ModelMultipleChoiceField(queryset=models.NNewType.objects.all())  # 多选
          # authors = form_model.ModelChoiceField(queryset=models.NNewType.objects.all())  # 单选
      
      ```

##### 14.8.4.验证

**4.1内置校验**

- required=True  不能为空
- min_length=6
- max_length=7

**4.2 自定义校验器**

- 写函数

  ```python
  from django.core.exceptions import ValidationError
  
  def checkname(value):
      # 通过校验规则 不做任何操作
      # 不通过校验规则   抛出异常
      if 'alex' in value:
          raise ValidationError('不符合社会主义核心价值观')
          
          
  class LoginForm(forms.Form):
      username = forms.CharField(
          min_length=8,
          label="用户名",
          initial="张三",
          validators=[check_name, ],  #自定义函数校验
          error_messages={
              "required": "不能为空",
          }
      )
  ```

- 内置函数校验器

  - ```
    'RegexField', 'EmailField', 'FileField', 'ImageField', 'URLField',
    ```

  - ```python
    #示例
    from django.core.validators import RegexValidator
    
    class RegForm(forms.Form):
    	phone= forms.CharField(
            label='手机号码',
            validators=[RegexValidator(r"^1[3-9]\d{9}$",'手机号码格式不正确')]
        )
        email= forms.CharField(label='邮箱',validators=[EmailValidator('邮箱格式不正确')] )
    
    ```

4.3 **钩子函数**

- 局部钩子

  ```python
  from django.core.exceptions import ValidationError
  
  def clean_username(self):
      # 局部钩子
      # 通过校验规则  必须返回当前字段的值
      # 不通过校验规则   抛出异常
      v = self.cleaned_data.get('username')
      if 'alex' in v:
          raise ValidationError('不符合社会主义核心价值观')
          else:
              return v
  ```

- 全局钩子

  ```python
  def clean(self):
      # 全局钩子
      # 通过校验规则  必须返回当前所有字段的值
      # 不通过校验规则   抛出异常   '__all__'
      pwd = self.cleaned_data.get('pwd')
      re_pwd = self.cleaned_data.get('re_pwd')
  
      if pwd == re_pwd:
          return self.cleaned_data
      else:
          self.add_error('re_pwd','两次密码不一致!!!!!')  #自定义异常
          raise ValidationError('两次密码不一致')
  ```

##### 14.8.5.is_valid()校验函数校验流程

1. 执行full_clean()的方法：

   ​	-- 定义错误字典

   ​	-- 定义存放清洗数据的字典

2. 执行_clean_fields方法：

   ​	-- 循环所有的字段

   ​	-- 获取当前的值

   ​	-- 对进行校验 （ 内置校验规则   自定义校验器）

   - 通过校验

     -- self.cleaned_data[name] = value 

     -- 如果有局部钩子，执行进行校验：

     -- 通过校验——》 self.cleaned_data[name] = value 

     -- 不通过校验——》     self._errors  添加当前字段的错误 并且 self.cleaned_data中当前字段的值删除掉

   - 没有通过校验

     -- self._errors  添加当前字段的错误

3. 执行全局钩子clean方法



### 14.9 中间件middleware

参考博客https://www.cnblogs.com/maple-shaw/articles/9333824.html

- **中间件是处理django的请求和响应的框架级别的钩子,本质是一个类**(直白一点中间件是帮助我们在视图函数执行之前和执行之后都可以做一些额外的操作)

- **由于其影响的是全局，所以需要谨慎使用，使用不当会影响性能。**

- 定义的中间件需要注册

- django中请求响应流程

  ![1561798819879](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561798819879.png)

##### 14.9.1.中间件定义的五个方法:

- **process_request(self,request)**
- **process_response(self, request, response)**
- **process_view(self, request, view_func, view_args, view_kwargs)**
- process_exception(self, request, exception)
- process_template_response(self,request,response)

------最好全记住!!!!!!!!!!!!

###### 1 process_request

- process_request(self,request)

**特征**

```在视图函数之前执行的中间件方法按照注册顺序执行,在视图函数之后执行的中间件方法按照注册顺序倒序执行```

1. **执行时间:**  在执行视图函数之前执行
2. **参数:** request
   - request和视图函数中的的request是一个对象
3. **执行顺序:**
   - 按照注册的顺序进行执行
4. **返回值:**
   - 返回值为none的时候,执行顺序正常
   - 返回值如果是HttpResponse, 后面的中间件的process_request、视图函数都不执行，直接执行当前中间件中的process_response方法，再倒序执行之前的中间件中process_response方法。

###### 2 process_response

- process_response(self, request, response)

**特征**

```在视图函数之前执行的中间件方法按照注册顺序执行,在视图函数之后执行的中间件方法按照注册顺序倒序执行```

1. **执行时间:**  在执行视图函数之后执行
2. **参数:** request / response
   - request  和视图函数中的的request是一个对象
   - response   返回给浏览器响应对象(不一定是视图对象,peocess_request也会返回对象)
3. **执行顺序:**
   - 按照注册的顺序,倒序执行
4. **返回值:**
   - ​	HttpResponse：必须返回response对象

- process_request执行流程

  ![1561802908112](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561802908112.png)

###### 3 process_view

- process_view(self, request, view_func, view_args, view_kwargs)

**特征**

```在视图函数之前执行的中间件方法按照注册顺序执行,在视图函数之后执行的中间件方法按照注册顺序倒序执行```

1. **执行时间:**  视图函数之前，process_request之后
2. **参数:** request
   - request 和视图函数中的的request是一个对象
   - view_func  视图函数		
   - view_args   视图函数的位置参数		
   - view_kwargs    视图函数的关键字参数
3. **执行顺序:**
   - 按照注册的顺序进行执行
4. **返回值:**
   - 返回值为none的时候, 执行顺序正常
   - 返回值如果是HttpResponse,  后面的中间的process_view、视图函数都不执行，直接执行注册的最后一个中间件中的process_response方法，再倒叙执行之前的中间中process_response方法。

- 执行流程图

  ![1561802700654](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561802700654.png)

###### 4 process_exception

- process_exception(self, request, exception)

**特征**

```在视图函数之前执行的中间件方法按照注册顺序执行,在视图函数之后执行的中间件方法按照注册顺序倒序执行```

1. **执行时间(（触发条件）):**  视图函数之后,视图层面有错时才执行
2. **参数:** request/exception
   - request 和视图函数中的的request是一个对象
   - exception  视图中的错误对象
3. **执行顺序:**
   - 按照注册的顺序  倒叙执行
4. **返回值:**
   - 返回值为none的时候, 交给下一个中间件取处理异常，都没有处理交由django处理异常
   - 返回值如果是HttpResponse,   后面的中间件的process_exception不执行，直接执行最后一个中间件中的process_response方法，倒叙执行之前的中间中process_response方法。

- 流程图

  ![1561801490783](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561801490783.png)

###### 5 process_template_response

- process_template_response(self,request,response)

**特征**

```在视图函数之前执行的中间件方法按照注册顺序执行,在视图函数之后执行的中间件方法按照注册顺序倒序执行```

1. **执行时间(（触发条件）):**  视图函数之后,视图返回的是一个templateResponse对象(跟render用法类似)
2. **参数:** ,request,response
   - request  和视图函数中的的request是一个对象
   - response   templateResponse对象
3. **执行顺序:**
   - 按照注册的顺序  倒叙执行
4. **返回值:**
   - HttpResponse：必须返回response对象

- 流程图

  ![1561802461972](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561802461972.png)

##### 14.9.2.完整的django请求的生命周期

![1561862049120](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561862049120.png)

### 14.10 csrf 与csrf校验

##### 14.10.1 csrf

- CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（[XSS](https://baike.baidu.com/item/XSS)），但它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装成受信任用户的请求来利用受信任的网站。与[XSS](https://baike.baidu.com/item/XSS)攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比[XSS](https://baike.baidu.com/item/XSS)更具危险性。

  ```
  一个网站用户A可能正在浏览聊天论坛，而同时另一个用户B也在此论坛中，并且B刚刚发布了一个具有A银行链接的图片消息。设想一下，B编写了一个在A的银行站点上进行取款的form提交的链接，并将此链接作为图片src。如果A的银行在cookie中保存他的授权信息，并且此cookie没有过期，那么当A的浏览器尝试装载图片时将提交这个取款form和他的cookie，这样在没经A同意的情况下便授权了这次事务。
  ```

  

![1562051692624](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1562051692624.png)



##### 14.10.2 csrf校验

- csrf装饰器

  ```python
  from django.views.decorators.csrf import csrf_exempt,csrf_protect，ensure_csrf_cookie
  csrf_exempt   #某个视图不需要进行csrf校验,在CBV上使用时 必须加在dispath方法上
  csrf_protect  #某个视图需要进行csrf校验
  ensure_csrf_cookie  #确保生成csrf的cookie,在中间件被禁用时,可以生成csrf验证cookie
  ```

  - 某个视图不需要csrf校验保护

    ```python
    from django.views.decorators.csrf import csrf_exempt,  csrf_protect,  ensure_csrf_cookie
    from django.utils.decorators import method_decorator
    #FBV 直接加载函数上
    	@csrf_exempt
    	def func(request):
    #CBV 必须加在dispath方法上
    	@ method_decorator('csrf_exempt',name='dispath')
        class login(view):
            def get...
            def post...
    	
    ```

  - 某个装饰器需要csrf校验保护

    ```python
    #确保生成csrf校验的cookie
    可以加在函数上,也可以加在cbv的方法上
    ```

- **csrf功能(校验流程面试)**

  ```模板中的csrf_token是为了使form表单中生长一个隐藏标签,name=csrfmiddlewaretoken,以供校验```

  1. csrf中间件执行prosecc_request方法:

     - 从cookie中获取到csrftoken的值
     - 把从csrftoken的值放到request.META中,META['CSRF_COOKIE']=csrftoken

  2. 然后执行procss_view方法

     - 查看视图是否使用csrf_exempt装饰器,使用了就不再进行csrf校验

     - 判断请求方式:

       1.如果是GET', 'HEAD', 'OPTIONS', 'TRACE'不进行csrf校验

       2.如果是其他的请求方式(post，put)进行csrf校验

       - 首先获取cookie中csrftoken的值即META['CSRF_COOKIE'],

       - 再获取name=csrfmiddlewaretoken的值,即from表单中隐藏标签里的值,

         ----如果能获取到值-->赋值给request_csrf_token

         ----如果获取不到,就去请求头中寻找x-csrftoken的值,-->赋值给request_csrf_token

       - 最后比较上述request_csrf_token和cookie中csrftoken的值，比较成功接收请求，比较不成功拒绝请求。

       ```python
       #前端不验证不会生成隐藏标签和cookie,但是如果没有中间件,只会生成隐藏标签,不会生成cookie
       ```

     

##### 14.10.3 ajax的csrf校验

- 前提条件:确保有csrftoken的cookie

  - 在页面中使用{% csrf_token %}

  - 或者对视图函数加装饰器加装饰器 ensure_csrf_cookie

    from django.views.decorators.csrf import csrf_exempt,csrf_protect,  ensure_csrf_cookie

- 方式一: 给data中添加csrfmiddlewaretoken的值

  ```python
  data: {
      'csrfmiddlewaretoken':$('[name="csrfmiddlewaretoken"]').val(),
      a: $("[name='i1']").val(),
      b: $("[name='i2']").val(),
  },
  ```

- 方式二: 加请求头

  ```python
  headers:{
      'x-csrftoken':$('[name="csrfmiddlewaretoken"]').val(),
  },
  ```

- 方式三:使用js文件

  - 在每个html页面中引入一个jquery.cookie.js插件。下面的代码:

  ```
  function getCookie(name) {
      var cookieValue = null;
      if (document.cookie && document.cookie !== '') {
          var cookies = document.cookie.split(';');
          for (var i = 0; i < cookies.length; i++) {
              var cookie = jQuery.trim(cookies[i]);
              // Does this cookie string begin with the name we want?
              if (cookie.substring(0, name.length + 1) === (name + '=')) {
                  cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                  break;
              }
          }
      }
      return cookieValue;
  }
  var csrftoken = getCookie('csrftoken');
  
  
  function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
  }
  
  $.ajaxSetup({
    beforeSend: function (xhr, settings) {
      if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
        xhr.setRequestHeader("X-CSRFToken", csrftoken);
      }
    }
  });
  ```

  



### **14.11其他**

##### **0.ajax**

**----------------1 json** 

- 什么是 JSON ？
  - JSON 指的是 JavaScript 对象表示法（JavaScript Object Notation）
  - JSON 是轻量级的文本数据交换格式
  - JSON 独立于语言 *
  - JSON 具有自我描述性，更易理解

![1561604700736](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1561604700736.png)

- json模块是用于数据类型的序列换和反序列化
- json对象要求
  - 属性名必须使用双引号
  - 不能使用十六进制值
  - 不能使用undefined
  - 不能使用函数和日期对象
- XML和JSON都使用结构化方法来标记数据,在json出现之前,都是使用XML

**----------------2 ajax简介**

- AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步的Javascript和XML”
- **AJAX 不是新的编程语言，而是一种使用js技术发送请求和接收响应的新方法。**
- 特点:
  - 异步交互:客户端发出一个请求后，无需等待服务器响应结束，就可以发出 个请求。
  - 局部刷新
  - 请求和响应的传输的数据量少(局部刷新)

```python
#### 发请求的方式
1.地址栏输入地址  GET
2.form表单  GET/POST  
3.a标签   GET
4.ajax get/post
```

**----------------3 jqery发ajax请求**

```python
$.ajax({
    url: '/calc/',
    type: 'post',
    data: {
        a: $("[name='i1']").val(),
        b: $("[name='i2']").val(),
    },
    success: function (res) {       #res响应体
        $("[name='i3']").val(res)
    },
    error:function (error) {
        console.log(error)
    }
})
```

**----------------4 ajax参数**

data参数中的键值对，如果值值不为字符串，需要将其转换成字符串类型。

```html
$("#b1").on("click", function () {
    $.ajax({
      url:"/ajax_add/",
      type:"GET",
      data:{"i1":$("#i1").val(),"i2":$("#i2").val(),"hehe": JSON.stringify([1, 2, 3])},
      success:function (data) {
        $("#i3").val(data);
      }
    })
  })
```

**----------------5  jqery发ajax请求上传文件**

```html
<input type="file" id="f1">
<button id="b1">上传</button>


$('#b1').click(function () {
        var  formobj =  new FormData();

        formobj.append('file',document.getElementById('f1').files[0]);
        // formobj.append('file',$('#f1')[0].files[0]);
        formobj.append('name','alex');

        $.ajax({
            url: '/upload/',
            type: 'post',
            data:formobj ,
            processData:false,  // 数据处理
            contentType:false,	//数据处理
            success: function (res) {
                $("[name='i3']").val(res)
            },
        })
    })
```

**----------------6ajax的csrf校验**(面试)

- 前提条件:确保有csrftoken的cookie

  - 在页面中使用{% csrf_token %}

  - 或者对视图函数加装饰器加装饰器 ensure_csrf_cookie

    from django.views.decorators.csrf import csrf_exempt,csrf_protect,  ensure_csrf_cookie

- 方式一: 给data中添加csrfmiddlewaretoken的值

  ```python
  data: {
      'csrfmiddlewaretoken':$('[name="csrfmiddlewaretoken"]').val(),
      a: $("[name='i1']").val(),
      b: $("[name='i2']").val(),
  },
  ```

- 方式二: 加请求头

  ```python
  headers:{
      'x-csrftoken':$('[name="csrfmiddlewaretoken"]').val(),
  },
  ```

- 方式三:使用js文件

  - 在每个html页面中引入一个jquery.cookie.js插件。下面的代码:

  ```
  function getCookie(name) {
      var cookieValue = null;
      if (document.cookie && document.cookie !== '') {
          var cookies = document.cookie.split(';');
          for (var i = 0; i < cookies.length; i++) {
              var cookie = jQuery.trim(cookies[i]);
              // Does this cookie string begin with the name we want?
              if (cookie.substring(0, name.length + 1) === (name + '=')) {
                  cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                  break;
              }
          }
      }
      return cookieValue;
  }
  var csrftoken = getCookie('csrftoken');
  
  
  function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
  }
  
  $.ajaxSetup({
    beforeSend: function (xhr, settings) {
      if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
        xhr.setRequestHeader("X-CSRFToken", csrftoken);
      }
    }
  });
  ```

  



##### 1.事务

- trans 反式
- action  行为
- atomic 原子

```python
from django.db import transaction

try:  #try要写在with之外,不然with捕获不到错误信息
    with transaction.atomic():
        # 进行一系列的ORM操作
        models.Publisher.objects.create(name='xxxxx')
        models.Publisher.objects.create(name='xxx22')

except Exception as e :
    print(e)
```

##### 2.在Python脚本中调用Django环境

```python
import os

if __name__ == '__main__':
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "BMS.settings")
    import django
    django.setup()

    from app01 import models

    books = models.Book.objects.all()
    print(books)
```

##### 3.Django终端打印SQL语句

在Django项目的settings.py文件中，在最后复制粘贴如下代码：

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```

### 14.12 restframework使用

1. 下载安装

```python
pip install djangorestframework  ## djangorestframework
pip install django-filter  ## 过滤使用

```

2. 注册

```python
INSTALLED_APPS = [
	...
    'rest_framework',
    'django_filters'
]
```

3. model

```python
class Feedback(models.Model):
    type_choice = (
        (0, '未分类'),
        (1, '学习笔记'),
        (2, '学员评价'),
        (3, '入职邀约'),
    )

    img = models.ImageField(upload_to='img/feedback/')
    back_type = models.IntegerField(choices=type_choice, default=0)
```

4. 路由

```
url(r'^ajax_feedback/', views.AjaxFeedback.as_view()),
```

5. 视图

```python
from rest_framework import generics
from web.serializers import FeedbackSerializer
from web.pagination import DefaultPagination
from django_filters.rest_framework import DjangoFilterBackend


class AjaxFeedback(generics.ListAPIView):
    queryset = models.Feedback.objects.all()
    serializer_class = FeedbackSerializer
    pagination_class = DefaultPagination
    filter_backends = [DjangoFilterBackend, ]
    filter_fields = ['back_type']
```

6. 序列化器类

```python
from rest_framework import serializers
from repository import models


class FeedbackSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Feedback
        fields = ['img']

```

7. 分页类

```python
from rest_framework.pagination import PageNumberPagination

class DefaultPagination(PageNumberPagination):
    page_size = 8  # 一页多少条数据
    page_query_param = 'page'  # 分页查询条件的key
    page_size_query_param = 'size'
    # max_page_size = 8

```



### 14.13 RESTful 架构

##### 1.什么是RESTful 架构

- REST与技术无关，代表的是一种软件架构风格，REST是[Representational State Transfer](http://www.ruanyifeng.com/blog/2011/09/restful.html)的简称，中文翻译为“表征状态转移”
- 综合上面的解释，我们总结一下什么是RESTful架构：

　　（1）每一个URI代表一种资源；

　　（2）客户端和服务器之间，传递这种资源的某种表现层；

　　（3）客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表				现层状态转化"。

- get 获取资源
- post 新建资源
- put 时更新资源
- delete 删除资源

##### 2.RESTful API设计

- API可以看作是一个url, API与用户的通讯协议,总是使用HTTPs协议,比较安全

- 域名推荐书写方式:

  - https://api.example.com                         尽量将API部署在专用域名（会存在跨域问题）
  - https://example.org/api/                        API很简单(推荐使用)

- 路径，视网络上任何东西都是资源，均使用名词表示（可复数）

  - https://api.example.com/v1/zoos
  - https://api.example.com/v1/animals
  - https://api.example.com/v1/employees

- method

  - GET      ：从服务器取出资源（一项或多项）
  - POST    ：在服务器新建一个资源
  - PUT      ：在服务器更新资源（客户端提供改变后的完整资源）
  - PATCH  ：在服务器更新资源（客户端提供改变的属性）
  - DELETE ：从服务器删除资源

  ```python
  GET /collection：返回资源对象的列表（数组）
  GET /collection/resource：返回单个资源对象
  POST /collection：返回新生成的资源对象
  PUT /collection/resource：返回完整的资源对象
  PATCH /collection/resource：返回完整的资源对象
  DELETE /collection/resource：返回一个空文档
  ```

- 状态码

  ```python
  OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
  CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
  Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
  NO CONTENT - [DELETE]：用户删除数据成功。
  INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
  Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
  Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
  NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
  Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
  Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
  Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
  INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
  
  更多看这里：http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html
  ```

- 错误处理，状态码是4xx时，应返回错误信息，error当做key。

  ```
  `{``    ``error: ``"Invalid API key"``}`
  ```

- 返回结果，针对不同操作，服务器向用户返回的结果应该符合以下规范。

  ```
  GET /collection：返回资源对象的列表（数组）
  GET /collection/resource：返回单个资源对象
  POST /collection：返回新生成的资源对象
  PUT /collection/resource：返回完整的资源对象
  PATCH /collection/resource：返回完整的资源对象
  DELETE /collection/resource：返回一个空文档
  ```

- Hypermedia API，RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

  ```python
  {"link": {
    "rel":   "collection https://www.example.com/zoos",
    "href":  "https://api.example.com/zoos",
    "title": "List of zoos",
    "type":  "application/vnd.yourformat+json"
  }}
  ```

##### 3.序列化器serializer

`序列化: 将对象,字符串等数据类型转化为可用于传输/存储的数据形式`

- -----serializer文件-----

```python
from rest_framework import serializers
from app01 import models

# 获取外键名称
class PublisherSerializer(serializers.Serializer):
    name = serializers.CharField()

# 1.自定义序列化器获取manytomany外键名称
class AuthorSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    name = serializers.CharField()


class BookSerializer(serializers.Serializer):
    # 要序列化的书籍参数
    title = serializers.CharField()
    price = serializers.DecimalField(max_digits=6, decimal_places=2)
    pub_date = serializers.DateTimeField()
	# 仅仅在get需要的字段
    pub = PublisherSerializer(required=False, read_only=True) # 获取外键字段名称,自定义序列化器
    authors = serializers.SerializerMethodField(read_only=True)  # 获取多对多字段名称 1.自定义序列化器 2.自定义 get_字段名字段名
    
	# 仅仅在post需要的字段
    post_pub = serializers.IntegerField(write_only=True)
    post_author = serializers.ListField(write_only=True)
	
     # 2.自定义 get_字段名
    def get_authors(self, obj): # obj是书籍对象
        ser_obj = AuthorSerializer(obj.authors.all(), many=True)
        return ser_obj.data
    
    
	# 新建数据写入数据库时自定义create方法
    def create(self, validated_data):# validated_data校验过的数据
        # 写入数据
        book_obj = models.Book.objects.create(
            title=validated_data['title'],
            price=validated_data['price'],
            pub_date=validated_data['pub_date'],
            pub_id=validated_data['post_pub']
        )
        book_obj.authors.set(validated_data['post_author'])
        return book_obj
	
    
    # put方法执行到save方法是,执行update
    def update(self, instance, validated_data):
        instance.title = validated_data.get('title',instance.title)# 使用get请求方法防止在部分修改数据时报错
        instance.price = validated_data.get('price',instance.price)
        instance.pub_date = validated_data.get('pub_date',instance.pub_date)
        instance.pub_id = validated_data.get('post_pub',instance.pub_id)
        instance.save()
        instance.authors.set(validated_data.get('post_authors',instance.authors.all()))
        return instance
```

- -----view文件-----

```python
from app_rest import models

from rest_framework.views import APIView
from rest_framework.response import Response  # rest_framework返回对象
from app_rest.serializer import BookSerializer


class BookList(APIView):
    """查询新增"""

    # 获取数据
    def get(self, request, *args, **kwargs):
        all_books = models.Book.objects.all()
        ser_data = BookSerializer(all_books, many=True)  # many=True可以加入一个对象列表,单个对象不需要加入many参数
        return Response(ser_data.data)

    # 提交数据
    def post(self, request, *args, **kwargs):
        ser_obj = BookSerializer(data=request.data)
        if ser_obj.is_valid():
            ser_obj.save()
            return Response(ser_obj.data)
        return Response(ser_obj.errors)


class BookView(APIView):
    """修改删除"""

    def get(self, request, pk, *args, **kwargs):
        """查询到要修改的数据"""
        book_obj = models.Book.objects.filter(pk=pk).first()
        ser_obj = BookSerializer(instance=book_obj)
        return Response(ser_obj.data)

    def put(self, request, pk, *args, **kwargs):
        """
        提交要修改的数据
        :return:
        """
        book_obj = models.Book.objects.filter(pk=pk).first()
        ser_obj = BookSerializer(instance=book_obj, data=request.data, partial=True)  # partial=True 允许部分修改
        if ser_obj.is_valid():
            ser_obj.save()
            return Response(ser_obj.data)
        return Response(ser_obj.errors)

    def delete(self, request, pk, *args, **kwargs):
        """删除"""
        obj = models.Book.objects.filter(pk=pk).first()
        if obj:
            obj.delete()
            return Response({'msg': '删除成功'})
        return Response({'error': '数据不存在'})

```

- ---data.json---需要数据结构  

  ```python
  {
    "title": "桃花侠大战菊花怪1",
    "price": "111.11",
    "pub_date": "2019-01-01T10:10:10Z",
    "post_pub": 1,
    "post_author": [
      1,
      2
    ]
  }
  ```

  

##### 4. 序列化器钩子

- 自定义校验器

  ```python
  from rest_framework import serializers
  
  def validate_title(value):
      if '苍老师' in value:
          raise serializers.ValidationError
      return value
  
  class BookSerializer(serializers.Serializer):
      # 要序列化的书籍参数
      title = serializers.CharField(validators=[])
  
  ```

- **局部钩子与全局钩子**

  ```python
  class BookSerializer(serializers.Serializer):
      title = serializers.CharField()
      price = serializers.DecimalField(max_digits=6, decimal_places=2)
      pub_date = serializers.DateTimeField()
  	# 仅仅在get需要的字段
      pub = PublisherSerializer(required=False, read_only=True) # 获取外键字段名称,自定义序列化器
      authors = serializers.SerializerMethodField(read_only=True)  # 获取多对多字段名称 1.自定义序列化器 2.自定义 get_字段名字段名
      
  	# 仅仅在post需要的字段
      post_pub = serializers.IntegerField(write_only=True)
      post_author = serializers.ListField(write_only=True)
  	
       # 2.自定义 get_字段名
      def get_authors(self, obj): # obj是书籍对象
          ser_obj = AuthorSerializer(obj.authors.all(), many=True)
          return ser_obj.data
      
      
      # 局部钩子
      def validate_title(self,value):
          if '苍老师' in value:
              raise serializers.ValidationError
          return value
      
      # 全局钩子
      def validate(self,attrs):
          if '苍老师' in attrs:
              raise serializers.ValidationError
      	return attrs
  
  
  ```



##### 5.modelserializer

```python
from rest_framework import serializers

# 获取外键名称
class PublisherSerializer(serializers.Serializer):
    name = serializers.CharField()

# 1.自定义序列化器获取manytomany外键名称
class AuthorSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    name = serializers.CharField()



# 显示字段
class BookSerializer(serializers.modelSerializer):
    
    # 显示外键以及多对多字段的name
	pub_info = serializers.SerializerMethodField(read_only = True )
	authors_info = serializers.SerializerMethodField(read_only = True )
    
	def get_pub_info(self,obj):
        return PublisherSerializer(obj,pub).data
	
    def get_authors_info(self,obj):
        return AuthorSerializer(obj,author.all(),many = True).data
    
    
    class Meta:
        model = models.Book
        fields = '__all__'
        
        #给字段加限制
        extra_kwargs = {
            'pub':{'write_only':True},
            'authors':{'write_only':True}
        }

```

6.

## 第十五章 Linux

<http://linux.imock.club/>

## 第十六章 Flask 框架

### 16.1.Flask重点总结

```python
#Flask 安装依赖包及作用
- jinja2 模板语言  (flask依赖包)
- markupsafe   防止css攻击  (flask依赖包)
- werkzeug  --wkz 类似于django中的wsgi,承载服务  (flask依赖包)
```

#### 1.1Flask启动

```python
# 三行启动
from flask import Flask
app = Flask(__name__)
app.run("0.0.0.0",9527)  #监听地址,监听端口
```

#### 1.2 Response

1. return  "字符串"    -->httpresponse
2. return  render_template('html文件')     -->默认存储位置是remplates,返回模板,依赖markupsafe包
3. return redirect("/login")    -->重定向   在响应头中加入-location:url地址,然后跳转

- 特殊的Response

4. return  send_file("文件名称或者文件路径+文件名称")   -->打开文件,返回文件你内容,自动识别文件类型,在响应头中加入content-Type:文件类型
5. return jsonify({k:v})   -->返回标准格式的json字符串,在响应投中加入content-Type:appcation/json
   - 在Flask 1.1.1版本中,直接返回dict时,本质上是在运行jsonify
   - 在Flask1.1.1中,可以直接返回字典,flask默认先把字典序列化之后再返回,在content-type中加入application/json

#### 1.3 FLask Request

- request.from  获取FormData中的数据(类似Djnago中的request.POST)
  - to_dict() 转化为类似字典类型,可以通过get和[key]取值,[key],**当key不存在时,出现KeyError错误**
- request.args  获取url中的数据(类似Djnago中的request.GET)
  - to_dict() 转化为类似字典类型,可以通过get和[key]取值,[key],**当key不存在时,出现KeyError错误
- request.files  获取Fome中上传文件:
- **request.json**  
  - 如果请求中存在 Content-Type：application/json ,那么请求体中的数据 被序列化到 request.json 中,并且以 以字典的形式存放
- **request.data**
  - 如果请求中 存在Content-Type 中不包含 Form 或 FormData ,保留请求体中的原始数据,是base类型 b"  ".存放
- request.method  #请求方法
- request.host  #主机位: 127.0.0.1:5000
- request.url   #完整的url,包括参数
- request.path  #请求路径,路由地址
- request.cookies   #字典获取浏览器请求时带上的cookie
- request.files  # request文件,返回filestorage,.save保存文件
- request.heads  获取请求头

#### 1.4 Flask session

- 交由客户端保管机制,客户端保存的是一串加密字符串,保存在服务器的内存中

- 设置session

  ```python
  1.在Flask中设置密钥 
  app.secret_key = "@#^%&*&@^&%^#*@"
  或者:
  app.config['SECRET_KEY'] = "@#^%&*&@^&%^#*@"
  2.设置session
  session["user"]="123"
  3.获取session
  session.get('user')
  
  
  # 交由客户端保管机制
  1.登陆成功后设置session,Flask根据密钥和要设置的session键值对经过位运算,生成session,保存到内存中,需要的话就通过get获取,不需要关闭时删除,并在浏览器中设置键值对--session:string
  2.在发出请求时,根据浏览器中的--session:string和Flask中secret_key,反序列化就得到了session键值对
  ```

#### 1.5 Flask 路由 

```python
from flask import Flask  
app = Flask(__name__)  

@app.route('/',methods=("POST","GET"),endpoint='别名',defaults={"count":20} ) 
def home(count): 			
count = request.args.get('count',count)  #指定页数就优先,如果没有取默认值

app.run()
```

- rule= '/'   第一个时url,

- methods= ["GET","POST",..]   支持的请求方式(get查询,post增加,put更新,delete删除,options跨域)

- endpoint 别名   * 同一个项目中别名不能重复,默认值是视图函数名,

  ```
  Mapping,别名.
  反向解析  url = url_for('别名')
  ```

- strict_slashes  = True  严格遵守路由匹配

- defaluts  默认参数

- redirect_to  ='/'   永久重定向,不管url是什么都会跳转到'/'

  - 应用场景: 地址更换时,点击原来地址跳转到新地址

  ```python
  #添加路由时不一定用装饰器,可以使用 
  app.add_url_rule(rule,  # 路由地址
                   view_func  #视图函数
                  )
  ```

#### 1.6 动态参数路由

- 可以分页,获取文件,解决分类,解决正则路由问题

```python
from flask import Flask  
app = Flask(__name__)  

@app.route('/home/<page>')  #page默认类型是字符串接收 
def home(page):   # 需要传参接收			
    pass  

@app.route('/home/<page>_<id>_<id2>')  #默认类型是字符串接收 
def home(page,id,id2):   # 需要传参接收			
    pass 

app.run() 
```

#### 1.7 Flask初始化配置##重要!!!

```python
from flask import Flask  
app = Flask(
    __name__,
    template_folder = 'templates',   # 更改母版存放路径,默认值是templates  ##重要!!!
    static_folder = 'static',  # 指定静态文件保存目录,默认值是static    "家"  ##重要!!! 
    static_url_path = "/static",  # 静态文件访问路径,默认值是 /+static_folder  "回家的路"  ##重要!!!
    
)  

#原理
@app.route('/<filename>', )
def func(filename):
    filepath = os.path.join('img', filename)  # img就是家
    return send_file(filepath)  				# filepath就是访问路径


if __name__ == '__main__':
	app.run()
```

#### 1.8 Flask实例配置

- 基本配置

```python
from flask import Flask  
app = Flask(
    __name__,
    template_folder = 'templates',   # 更改母版存放路径,默认值是templates  ##重要!!!
    static_folder = 'static',  # 指定静态文件保存目录,默认值是static    "家"  ##重要!!! 
    static_url_path = "/static",  # 静态文件访问路径,默认值是 /+static_folder  "回家的路"  ##重要!!!
    
)  

# 实例化配置
app.debug = True # 修改代码自动重启
app.secret_key = '$%^^$'   #设置session值需要改密匙
app.session_cookie_name = 'session'  # 设置的session名称 ,默认是session
app.permanent_session_lifetime=  # session生命周期,以秒计时,默认31天

# 另外一种更改配置方式
app.config['DEBUG']= True  # 这种方式速度更快

#app.config   Flask配置
#app.defalut_config     flask默认配置

if __name__ == '__main__':
	app.run()
```

- **settings文件配置**(重要!!!),快速切换工作模式

```python
# settinigs.py文件代码

class DebugConfig(object):
    """线上开发环境"""
    DEBUG = True
    SECRET_KEY = "#$%^&*($#$%^&*%$#$%^&*^%$#$%"
    PERMANENT_SESSION_LIFETIME = 3600
    SESSION_COOKIE_NAME = "I am Not Session"


class TestConfig(object):
    """测试环境"""
    TESTING = True
    SECRET_KEY = hashlib.md5(f"{time.time()}#$%^&*($#$%^&*%$#$%^&*^%$#$%{time.time()}".encode("utf8")).hexdigest()
    PERMANENT_SESSION_LIFETIME = 360000
    SESSION_COOKIE_NAME = "#$%^&*($#$%^&*%$#$%^&*^%$#$%"   # session名字
    
    
    
#配置生效
1.导入配置文件
from settings import DebugConfig,TestConfig
2.环境生效
app.config.from_object(DebugConfig)  # 线上环境
app.config.from_object(TestConfig)	# test环境,需要的时候只需要启用DebugConfig,TestConfig其中一条
```

#### 1.9 Flask蓝图 Blueprint

- Blueprint,类似普通的Flask实例,不能被run的Flask实例,不存在config

1. 创建蓝图bp_users.py文件,名字可以更改

   ```python
   from flask import Blueprint
   
   bp = Blueprint('bp01', __name__,url_prefix='url前缀')  # 'bp01'第一个参数是唯一标识,整个环境不能重复!  url_prefix='url前缀',当存在多个蓝图url冲突时,在地址栏输入'url前缀',就可以访问指定的蓝图文件
   
   
   @bp.route('/user',endpoint='user')
   def user():
       return '我是蓝图函数01'
   ```

2. 建立关系

   ```python
   # 在项目的app.run文件中
   from app01 import bp  #导入蓝图
   app.register_blueprint(bp) # 注册蓝图
   ```

3. 访问指定路径

   - 在蓝图中反向解析时,需要注意书写格式:

     ```python
     url = url_for('蓝图标识.装饰器别名')
     ```

#### 1.10特殊装饰器

`类似于django中的中间件`

- @app.before_request   
  - 在请求进入视图函数之前,顺序执行,做出处理
  - 类似dganjo中的request中间件
  - 执行规律django中的request中间件一样
- @app.after_request
  - 在视图函数之后执行,倒序执行,做出处理
  - 类似django中的response中间件
  - 与django中间件不同的是:after_request不管什么情况,只要有响应都会倒序全部执行
- @app.errorhandler(错误码)
  - 错误监听装饰器
  - 错误码只能是4(请求错误)或5(服务器错误)开头的
  - 可以重定义错误界面

```python
@app.before_request
def is_login():
    """
    校验登录状态
    :return:
    """
    path = request.path
    if path != '/login':
        if not session.get('is_login'):
            return redirect('/login')
    return None


@app.errorhandler(404)
def error(error_msg):  # 形参必须添加
    """
    校验登录状态
    :return:
    """
    return '没找到页面'
```

#### 1.11CBV

- 基本格式

  ```python
  from flask import view,Flask
  app = Flask(__name__)
  
  class Login(views.MethodView):    #继承MethodView,使类变成视图函数
      def get(self,*args,**kwargs):
          pass
      def post(self,*args,**kwargs):
          pass
  app.add.url_rule('/login',
                  endpoint='login',  # 如果endpoint不定义的话名称就是view_func的name,必须唯一
                  view_func=Login.as_view(name='loginlogin'),  # name就是就是view_func的名称
                  )    
      
  app.run()
  ```

### 16.2.第三方组件Flask-Session

- setdefault()  字典的方法

  ```python
  dict.setdefault(key, default=None)
  #参数
  key -- 查找的键值。
  default -- 键不存在时，设置的默认键值。
  #返回值
  如果 key 在 字典中，返回对应的值。如果不在字典中，则插入 key 及设置的默认值 default，并返回 default ，default 默认值为 None。
  ```

  

- 安装Flask-session包

  - app.session_interface  就是默认的session接口,Flask利用session_interface选择session的存放位置和存放机制.

  ```python
  from flask import Flask, request, session
  from flask_session import Session  #导入Session
  from redis import Redis  # 导入redis
  
  app = Flask(__name__)
  app.secret_key = '$%^&*%$'  #  flask_session使用pickle转化,密钥可以不使用
  app.config['SESSION_TYPE'] = 'redis'  # 设置session存放机制,,浏览器中存的就是session_id,session存在redis中
  app.config['SESSION_REDIS'] = Redis(host='192.168.12.10', port=6379, db=10)
  Session(app)  # 使普通sesson变成flask_session
  
  # app.session_interface    #Flask利用app.session_interface 选择session存放位置和存放机制
  
  @app.route('/set')
  def sets():
      session['key'] = 'QWER'
      return 'set'
  
  @app.route('/get')
  def gets():
      return session.get('key')
  
  app.run()
  ```







### 16.3.Flask请求上下文管理

#### 3.1 偏函数

- partial   使用该方式可以生成一个新函数

  ```python
  from functools import partial
   
  def mod( n, m ):
    return n % m
   
  mod_by_100 = partial( mod, 100 )  # 100传给n
   
  print mod( 100, 7 )  # 2
  print mod_by_100( 7 )  # 2
  
  ```

#### 3.2 线程安全

```python
import time
from threading import local

class Foo(local): 
# 继承local,保证线程安全,也保证了处理速度,threading.local()这个方法的特点用来保存一个全局变量，但是这个全局变量只有在当前线程才能访问，
    num = 0
    
foo = Foo()
def add(i):
    foo.num =i
    time.sleep(0.5)
    print(foo.num)

from threading import Thread
for i in range(20):
    task = Thread(target=add,args=(i,))
    task.start()
    

```

##### 3.3 请求上文 

app.run()

##### 3.4 请求下文

request.method

- callable(local)  判断local是不是可执行对象

## 第十七章 MongoDB

- 文件型数据库

- 安装mongoDB 3.4

- 添加环境变量

  ```python
  MongoDB:
      使用了不存在的对象会创建该对象
      使用Json结构存储,类似字典
  ```

  ```python
  # mongodb的优缺点
  
  # MongoDB的优点：
  1、弱一致性（最终一致），更能保证用户的访问速度
  
  2、文档结构的存储方式，能够更便捷的获取数据
  对于一个层级式的数据结构来说，如果要将很多的数据使用扁平式的、表状的结构来保存数据，这不管是在查询还是获取数据时都会很困难。
  
  3、第三方支持丰富。
  这是它跟其它的NoSQL相比，MongoDB也具有的优势，因为现在网络上的很多NoSQL开源数据库完全属于社区型的，没有官方支持，给使用者带来了很大的风险。
  而开源的文档数据库MongoDB背后有商业公司为它提供商业培训和支持。
  
  4、性能优越
  在使用场合下，千万级别的文档对象，近10G的数据，对有索引的ID的查询不会比mysql慢，而对非索引字段的查询，则是全面胜出。 mysql实际无法胜任大数据量下任意字段的查询，而mongodb的查询性能可以，同时它的写入性能也很厉害，可以写入百万级别的数据。
  
  # MongoDB的缺点：
  
  1、MongoDB不支持事务操作
  所以事务要求严格的系统，比如银行系统就不能用它。
  
  2、MongoDB占用空间过大
  	2.1、空间的预分配:
  当MongoDB的空间不足时它就会申请生成一大块硬盘空间，而且申请的量都是有64M、128M、256M来增加直到2G为单个文件的较大体积，并且随着数量叠增，可以在数据目录下看到整块生成而且不断递增的文件。
  
  	2.2、删除记录不释放空间：
  这很容易理解，为避免记录删除后的数据的大规模挪动，原记录空间不删除，只标记“已删除”即可，以后还可以重复利用。
  
  3、（开发和IT运营要注意）MongoDB没有MySQL那样成熟的维护工具
  ```

  

### 1.启动

- mongod     cmd启动mongodb服务
- 监听端口   27017
- 数据库数据存放路径默认是c:/data/db
- mongod --dbpath="D:/mongodb-data/data/db"   更改数据存放位置,临时,永久生效需要更改配置文件

| MySQL            | MongoDB            |
| ---------------- | ------------------ |
| database  数据库 | database  数据库   |
| Tables  表       | Collection  表     |
| Colum  列,字段   | Field  列,字段     |
| Row  行,数据     | Documents  行,数据 |



### 2.连接mongodb

- mongo  启动mongodb客户端

### 3.基础命令

- show databases   查看当前服务器中的数据库
- use dbname   切换当前使用的数据库
- db   查看当前使用的数据库,也可以代之当前数据库名  
- show tables   查看当前数据内的表

```python
use dbname   # 如果数据库不存在,就在内存中创建数据库,当数据表中存在数据,数据库及数据表都会写入磁盘中
show databases  # 查看当前磁盘中的数据库
db(内存中).tablename  # 如果数据表不存在 在内存中创建数据表
show tables  # 查看当前数据库中磁盘上的数据表
```

#### 3.1 增

- db.tablename.insert({})   写入数据

  ```
  db.my_table.insert({name:"bigox"})
  ```

- 官方推荐 :

  - db.tablename.insertOne({name:"alexander.dsb.li"})     增加一条数据
  - db.tablename.insertMany([{name:"alexander.dsb.li"},{name:"alexander.dsb.li"}])       增加多条数据



#### 3.2 查

- db.tablename.find()   查找当前表内的所有数据

  ```
  - db.tablename.find({age:999,name:'bigox'})   查找当前表内所有条件符合 age=999且name=bigox 的所有数据
  - db.tablename.find({age:{"$gt":999}})   查找当前表内所有条件符合 age>999 的所有数据
  ```

- db.tablename.findOne({})   查询符合条件的第一条数据

  

  ```sql
  db.tablename.find({age:{"$gt":999}}) 
  
  $gt  大于
  $lt  小于
  $gte 大于等于
  $lte 小于等于
  $eq  等于
  $ne  不等于(不存在和不等于都是不等于)
  -------------------
  $and 并列:用法跟 db.tablename.find({age:999,name:'bigox'}) 一样
  	- db.tablename.find({"$and":[{age:999},{name:"alex"}])   
  	查找当前表内所有条件符合 age=999 且 name =alex 的所有数据
  	
  $or 或者
  	- db.tablename.find({"$or":[{age:999},{name:"alex"}])   
  	查找当前表内所有条件符合 age=999或者name =alex 的所有数据
  
  $in 包含/或者:同一字段或者条件
  	- db.tablename.find({"age":{"$in":[999,1000,998]}}) 
  	查找当前表内所有条件符合 age=999或者age =1000 或age=999 的所有数据
  
  $all 子集或者:查询条件必须是子集才能查到
  	- b.tablename.find({"hobby":{"$all":[1,2,3]}}) 
  	查找当前表内所有条件符合 hobby中包含有[1,2,3] 的所有数据
  
  ```

- object 查寻可以直接使用 对象.属性 的方式作为key

  - db.tablename.find({"student.age":999}) 

    ```
    当Array中出现了object会自动遍历
    ```

- 如果查询 _id 那么 数据的值必须带上 ObjectId 的数据类型

  ```
  db.Users.find({"_id":ObjectId("5d50e778b2a72712f5ee54c5")})
  ```

  

#### 3.3 改

- db.tablename.update({name:"alex"},{"$set":{name:DSB}})   修改符合查询条件name= alex 的**第一条数据**name = DS

- 官方推荐写法:

  - db.tablename.updateOne({},{})         修改符合条件的第一条数据
  - db.tablename.updateMany({},{})        修改符合条件的所有数据

  ```python
  $ 修改器
  $set  强制修改, 新修改的数据字段不存在自动创建
  $unset  强制删除某字段
  - db.tablename.update({name:"alex"},{"$unset":{age:True}})   
  	删除符合查询条件name= alex 的第一条数据age字段
  	
  $inc  引用增加(负数为减)
  - db.tablename.update({name:"alex"},{"$inc":{age:1}}) 
  	符合查询条件name= alex 的第一条数据age字段加1
  # 针对$array的修改器 以及 $关键字的用法和特殊性
  	Array是list 列表类型 
  	list -> append remove pop extends
  	Array -> $push $pull  $pop $pushAll $pullAll
  $push 在Array中追加数据 
  - db.users.update({name:"MJJ"},{"$push":{"hobby":"抽烟"}})
  	
  $pushAll 在Array中批量增加数据 [] == list.extends
  - db.users.update({name:"MJJ"},{"$pushAll":{"hobby":["喝酒","烫头"]}})
  	
  $pull 删除符合条件的所有元素
  - db.users.update({name:"MJJ"},{"$pull":{"hobby":"烫头"}})
  	
  $pop 删除array中第一条或最后一条数据
  	db.users.update({name:"MJJ"},{"$pop":{"hobby":1}}) 正整数为 删除最后一条数据
  	db.users.update({name:"MJJ"},{"$pop":{"hobby":-1}}) 负整数为 删除第一条数据
  	
  $pullAll 批量删除符合条件的元素 []
  	db.users.update({name:"MJJ"},{"$pullAll":{"hobby":[3,4,5]}})
  	
  $关键字 的特殊用法
  	储存符合条件的元素下标索引 - 只能存储一层遍历的索引位置
  $ = 9
  	db.users.update({"hobby":10},{"$set":{"hobby.$":0}})
  ```

#### 3.4 删

- db.tablename.remove({查询条件})    删除所有符合条件的所有数据,不写入条件时全部删除,**不建议使用**
- 官方推荐写法:
  - db.tablename.deleteOne({查询条件})	  删除符合条件的第一条数据
  - db.tablename.deleteMany({查询条件})     删除所有符合条件的数据

### 4.mongoDB数据类型

- 数据类型:
  - Object  ID ：Documents 自生成的 _id,**整个系统内的ID都不会重复**
  - String： 字符串，必须是utf-8
  - Boolean：布尔值，true 或者false (这里有坑哦~在我们大Python中 True False 首字母大写)
  - Integer：整数 (Int32 Int64 你们就知道有个Int就行了,一般我们用Int32),特殊指定才会使int,不然就是double
  - Double：浮点数 (没有float类型,所有小数在mongodb中都是Double)
  - Arrays：数组或者列表，多个值存储到一个键 (list哦,大Python中的List哦)
  - Object：Python中的字典,这个数据类型就是字典
  - Null：空数据类型 , 一个特殊的概念,None Null
  - Timestamp：时间戳
  - Date：存储当前日期或时间unix时间格式 (我们一般不用这个Date类型,时间戳可以秒杀一切时间类型)

### 5.高级函数

- 排序  sort

- 筛选  limit

- 跳过  skip

  ```python
  # 排序:
  	db.users.find({}).sort({age:-1}) 依照age字段进行倒序
  	db.users.find({}).sort({age:1}) 依照age字段进行正序
  # 筛选:
  	db.users.find({}).limit(4) 筛选前4条数据
  # 跳过:
  	db.users.find({}).skip(3) 跳过前3条数据 显示之后的所有数据
  	# 在mongo中的执行顺序
      	排序>跳过>筛选
      
      
  #应用---分页公式
  	1.排序 2.跳过 3.筛选
  	
  	page = 页码 = 1
  	count = 条目 = 2
  	#db.users.find({}).limit(2).skip(2).sort({ age:-1 })
  	db.users.find({}).limit(count).skip((page-1)*count).sort({ age:-1 })
  	
  ```

### 6.pymongo模块

- 安装pymongo模块

  ```python
  from pymongo import MongoClient
  from bson import ObjectId
  
  MC = MongoClient('127.0.0.1', 27017)  # 实例化MC
  MongoDB = MC['S21DAY93']  # 创建'S21DAY93'数据库
  
  # 插入数据
  # res = MongoDB.Users.insert({'name': 'YWB', "age": 999},{"name":"bigox","age":1})
  # print(res)
  # print(res.inserted_id, type(res.inserted_id))  # 5d511d22dd8f480d5e955987 <class 'bson.objectid.ObjectId'>
  
  # 查询数据
  # res = MongoDB.Users.find_one({"name": "YWB"})
  # res = MongoDB.Users.find({"name":"YWB"})
  
  # 删除数据
  # MongoDB.Users.delete_many({})
  # print(res)
  # res = MongoDB.Users.find_one({"_id": ObjectId("5d511d22dd8f480d5e955987")})  # 查询_id的时候必须加上ObjectId()
  
  # 修改数据
  # MongoDB.Users.update_many({},{"$inc":{"age":1}})
  
  # 高级函数
  
  # res = MongoDB.Users.find().sort("age",-1)
  # for i in res:
  #     print(i)
  ```

  - 查询_id的时候必须加上 ObjectId()

### 7.分词jieba模块

```python
import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # 全模式

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # 精确模式

seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
print(", ".join(seg_list))

seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式,建议使用,但是速度较慢
print(", ".join(seg_list))
```

### 8.转拼音pypiyin模块

- lazy_pinyin 
- TONE,TONE2,TONE3  转换模式

```python
from pypinyin import lazy_pinyin,TONE,TONE2,TONE3

a = "我要给圆圆发消息"
#换成字母
az = "woyaogeiyuanyuanfaxiaoxi"

b = "我要给元元发消息"
#换成字母
bz = "woyaogeiyuanyuanfaxiaoxi"

c = "我要给苑苑发消息"
cz = "woyaogeiyuanyuanfaxiaoxi"

resa = lazy_pinyin(a,style=TONE)
print(resa)  # ['wǒ', 'yào', 'gěi', 'yuán', 'yuán', 'fā', 'xiāo', 'xī']
resb = lazy_pinyin(b,style=TONE2)
print(resb)  #['wo3', 'ya4o', 'ge3i', 'yua2n', 'yua2n', 'fa1', 'xia1o', 'xi1']
resc = lazy_pinyin(c,style=TONE3)
print(resc)  #['wo3', 'yao4', 'gei3', 'yuan4', 'yuan4', 'fa1', 'xiao1', 'xi1']

print("".join(resc))  # 列表转化字符串
```

### 9.机器学习综合库gensim模块

- 使用步骤:

```python
# 1.模块导入
import jieba
import gensim
from gensim import corpora
from gensim import models
from gensim import similarities


# 2.制作问题库 

l1 = ["你的名字是什么", "你今年几岁了", "你有多高你胸多大", "你胸多大"]  # 问题库

# 3.对问题样本和问题库分词处理

a = "你今年多大了"  # 问题样本
all_doc_list = []
for doc in l1:
    doc_list = [word for word in jieba.cut(doc)]
    all_doc_list.append(doc_list)
doc_test_list = [word for word in jieba.cut(a)]
print(all_doc_list)
print(doc_test_list)


# 4.制作语料库

dictionary = corpora.Dictionary(all_doc_list)  # 制作词袋
# 词袋的理解
# 词袋就是将很多很多的词,进行排列形成一个 词(key) 与一个 标志位(value) 的字典
# 例如: {'什么': 0, '你': 1, '名字': 2, '是': 3, '的': 4, '了': 5, '今年': 6, '几岁': 7, '多': 8, '有': 9, '胸多大': 10, '高': 11}
# 至于它是做什么用的,带着问题往下看
print("token2id", dictionary.token2id)
print("dictionary", dictionary, type(dictionary))

# -->问题库的语料库
corpus = [dictionary.doc2bow(doc) for doc in all_doc_list]
# 语料库:
# 这里是将all_doc_list 中的每一个列表中的词语 与 dictionary 中的Key进行匹配
# 得到一个匹配后的结果,例如['你', '今年', '几岁', '了']
# 就可以得到 [(1, 1), (5, 1), (6, 1), (7, 1)]
# 1代表的的是 你 1代表出现一次, 5代表的是 了  1代表出现了一次, 以此类推 6 = 今年 , 7 = 几岁
print("corpus", corpus, type(corpus))

# -->问题的语料库
# 将需要寻找相似度的分词列表 做成 语料库 doc_test_vec
doc_test_vec = dictionary.doc2bow(doc_test_list)
print("doc_test_vec", doc_test_vec, type(doc_test_vec))

# 5. 将corpus语料库(初识语料库) 使用Lsi模型进行训练

lsi = models.LsiModel(corpus)
# 模型有很多,这里的只是需要学习Lsi模型来了解的,这里不做阐述
print("lsi", lsi, type(lsi))
# 语料库corpus的训练结果
print("lsi[corpus]", lsi[corpus])
# 获得语料库doc_test_vec 在 语料库corpus的训练结果 中的 向量表示
print("lsi[doc_test_vec]", lsi[doc_test_vec])

# 6. 获取文本相似度

# 稀疏矩阵相似度 将 主 语料库corpus的训练结果 作为初始值
index = similarities.SparseMatrixSimilarity(lsi[corpus], num_features=len(dictionary.keys()))
print("index", index, type(index))

# 将 语料库doc_test_vec 在 语料库corpus的训练结果 中的 向量表示 与 语料库corpus的 向量表示 做矩阵相似度计算
sim = index[lsi[doc_test_vec]]
print("sim", sim, type(sim))

# 7. 获取相似度最高的结果
# 对下标和相似度结果进行一个排序,拿出相似度最高的结果
# cc = sorted(enumerate(sim), key=lambda item: item[1],reverse=True)
cc = sorted(enumerate(sim), key=lambda item: -item[1])
print(cc)

text = l1[cc[0][0]]
print(a,text)

```



## 第十八章 redis

- redis默认端口   6379
- mysql默认端口   3306

### 1 redis 配置

- 下载redis 
- 配置环境变量
- 命令行启动redis
  - redis-server  开启redis服务  
- 命令行开启redis客户端
  - redis-cli

### 2 redis 基本指令

- set  key  value  设置数据,一个键值对(哈希存储结构)
- get  key  查询当前数据库中对应的vlaue值
- keys  '查询的key'    查询key
  - keys  *  查询当前数据库中所有的key
  - keys a*   查询当前数据库中所有以a的key
  - keys  * n *    查询当前数据库中所有含有n的key
  - keys   n*s   n开头s结尾的key
- select  #  切换到#号库,#是数据库编号,数据库0-15,为了数据隔离

### 3 python操作redis

1. 创建操作py文件

   - 安装模块redis

   ```python
   from redis import Redis
   
   redis_cli = Redis(host = '192.168.12.87',port=6379,db=6)  
   
   redis_cli.set('name','S21')  # 设置值
   
   res = redis_cli.get('name')  # 获取值,字节类型的
   print(res.decode('utf8'))   #转化为字符串
   ```



## 附录:

## py2与py3的区别:

| 序号 |            不同点             |                       python2                       |             python3             |
| ---- | :---------------------------: | :-------------------------------------------------: | :-----------------------------: |
| 1    |        字符串类型不同         |                字符串 unicode    str                |     字符串str		bytes      |
| 2    |         输入格式不同          |                     raw_input()                     |             input()             |
| 3    |         输出格式不同          |                      print ""                       |             print()             |
| 4    |          整型类型int          |                      int和long                      |             只有int             |
| 5    |           整型除法            |                    只取整数部分                     |            取全部值             |
| 6    |        rang/Xrang不同         | rang立即生成列表,<br>xrang在使用时生成,边使用边生成 | range在使用时生成,边使用边生成  |
| 7    |         模块和包不同          |         创建模块时会自动生成__ init __文件          |          不会,建议添加          |
| 8    |      map/filter输出不同       |                      返回列表                       |     返回迭代器,不循环不生成     |
| 9    | 字典keys/values/items数据类型 |                    返回值是列表                     | 返回迭代器,可循环但是不可以索引 |

## 常用端口号

```python
ssh 22
http 80
https 443
mysql 3306
redis 6379
mongdb 27017
oracle数据库 1521
window远程桌面: 3389
ftp默认端口:  21/20
```

