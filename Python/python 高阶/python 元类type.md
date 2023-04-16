## 引子

>- **所有的 Python 的用户定义类，都是 type 这个类的实例。**
>
>- **用户自定义类，只不过是 type 类的 `__call__` 运算符重载**
>- **metaclass 是 type 的子类，通过替换 type 的 `__call__` 运算符重载机制，“超越变形”正常的类**

**type()** 函数属于 [Python](http://c.biancheng.net/python/) 内置函数，通常用来查看某个变量的具体类型。其实，type() 函数还有一个更高级的用法，即创建一个自定义类型（也就是创建一个类）。

type() 函数的语法格式有 2 种，分别如下：

- type(obj) 

- type(name, bases, dict)

以上这 2 种语法格式，各参数的含义及功能分别是：

- 第一种语法格式用来查看某个变量（类对象）的具体类型，obj 表示某个变量或者类对象。
- **第二种语法格式用来创建类，其中 name 表示类的名称；bases 表示一个元组，其中存储的是该类的父类；dict 表示一个字典，用于表示类内定义的属性或者方法。**


对于使用 type() 函数查看某个变量或类对象的类型，由于很简单，这里不再做过多解释，直接给出一个样例：

```python
#查看 3.4 的类型
print(type(3.4))  #<class 'float'>
#查看类对象的类型
class CLanguage:    
	passclangs = CLanguage()print(type(clangs))  #<class '__main__.CLanguage'>
```


这里重点介绍 type() 函数的另一种用法，即创建一个新类，先来分析一个样例：

```python
#定义一个实例方法
def say(self):
	print("我要学 Python！")
#使用 type() 函数创建类
CLanguage = type("CLanguage",(object,),dict(say = say, name = "C语言中文网"))
#创建一个 CLanguage 实例对象
clangs = CLanguage()
#调用 say() 方法和 name 属性
clangs.say()  #我要学 Python！
print(clangs.name) #C语言中文网
```

> 注意，Python 元组语法规定，当 (object,) 元组中只有一个元素时，最后的逗号（,）不能省略。

此程序中通过 type() 创建了类，其类名为 CLanguage，继承自 objects 类，且该类中还包含一个 say() 方法和一个 name 属性。

> 有读者可能会问，如何判断 dict 字典中添加的是方法还是属性？很简单，如果该键值对中，值为普通变量（如 "C语言中文网"），则表示为类添加了一个类属性；反之，如果值为外部定义的函数（如 say() ），则表示为类添加了一个实例方法。

可以看到，使用 type() 函数创建的类，和直接使用 class 定义的类并无差别。事实上，我们在使用 class 定义类时，Python 解释器底层依然是用 type() 来创建这个类。

## 动态地创建类

>  type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)

```python
#定义一个实例方法
def say(self):
	print("我要学 Python！")
#使用 type() 函数创建类
CLanguage = type("CLanguage",(object,),dict(say = say, name = "C语言中文网"))
#创建一个 CLanguage 实例对象
clangs = CLanguage()
#调用 say() 方法和 name 属性
clangs.say()  #我要学 Python！
print(clangs.name) #C语言中文网
```

## 到底什么是元类

>1.拦截类的创建
>2.修改类
>3.返回修改之后的类

- **元类就是用来创建这些类（对象）的，元类就是类的类**
- **type就是Python的内建元类**

- 元类就是用来创建类的“东西”。你创建类就是为了创建类的实例对象，不是吗？但是我们已经学习到了Python中的类也是对象。好吧，元类就是用来创建这些类（对象）的，**元类就是类的类**，你可以这样理解 为：

```python
MyClass = MetaClass()
MyObject = MyClass()
```

- 你已经看到了type可以让你像这样做：

```python
MyClass = type('MyClass', (), {})
```

- 这是因为函数type实际上是一个元类。type就是Python在背后用来创建所有类的元类。现在你想知道那为什么type会全部采用小写形式而不是Type呢？好吧，我猜这是为了和str保持一致性，str是用来创建字符串对象的类，而int是用来创建整数对象的类。type就是创建类对象的类。你可以通过检查class属性来看到这一点。Python中所有的东西，注意，我是指所有的东西——都是对象。这包括整数、字符串、函数以及类。它们全部都是对象，而且它们都是从一个类创建而来。

```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>>foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
```

- 现在，对于任何一个class的class属性又是什么呢？

```python
>>> a.__class__.__class__
<type 'type'>
>>> age.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```

- 因此，元类就是创建类这种对象的东西。（ **type就是Python的内建元类**，你也可以创建自己的元类。)

## metaclass属性

- 你可以在写一个类的时候为其添加metaclass属性。

  ```python
  class Foo(object):
      __metaclass__ = something…
  ```

- Python会在内存中通过metaclass创建一个名字为Foo的类对象（我说的是类对象，请紧跟我的思路）。如果Python没有找到metaclass，它会继续在（父类）中寻找metaclass属性，并尝试做和前面同样的操作。如果Python在任何父类中都找不到metaclass，它就会在模块层次中去寻找metaclass，并尝试做同样的操作。如果还是找不到metaclass,Python就会用内置的type来创建这个类对象。

  ```python
  # 元类会自动将你通常传给‘type'的参数作为自己的参数传入
  def upper_attr(future_class_name, future_class_parents, future_class_attr):
      '''返回一个类对象，将属性都转为大写形式'''
      #  选择所有不以'__'开头的属性
      attrs = ((name, value) for name, value in future_class_attr.items() if not name.startswith('__'))
   
  
  
    # 将它们转为大写形式
      uppercase_attr = dict((name.upper(), value) for name, value in attrs)
  
      # 通过'type'来做类对象的创建
      return type(future_class_name, future_class_parents, uppercase_attr)
  
  __metaclass__ = upper_attr  #  这会作用到这个模块中的所有类
  
  class Foo(object):
      # 我们也可以只在这里定义__metaclass__，这样就只会作用于这个类中
      bar = 'bip'
  
  print hasattr(Foo, 'bar')
  # 输出: False
  print hasattr(Foo, 'BAR')
  # 输出:True
  
  f = Foo()
  print f.BAR
  # 输出:'bip'
  ```

  

- 真实的产品代码中一个元类应该是像这样的：

  ```python
  class UpperAttrMetaclass(type):
      def __new__(cls, name, bases, dct):
          attrs = ((name, value) for name, value in dct.items() if not name.startswith('__')
          uppercase_attr  = dict((name.upper(), value) for name, value in attrs)
          return type.__new__(cls, name, bases, uppercase_attr)
  ```

- 如果使用super方法的话，我们还可以使它变得更清晰一些，这会缓解继承（是的，你可以拥有元类，从元类继承，从type继承）

  ```python
  class UpperAttrMetaclass(type):
      def __new__(cls, name, bases, dct):
          attrs = ((name, value) for name, value in dct.items() if not name.startswith('__'))
          uppercase_attr = dict((name.upper(), value) for name, value in attrs)
          return super(UpperAttrMetaclass, cls).__new__(cls, name, bases, uppercase_attr)
  ```

## 元类的应用

- 元类就是深度的魔法，99%的用户应该根本不必为此操心。如果你想搞清楚究竟是否需要用到元类，那么你就不需要它。那些实际用到元类的人都非常清楚地知道他们需要做什么，而且根本不需要解释为什么要用元类。 —— Python界的领袖 Tim Peters

- 元类的主要用途是创建API。**一个典型的例子是Django ORM**。它允许你像这样定义：

```python
class Person(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()
```

- 但是如果你像这样做的话：

```python
guy  = Person(name='bob', age='35')
print guy.age
```

​		这并不会返回一个IntegerField对象，而是会返回一个int，甚至可以直接从数据库中取出数据。这是有可能的，因为models.Model定义了metaclass， 并且使用了一些魔法能够将你刚刚定义的简单的Person类转变成对数据库的一个复杂hook。Django框架将这些看起来很复杂的东西通过暴露出一个简单的使用元类的API将其化简，通过这个API重新创建代码，在背后完成真正的工作。