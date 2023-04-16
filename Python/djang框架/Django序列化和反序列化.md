# Django序列化和反序列化

- 官方文档(3.2)传送门:https://docs.djangoproject.com/zh-hans/3.2/topics/serialization/#id2

## 一 序列化

- **DRF**的核心 就是 前后端分离的核心

- **前后端分离开发的核心：**
  - 将模型转换为json 称之为 序列化
  - 将json转换成模型 称之为 反序列化

### 1.1 序列化器的字段

- Serializer 序列化器: 为了得到模型里的字段，序列化器中的字段应与模型类中的字段名一致

```python
    ''' serializers.py '''
		#from django.core.serializers import serialize # 不是drf
    
    from rest_framework import serializers  #引入序列化器对象

    # drf 版本
    class BookInfoSerializer(serializers.Serializer):

        # read_only=True 只能读 不能修改
        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharField(max_length=20,label='书籍名')
        pub_date = serializers.DataField(label='发布日期')
        readcount = serializers.IntegerField()
        is_delete = serializers.BooleanField()
        image = serializers.ImageField()
```

### 1.2 序列化

- 创建序列器
  - 序列化器的第一个参数：instance 用于序列化操作
  - 序列化器的第二个参数：data 用于反序列化操作
  - 除了instance和data参数外，在构造Serializer对象时，还可通过context参数额外添加数据，如

	> serializer = AccountSerializer(account, context={'request': request})

	- 通过context参数附加的数据，可以通过Serializer对象的context属性获取。

```python
    ''' views.py '''

    book = BookInfo.objects.get(id=2)
    
    s = BookInfoSerializers(instance=book)

    # 我们是通过 序列化器的data属性来获取 模型转换为字典的数据
    s.data


    # 传递多个数据
    # 应用： 查询所有书籍列表

    books = BookInfo.objects.all()
    # 创建序列化器，将所有书籍信息传递给序列化器
    # books = [BookInfo,BookInfo,...] 对象列表
    s = BookInfoSerializers(books,many=True)
    
    person = PeopleInfo.objects.get(id=6)

    # 序列化器初始化
    s = PeopleInfoSerializer(instance=person)
```

### 1.3 关联序列化器的操作

- 对于关联字段，可以采用以下几种方式：

1. PrimaryKeyRelatedField

2. StringRelatedField

3. 使用关联对象的序列化器

```python
#########关联序列化器##########################

    class PeopleInfoSerializer(serializers.Serializer):
        """英雄数据序列化器"""
        GENDER_CHOICES = (
            (0, 'male'),
            (1, 'female')
        )
        id = serializers.IntegerField(label='ID', read_only=True)
        name = serializers.CharField(label='名字', max_length=20)
        gender = serializers.ChoiceField(choices=GENDER_CHOICES, label='性别', required=False)
        description = serializers.CharField(label='描述信息', max_length=200, required=False, allow_null=True)


        ''' PrimaryKeyRelatedField '''
        # 设置关联外键的时候，要将 read_only=True
        # 包含read_only=True参数时，该字段将不能用作反序列化使用
        # book = serializers.PrimaryKeyRelatedField(read_only=True,label='外键')
        # 或者
        # 包含queryset参数时，将被用作反序列化时参数校验使用
        # queryset 将关联模型的所有数据传递给这个属性就可以
        book = serializers.PrimaryKeyRelatedField(label='外键',queryset=BookInfo.objects.all())

        '''StringRelatedField'''
        # 现在通过  PrimaryKeyRelatedField得到的是一个  外键的一个值  2
        # 接下来通过 一个设置 来获取 书籍的名字

        # StringRelatedField 可以获取关联模型中的 __str_ 里的字符串
        book = serializers.StringRelatedField()

        ''' 使用关联对象的序列化器  拿到所有数据 '''
        book = BookInfoSerializer()
```

### 1.4 关联查询

- 关联模型类名小写_set 作为字段名

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharFIeld(max_length=20,label='书籍名')
        pub_date = serializers.DataField(label='发布日期')
        readcount = serializers.IntegerField()
        is_delete = serializers.BooleanField()
        iamge = serializers.ImageField()

        
        # 书籍和人物的关系是 1：n   ===> many=True
        peopleinfo_set = serializers.PrimaryKeyRelatedField(read_only=True,many=True)

        def __str__(self):
            return self.name
```

## 二 反序列化

- 反序列化 分为两步：
  - **数据校验**
  - **数据入库**

### 2.1 数据校验

- 使用序列化器进行反序列化时，需要对数据进行验证后，才能获取验证成功的数据或保存成模型类对象。

  - 在获取反序列化的数据前，必须调用is_valid()方法进行验证，验证成功返回True，否则返回False。

  - 验证失败，可以通过序列化器对象的errors属性获取错误信息，返回字典，包含了字段和字段的错误。

  - 验证成功，可以通过序列化器对象的validated_data属性获取数据。

  - 在定义序列化器时，指明每个字段的序列化类型和选项参数，本身就是一种验证行为

#### 数据校验方式一

在定义序列化器字段的时候，规定是什么类型 就要提交符合规则的数据

例如：DateField 就需要传入符合日期规则的数据

```python
    ##############将JSON转换为模型  反序列化#############

     ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharFIeld(max_length=20,label='书籍名')
        pub_date = serializers.DataField(label='发布日期')
       
        peopleinfo_set = serializers.PrimaryKeyRelatedField(read_only=True,many=True)

        def __str__(self):
            return self.name

    ''' views.py '''

    dict = {
        'name':'itcast',
        'pub_date':'123'   # Flase
        # 'pub_date':'2010-1-1'  # True
    }

    # 1.创建序列器
    # 序列化器的第一个参数：instance 用于序列化操作
    # 序列化器的第二个参数：data 用于反序列化操作
    serializer = BookInfoSerializer(data=dict)

    # 2.需要调用序列化器的 is_valid 方法 valid验证  返回True False
    # 如果数据可用  返回True
    serializer.is_valid()

    # raise_exception=True 可以设置为True 来抛出异常
    serializer.is_valid(raise_exception=True)
```

#### 数据校验方式二

- 字段的选项

  - required : 进行反序列化的时候，必须传这个字段

  - min_length,max_length 作用于字符串

  - min_value,max_value 作用于Int整型

  - default 不传入数据 设置默认值

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharFIeld(min_length=5,max_length=20,label='书籍名',)
        pub_date = serializers.DataField(label='发布日期',required=True)


        def __str__(self):
            return self.name

    ''' views.py '''
    dict = {
        'name':'itcast',
        'pub_date':'123'   # 若去掉pub_date 则报错
    }   
```

#### 数据校验方式三

- 对单个字段的数据进行验证

语法形式为： 在序列化器中实现方法 def validate_字段名()

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharFIeld(min_length=5,max_length=20,label='书籍名',)
        pub_date = serializers.DataField(label='发布日期',required=True)
        readcount = serializers.IntegerField(default=0,required=False)

        def __str__(self):
            return self.name

        def validate_readcount(self,value):
            # value 就是字段传递过来的数据
            if value < 0:
                raise serializers.ValidationError('阅读量不能为负数')

            # 需要将value返回回去
            return value

    ''' views.py '''
    dict = {
        'name':'itcast',
        'readcount':-20,  # 报异常
    }   
```

#### 数据校验方式四

- 对多个字段的数据进行验证时

语法形式为： 在序列化器中实现方法 def validate(self,attrs)

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        id = serializers.IntegerField(read_only=True,label='id')
        name = serializers.CharFIeld(min_length=5,max_length=20,label='书籍名',)
        pub_date = serializers.DataField(label='发布日期',required=True)
        readcount = serializers.IntegerField(default=0,required=False)
        commentcount = serializers.IntegerField(default=0,required=False)


        def __str__(self):
            return self.name

        # 对多个字段进行验证
        # def validate(self,attrs):
        def validate(self,data):
            # attrs  -->  其实就是data
            readcount = data.get('readcount')
            commentcount = data['commentcount']

            if readcount < commentcount:
                raise serializers.ValidationError('评论量不能大于阅读量')

            # 要将数据返回
            return data


    ''' views.py '''
    # 自定义需求：评论量不能大于阅读量
    dict = {
        'name':'itcast',
        'readcount':20, 
        'commentcount':100
    }   
```

#### 数据校验方式五

- 自定义验证方法

同时给字段添加自定义验证方法

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        # 自定义验证方法
        def custom_validate(self):
            if self == 'admin':
            raise serializers.ValidationError('我就是来捣乱的')

        id = serializers.IntegerField(read_only=True,label='id')
       
        # validators=[]  是给字段 添加自定义验证方法
        name = serializers.CharFIeld(min_length=5,max_length=20,label='书籍名',validators=[custom_validate])
       

        def __str__(self):
            return self.name


    ''' views.py '''
    # 规定：评论量不能大于阅读量
    dict = {
        'name':'itcast',
        'readcount':20, 
        'commentcount':100
    }   
```

### 2.2 数据入库

#### 数据保存 save 方法 

- 继承自Serializer的序列化 我们在调用save方法的时候，需要手动实现create方法，

- 调用save方法之前，必须调用 is_valid方法,  即 **要想保存数据，必须保证数据是经过校验的**。

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        def create(self,validated_data):

            # dict -->  data --> attrs  -->  validated_data
            # validated_data 此处其实就是views.py中的dict
            # validated_data 已经被验证过的数据

            # *  对列表进行解包    *list
            # ** 对字典进行解包    **dict
            #   此处解包  将dict中的值 赋值给对象中的对应字段
            book = BookInfo.objects.create(**validated_data)

            # create 需要将创建的对象返回
            return book 



     ''' views.py '''

    # 规定：评论量不能大于阅读量
    dict = {
        'name':'itcast',
        'readcount':20, 
        'commentcount':100
    }   



    serializer = BookInfoSerializer(data=dict)
    serializer.is_valid(raise_exception=True)

    # 3. 保存需要调用序列化器的save方法
    # 继承自Serializer的序列化 我们在调用save方法的时候，需要手动实现create方法
    serializer.save()
```

#### 序列化器中传入两个参数 - 更新

- 如果我们在序列化器中既传入了对象，又传入了数据, 系统会认为我们在更新数据

继承自Serializer的类，要更新数据的时候，需要手动实现update方法

```python
    ''' serializers.py '''

    class BookInfoSerializer(serializers.Serializer):

        def update(self,instance,validated_data):
            # instance : 就是我们在更新数据时，传入序列化器的对象
            # validated_data ： 验证之后的数据

            instance.name = validated_data.get('name',instance.name)
            instance.pub_date = validated_data.get('pub_date',instance.pub_date)
            instance.readcount = validated_data.get('readcount',instance.readcount)
            instance.commentcount = validated_data.get('commentcount',instance.commentcount)

            instance.save()

            # update()方法需要我们手动返回对象
            return instance
            


    '''views.py '''

    # 1.获取对象
    book = BookInfo.objects.get(id=2)
    # 2.保存数据
    data = {
        'name':'lalala',
        'pub_date':'2018-1-1',
        'readcount':1000,
        'commentcount':10
    }    

    # 3.创建序列化器
    s = BookInfoSerializer(instance=book,data=data)
    # 4.验证数据
    s.is_valid(raise_exception=True)
    # 5.保存数据
    s.save()
```

## 三 ModelSerializer

- 如果我们想要使用序列化器对应的是Django的模型类，DRF为我们提供了ModelSerializer模型类, 序列化器来帮助我们快速创建一个Serializer类。

- ModelSerializer与常规的Serializer相同，但提供了：

  - 基于模型类自动生成一系列字段

  - 包含默认的create()和update()的实现

```python
    ''' serializers.py '''

    class BookSerializer(serializers.ModelSerializer):
        
        # 如何设置   通过class Meta
        class Meta:
            model = BookInfo    # 设置关联模型     model就是关联模型
            # fields = '__all__'  # fields设置字段   __all__表示所有字段
            # fields = ['id','name','pub_date']  # fields设置字段  []列表显示来设置
            exclude = ['image']   # exclude 排除列表中的字段，剩余的字段都显示

            read_only_fields = ('id','readcount','commentcount')

            # 我们可以对自动生成的字段  进行额外的设置
            extra_kwargs = {
                # 字段名：{选项：值}
                'pub_date':{'required':True},
                'readcount':{
                    'max_value':10000,
                    'min_value':0
                }
            }

    '''views.py'''

    #########ModelSerializer##############
    data = {
            'name':'abc',
            'pub_date':'2018-1-1',
            'readcount':1000,
            'commentcount':10
        }    

    s = BookSerializer(data=data)
    s.is_valid(raise_exception=True)
    s.save()
```