# 数据分析

- 数据分析：是把隐藏在一些看似杂乱无章的数据背后的信息提炼出来，总结出所研究对象的内在规律
- 数据分析三剑客：Numpy,Pandas,Matplotlib

## 1. Numpy

- NumPy(Numerical Python) 是 Python 语言的一个扩展程序库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。

### 1.1 创建 ndarray

1. 使用np.array()创建

   ```python
   import numpy as np
   # 一维数据创建
   ret = np.array([1, 2, 3])
   # 二维数据创建
   ret = np.array([[1, 2, 3], [4, 5, 6]])
   print(ret)
   ```

   - **numpy默认ndarray的所有元素的类型是相同的**

   - 如果传进来的列表中包含不同的类型，则统一为同一类型，优先级：str>float>int

   - 使用matplotlib.pyplot获取一个numpy数组，数据来源于一张图片

     ```python
     import matplotlib.pylab as plt
     # 图片数据转化为数组
     img_arr = plt.imread('./cat.jpg')
     # 数组转图片
     img_show = plt.imshow(img_arr)
     # 操作该numpy数据，该操作会同步到图片中
     plt.imshow(img_arr-100)
     ```

2. 使用np的routines函数创建

   -  np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None) 等差数列

     ```python
     np.linspace(0,100,num=20)
     ```

   - np.arange([start, ]stop, [step, ]dtype=None)

     ```python
     np.arange(0,100,2)
     ```

   - np.random.randint(low, high=None, size=None, dtype='l') 随机生成

     ```python
     np.random.seed(100) #固定随机性#随机因子：系统的时间
     arr = np.random.randint(0,100,size=(4,5)) #size 4行5列
     ```

   - np.random.random(size=None)

     生成0到1的随机数，左闭右开 np.random.seed(3)

     ```python
     np.random.random(size=(4,5))  # 4行5列
     ```

### 1.2 ndarray的属性

- 4个必记参数： ndim：维度 shape：形状（各维度的长度） size：总长度；dtype：元素类型

  ```python
  arr = np.random.randint(0, 100, size=(4, 5))
  arr.ndim
  arr.shape
  ...
  ```


### 1.3 ndarray的基本操作

- 索引

  - 一维与列表完全一致 多维时同理

- 切片

  - 一维与列表完全一致 多维时同理

  ```python
  np.random.seed(100)  # 固定随机性#随机因子：系统的时间
  arr = np.random.randint(0, 100, size=(4, 5))
  #获取二维数组前两行
  arr[0:2]
  #获取二维数组前两列
  arr[:,0:2]
  # 获取二维数组前两行和前两列数据
  arr[0:2,0:2]
  # 将数组的行倒序
  arr[::-1]
  #列倒序
  arr[:,::-1]
  #全部倒序
  arr[::-1,::-1]
  # 图片倒置,裁剪
  plt.imshow(img_arr[:,::-1,:])
  ```

- 变形

  - 使用arr.reshape()函数，注意参数是一个tuple！

  - 将一维数组变形成多维数组  arr_1.reshape((2,10))

    ```python
    arr_1 = np.random.randint(0, 100, size=(1,20))
    print(arr_1)
    print(arr_1.reshape((2, 10)))
    '''[[ 9 93 86  2 27  4 31  1 13 83  4 91 59 67  7 49 47 65 61 14]]
    [[ 9 93 86  2 27  4 31  1 13 83]
     [ 4 91 59 67  7 49 47 65 61 14]]
    '''
    ```

  - 将多维数组变形成一维数组 arr.reshape((20,))

- 级联

  - np.concatenate()

  - 一维，二维，多维数组的级联，实际操作中级联多为二维数组

  - np.concatenate((arr,arr,arr),axis=1)

    ```python
    array([[ 8, 24, 67, 87, 79],
           [48, 10, 94, 52, 98],
           [53, 66, 98, 14, 34],
           [24, 15, 60, 58, 16]])
    np.concatenate((arr,arr,arr),axis=1)
    array([[ 8, 24, 67, 87, 79,  8, 24, 67, 87, 79,  8, 24, 67, 87, 79],
           [48, 10, 94, 52, 98, 48, 10, 94, 52, 98, 48, 10, 94, 52, 98],
           [53, 66, 98, 14, 34, 53, 66, 98, 14, 34, 53, 66, 98, 14, 34],
           [24, 15, 60, 58, 16, 24, 15, 60, 58, 16, 24, 15, 60, 58, 16]])
    ```

  - 应用,合并参数一致的图片

    ```python
    img_3 = np.concatenate((img_arr,img_arr,img_arr),axis=1)
    img_9 = np.concatenate((img_3,img_3,img_3),axis=0)
    plt.imshow(img_9)
    ```

  - 级联的参数是列表：一定要加中括号或小括号
  - 维度必须相同
  - 形状相符: 在维度保持一致的前提下，如果进行**横向（axis=1）级联**，必须保证进行级联的数组行数保持一致。如果进行**纵向（axis=0）级联**，必须保证进行级联的数组列数保持一致。
  - 可通过axis参数改变级联的方向

### 1.4 ndarray的聚合操作

- 求和np.sum

  ```python
  arr.sum(axis=1)  # 横向（axis=1）级联,纵向（axis=0）级联
  ```

-  最大最小值：np.max/ np.min

- 平均值：np.mean()

- 其他聚合操作

### 1.5 ndarray 的排序

np.sort()与ndarray.sort()都可以，但有区别：

- np.sort()不改变输入
- ndarray.sort()本地处理，不占用空间，但改变输入

```
np.sort(arr,axis=0)
```



## 2. Pandas

- 清洗数据：（重复值、异常值和空值）

- pandas需要导入

  ```python
  import pandas as pd
  from pandas import Series,DataFrame
  import numpy as np
  ```

### 2.1 Series

- Series是一种类似与一维数组的对象，由下面两个部分组成：
  - values：一组数据（ndarray类型）
  - index：相关的数据索引标签

- Series的创建:默认索引为0到N-1的整数型索引

  1. 由列表创建

  2. 由numpy数组创建

     ```python
     #使用列表创建Series
     Series(data=[1,2,3])
     Series(data=[1,2,3],index=['a','b','c']) #显式索引,显示索引不会覆盖隐式索引
     
     #使用numpy创建Series
     s = Series(data=np.random.randint(0,100,size=(4,)),index=['a','b','c','d'])
     
     ```

#### 2.2 Series的索引

- 可以使用中括号取单个索引（此时返回的是元素类型），或者中括号里一个列表取多个索引（此时返回的是一个Series类型）。

- 显式索引：

  - 使用index中的元素作为索引值

  - 使用s.loc[]（推荐）:注意，loc中括号中放置的一定是显示索引

    注意，此时是闭区间

    ```python
    s[[1,2]]--->
    b    92
    c    94
    dtype: int32
    ```

- 隐式索引：

  - 使用整数作为索引值

  - 使用.iloc[]（推荐）:iloc中的中括号中必须放置隐式索引

    注意，此时是半开区间

- 切片:隐式索引切片和显示索引切片
  
  - 显示索引切片:index和loc

#### 2.3 Series 的属性

- 可以通过shape，size，index,values等得到series的属性

- 可以使用s.head(),tail()分别查看前n个和后n个值

- .unique()对Series元素进行去重

  ```python
  s = Series(data=[1,1,2,2,3,3,3,4,5,6,7,7,8,9,9,9])
  s.unique()
  ```

- 当索引没有对应的值时，可能出现缺失数据显示NaN（not a number）的情况

  - 使得两个Series进行相加

    ```python
    s1 = Series(data=[1,2,3],index=['a','b','c'])
    s2 = Series(data=[1,2,3],index=['a','b','d'])
    s = s1 + s2
    s  --->
    
    a    2.0
    b    4.0
    c    NaN
    d    NaN
    dtype: float64
    ```

  - 可以使用pd.isnull()，pd.notnull()，或s.isnull(),notnull()函数检测缺失数据,返回值是bool类型

#### 2.4 Series的运算

- `+ - * /`add() sub() mul() div() : s1.add(s2,fill_value=0)

```python
s1 = Series(data=np.random.randint(0, 10, size=(4,)), index=['a', 'b', 'c', 'd'])
s2 = Series(data=np.random.randint(0, 10, size=(4,)), index=['a', 'b', 'e', 'f'])
print(s1,s2,s1.add(s2))
```

-  Series之间的运算
  - 在运算中自动对齐不同索引的数据
  - 如果索引不对应，则补NaN

### 2.5 DataFrame

- DataFrame是一个【表格型】的数据结构。DataFrame由按一定顺序排列的多列数据组成。设计初衷是将Series的使用场景从一维拓展到多维。DataFrame既有行索引，也有列索引。
  - 行索引：index
  - 列索引：columns
  - 值：values

#### 2.6 DataFrame的创建

- 以字典来创建。

  - DataFrame以字典的键作为每一【列】的名称，以字典的值（一个数组）作为每一列。
  - DataFrame会自动加上每一行的索引。
  - 用字典创建的DataFrame后，则columns参数将不可被使用。
  - 若传入的列与字典的键不匹配，则相应的值为NaN。

  ```python
  from pandas import Series, DataFrame
  d = DataFrame(data={"name": "bigox"},index=[1,2])
  print(d)
  ```

- 使用ndarray创建DataFrame

  ```python
  DataFrame(data=np.random.randint(0,100,size=(3,4)),index=['a','b','c'])
  --->
  	0	1	2	3
  a	77	43	59	44
  b	2	93	60	74
  c	63	6	39	66
  ```

#### 2.7 DataFrame属性

- values、columns、index、shape

  ```python
  # 使用字典创建DataFrame：创建一个表格用于展示张三，李四，王五的成绩
  dic = {
      '张三':[11,22,33,44],
      '李四':[55,66,77,88]
  }
  df_score = DataFrame(data=dic,index=['语文','数学','英语','理综'])
  ```

#### 2.8 DataFrame索引

1. 对列进行索引

   - 通过类似字典的方式  df['q']

   - 通过属性的方式     df.q

     - 将DataFrame的列获取为一个Series。返回的Series拥有原DataFrame相同的索引，且name属性也已经设置好了，就是相应的列名。

     ```python
     #获取前两列
     df[['A','B']]
     ```

     

2. 对行进行索引

   - 使用.loc[]加index来进行行索引

   - 使用.iloc[]加整数来进行列索引

     - 同样返回一个Series，index为原来的columns。

     ```python
     df.iloc[[0,1]]
     df.loc[['a','b']]
     ```

3. 对元素索引的方法

   - 使用列索引
   - 使用行索引(iloc[3,1] or loc['C','q']) 行索引在前，列索引在后

   ```python
   df.loc['a','D']
   df.loc[['a','b'],'D']
   ```
   
   ![1567644552710](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567644552710.png)

#### 2.9 DataFrame切片

【注意】 直接用中括号时：

- 索引表示的是列索引
- 切片表示的是行切片

```python
#切出前两行
df['a':'b']  # a,b是行索引
df[0:2]
```

- 在loc和iloc中使用切片(切列) ： df.loc['B':'C','丙':'丁']

  ```python
  df = DataFrame(data=np.random.randint(0,100,size=(3,4)),index=['a','b','c'],columns=['A','B','C','D'])
  df.loc[:,'A':'B']
  --->
  	A	B
  a	61	87
  b	59	52
  c	98	17
  ```

![1567644600049](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567644600049.png)

#### 2.10 DataFrame的运算

1.  DataFrame之间的运算

- 同Series一样：
  - 在运算中自动对齐不同索引的数据
  - 如果索引不对应，则补NaN

2. 其他运算

   ```python
   #删除df中的某一列
   df.drop(labels='Unnamed: 0',axis=1,inplace=True)
   
   #将df总的date这一列作为源数据的行索引,将字符串i形式的时间数据转换成时间类型
   df = pd.read_csv('./maotai.csv',index_col='date',parse_dates=['date'])
   
   #一旦遇到了一组布尔值，直接将布尔值作为源数据的行索引
   df.loc[(df['close'] - df['open']) / df['open'] > 0.03]
   
   #将买股票对应的行数据找出
       #- 在new_df中将每个月的第一个交易日的行数据取出
       #- 将每一行中的open值取出
   df_monthly = new_df.resample('M').first()  # M月
   df_yearly = new_df.resample('A').last()[:-1]  # A年
   ```

   

### 2.11 处理丢失数据

1. None 是python中自带的,其类型为python object为空,不能参与计算

2. NaN 是一个**浮点类型**的数据,能参与计算.计算**结果总是NaN**

3. pandas中的None和NaN
   
   - pandas中会把None等为空的转化为NaN类型
   
4. pandas 处理空值操作
   - isnull()  :为空的返回True
   - notnull()  :为空的返回False
   - dropna() :  过滤丢失数据
     - 可以选择过滤的是行还是列（默认为行）:axis中0表示行，1表示的列
   - fillna() : 覆盖该空值
     - `fillna()`:value和method参数
       - df.fillna(method='bfill',axis=0) # 以后面行的数据填充
       - df.fillna(method='ffill',axis=1) # 以前面列的数据填充
   
   ```python
   #固定搭配
   isnull=》any  有空时返回True
   notnull=》all  全不为空的时候返回True
   #结论：将df.notnull().all(axis=1)作为源数据的行索引，就可以将空对应的行删除
   df.loc[df.notnull().all(axis=1)] 
   
   indexs = df.loc[df.isnull().any(axis=1)].index #获取的是值对应行的行索引
   df.drop(labels=indexs,axis=0)
   
   #dropna() : 可以选择过滤的是行还是列（默认为行）:axis中0表示行，1表示的列
   df.dropna(axis=0)
   
   df.fillna(method='bfill',axis=0) # 以后面行的数据填充
   df.fillna(method='ffill',axis=1) # 以前面列的数据填充
   ```

### 2.12 pandas的拼接操作

- pandas的拼接分为两种：
  - 级联：pd.concat, pd.append
  - 合并：pd.merge, pd.join

1. **pd.concat 级联**

   - pandas使用pd.concat函数，与np.concatenate函数类似，只是多了一些参数：
     ```python
     objs
     axis=0
     keys
     join='outer' / 'inner':表示的是级联的方式，outer会将所有的项进行级联（忽略匹配和不匹配），而inner只会将匹配的项级联到一起，不匹配的不级联
     ignore_index=False
     ```

   - 匹配级联:简单连接

   - 不匹配级联:

     ```python
     # 不匹配指的是级联的维度的索引不一致。例如纵向级联时列索引不一致，横向级联时行索引不一致
     有2种连接方式：
     	外连接：补NaN（默认模式）
     	内连接：只连接匹配的项
         
     df1 = DataFrame(data=np.random.randint(0,100,size=(3,3)),index=['A','B','C'],columns=['a','b','c'])
     df2 = DataFrame(data=np.random.randint(0,100,size=(3,3)),index=['A','B','D'],columns=['a','b','d'])
     pd.concat((df1,df1),axis=0) # 匹配级联    
     pd.concat((df1,df2),axis=1,join='inner')  # 不匹配级联 
     ```

2. **pd.merge()合并**

- merge与concat的区别在于，merge需要**依据某一共同的列来进行合并**

  使用pd.merge()合并时，会自动根据两者相同column名称的那一列，作为key来进行合并。

  注意每一列元素的顺序不要求一致

- 参数：
  - how：out取并集 inner取交集
  - on：当有多列相同的时候，可以使用on来指定使用那一列进行合并，on的值为一个列表

- **一对一合并**

  - column名称以及其元素都相同的列进行合并,不要求子元素顺序相同

- **多对一合并**

  - 多对一合并时会自动匹配的项,

  ![1567669420459](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567669420459.png)

- **多对多合并**

  - 内合并(不同的项丢弃,只保留两者都有的key（默认模式）)

  - 外连接(不同的项填充NaN,外合并 how='outer'：补NaN)

  - 左连接(保证左边Dataframe的数据完整性)

  - 右连接(保证右边Dataframe的数据完整性)

    ```python
    merge(df1,df2,how="inner",on="group")
    ```

- key的规范化

  - 当列冲突时，即有多个列名称相同时，需要使用on=来指定哪一个列作为key，配合suffixes指定冲突列名

  - 当两张表没有可进行连接的列时，可使用left_on和right_on手动指定merge中左右两边的哪一列列作为连接的列

    ```python
    df1 = DataFrame({'employee':['Bobs','Linda','Bill'],
                    'group':['Accounting','Product','Marketing'],
                   'hire_date':[1998,2017,2018]})
    
    df5 = DataFrame({'name':['Lisa','Bobs','Bill'],
                    'hire_dates':[1998,2016,2007]})
    
    pd.merge(df1,df5,left_on='employee',right_on='name')
    ```

### 2.13 pandas 高级数据处理

1. #### 删除重复元素

   - duplicated() 函数检测重复的行,返回元素为布尔类型的Series对象，每个元素对应一行，如果该行不是第一次出现，则元素为True

     - keep 参数,指定保留的数据
       - first 保留第一行数据
       - last 保留最后一行数据
       - False 不保留重复数据

   - drop_duplicates() 函数删除重复的行

     - drop_duplicates(keep='first/last'/False)

     ```python
     df.drop_duplicates(keep='first')
     ```

2. #### 映射

- **replace() 函数**:替换元素,使用replace()函数，对values进行映射操作

- DataFrame替换操作

  - 单值替换

    - 普通替换： 替换所有符合要求的元素:to_replace=15,value='e'
    - 按列指定单值替换： to_replace={**列标签**：替换值} value='value'

    ```python
    df.replace(to_replace=5,value='five')  # 把值为5的元素值替换为five
    ```

  - 多值替换

    - 列表替换: to_replace=[] value=[]
    - 字典替换（推荐） to_replace={to_replace:value,to_replace:value}

    ```python
    df.replace(to_replace={88:'8888'}) # 替换88为 8888
    df.replace(to_replace={3:6},value='six') # 替索引为3的列中的6 为six
    ```

- **map()函数**：新建一列 ， map函数并不是df的方法，而**是series的方法**

- map当做一种运算工具，至于执行何种运算，是由map函数的参数决定的（参数：lambda，函数）

  - map() 可以映射新一列数据

    ```python
    dic = {
        'name':['周杰伦','张三','周杰伦'],
        'salary':[15000,20000,15000]
    }
    df = DataFrame(data=dic)
    #映射关系表（字典）
    dic_map = {
        '周杰伦':'jay',
        '张三':'tom'
    }
    df['e_name'] = df['name'].map(dic_map) # e_name 是新列的名称,用name列做映射关系
    ```

  - map() 中可以使用lambd表达式

  - map() 中可以使用方法，可以是自定义的方法

    eg:map({to_replace:value})

  - **注意** map()中不能使用sum之类的函数，for循环

  - 新增一列：给df中，添加一列，该列的值为中文名对应的英文名

    ![1567673366339](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567673366339.png)

    ```python
    # 注意：
    并不是任何形式的函数都可以作为map的参数。只有当一个函数具有一个参数且有返回值，那么该函数才可以作为map的参数。
    ```

    

3. #### 使用聚合操作对数据异常值检测和过滤

   - 使用df.std()函数可以求得DataFrame对象每一列的标准差

     ```python
     # 创建一个1000行3列的df 范围（0-1），求其每一列的标准差
     df=df.DataFrame(data=np.random.random(size=(1000,3)),columns=['A','B','C'])
     # 对df应用筛选条件,去除标准差太大的数据:假设过滤条件为 C列数据大于两倍的C列标准差
     twice_std = df["c"].std()*2
     # df.loc[df['C'] > twice_std] 找到异常数据
     drop_indexs = df.loc[df['C'] > twice_std].index  # 找到异常数据的行索引
     df.drop(labels=drop_indexs,axis=0)  # 删除异常数据行
     ```

     

4. #### 排序

   - .take() 函数排序

     ```python
     - take()函数接受一个索引列表，用数字表示,使得df根据列表中索引的顺序进行排序
     - df.take([1,3,4,2,5])
     ```

     - 可以借助  np.random.permutation()  函数随机排序

     - np.random.permutation(x)可以生成x个从0-(x-1)的随机数列

       ```python
       np.random.permutation(5) # 生成一个0-5随机顺序的数组
       --->
       array([1, 0, 4, 2, 3])
       
       #df.take(indices=[1,2,0],axis=1).take(indices=np.random.permutation(1000),axis=0)[0:10]
       ```

   - 应用:当DataFrame规模足够大时，直接使用np.random.permutation(x)函数，就配合take()函数实现随机抽样

     

5. ### 数据分类处理(分组聚合)!!!

   - 数据分类处理：

     - 分组：先把数据分为几组
     - 用函数处理：为不同组的数据应用不同的函数以转换数据
     - 合并：把不同组得到的结果合并起来

   - 数据分类处理的核心：

     ```python
      - groupby() 函数
      - groups属性查看分组情况
      - eg: df.groupby(by='item').groups
     ```

- groupby() 分组函数

  - 使用groupby实现分组

  ```python
  df = DataFrame({'item':['Apple','Banana','Orange','Banana','Orange','Apple'],
                  'price':[4,3,3,2.5,4,2],
                 'color':['red','yellow','yellow','green','green','green'],
                 'weight':[12,20,50,30,20,44]})
  
  df.groupby(by='item',axis=0)  -->
  <pandas.core.groupby.DataFrameGroupBy object at 0x00000157CA0B1710>
  ```

  - 使用groups查看分组情况

  ```python
  # 该函数可以进行数据的分组，但是不显示分组情况
  df.groupby(by='item',axis=0).groups
  ```
  - 分组后的聚合操作：分组后的成员中可以被进行运算的值会进行运算，不能被运算的值不进行运算

  ```python
  #给df创建一个新列，内容为各个水果的平均价格
  mean_price_s = df.groupby(by='item',axis=0)['price'].mean()
  dic = mean_price_s.to_dict() # 转为字典
  df['item'].map(dic) # 创建映射关系
  df['mean_price'] = df['item'].map(dic)  # 创建新的列mean_price
  #按颜色查看各种颜色的水果的平均价格
  color_mean_price_s = df.groupby(by='color')['price'].mean()
  dic = color_mean_price_s.to_dict()
  df['mean_price_color'] = df['color'].map(dic)
  ```

6. #### 高级数据聚合

   - 使用groupby分组后，也可以**使用transform和apply提供自定义函数实现更多的运算**
     - df.groupby('item')['price'].sum() <==> df.groupby('item')['price'].apply(sum)
     - transform和apply都会进行运算，在transform或者apply中传入函数即可
     - transform和apply也可以传入一个lambda表达式

   ![1567676103750](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567676103750.png)

## 3.matplotlib 

- 静态图形处理

- 数据分析三剑客
  - Numpy : 主要为了给pandas提供数据源
  - pandas :  更重要的数据结构
  - matplotlib : 静态图形处理 

**海滨城市温度分析案例**

1. 导包

   ```python
   # 导包
   import numpy as np
   import pandas as pd
   from pandas import Series,DataFrame
   
   import matplotlib.pyplot as plt
   
   from pylab import mpl
   mpl.rcParams['font.sans-serif'] = ['FangSong'] # 指定默认字体
   mpl.rcParams['axes.unicode_minus'] = False # 解决保存图像是负号'-'显示为方块的问题
   ```

2. 导入数据(各个海滨城的数据)

   ```python
   # 导入数据(各个海滨城市数据)
   
   ferrara1 = pd.read_csv('./ferrara_150715.csv')
   ferrara2 = pd.read_csv('./ferrara_250715.csv')
   ferrara3 = pd.read_csv('./ferrara_270615.csv')
   ferrara=pd.concat([ferrara1,ferrara1,ferrara1],ignore_index=True)
   
   torino1 = pd.read_csv('./torino_150715.csv')
   torino2 = pd.read_csv('./torino_250715.csv')
   torino3 = pd.read_csv('./torino_270615.csv')
   torino = pd.concat([torino1,torino2,torino3],ignore_index=True) 
   ...
   ```

3. 去除没用的列

   ```python
   city_list = [faenza,cesena,piacenza,bologna,asti,ravenna,milano,mantova,torino,ferrara]
   for city in city_list:
       city.drop(labels='Unnamed: 0',axis=1,inplace=True)
   ```

   <img src="C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567756766535.png" alt="1567756766535" style="zoom:50%;" />

4. 构造数据,显示最高温度与离海远近的关系

   ```python
   max_temp = []   
   dist_list = []
   for city in city_list:
       temp = city["temp"].max()
       max_temp.append(temp)
       dist = city['dist'][0]
       dist_list.append(dist)
       
   plt.scatter(dist_list,max_temp)   # 传入两个列表
   plt.xlabel("距离")  # x
   plt.xlabel("最高温度") # y
   plt.title("最高温度和距离之间的关系") # 标题
   ```

   ![1567756889916](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1567756889916.png)