### 1. 生成器定义

在Python中，一边循环一边计算的机制，称为生成器：generator。

### 2. 为什么要有生成器

列表所有数据都在内存中，如果有海量数据的话将会非常耗内存。

如：仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

如果列表元素按照某种算法推算出来，那我们就可以在循环的过程中不断推算出后续的元素，这样就不必创建完整的list，从而节省大量的空间。

简单一句话：我又想要得到庞大的数据，又想让它占用空间少，那就用生成器！

### 3.如何创建生成器

第一种方法很简单，只要把一个列表生成式的`[]`改成`()`，就创建了一个generator：

```
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

 创建`L`和`g`的区别仅在于最外层的`[]`和`()`，`L`是一个list，而`g`是一个generator。 

方法二， 如果一个函数中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator。调用函数就是创建了一个生成器（generator）对象。

 

### 4. 生成器的工作原理

（1）生成器(generator)能够迭代的关键是它有一个next()方法，

　　工作原理就是通过重复调用next()方法，直到捕获一个异常。

（2）带有 yield 的函数不再是一个普通函数，而是一个生成器generator。

　　可用next()调用生成器对象来取值。next 两种方式 t.__next__() | next(t)。

　　可用for 循环获取返回值（每执行一次，取生成器里面一个值）

　　（基本上不会用`next()`来获取下一个返回值，而是直接使用`for`循环来迭代）。

（3）yield相当于 return 返回一个值，并且记住这个返回的位置，下次迭代时，代码从yield的下一条语句开始执行。

（4）.send() 和next()一样，都能让生成器继续往下走一步（下次遇到yield停），但send()能传一个值，这个值作为yield表达式整体的结果

　　——换句话说，就是send可以强行修改上一个yield表达式值。比如函数中有一个yield赋值，a = yield 5，第一次迭代到这里会返回5，a还没有赋值。第二次迭代时，使用.send(10)，那么，就是强行修改yield 5表达式的值为10，本来是5的，那么a=10

### 5. send()方法

```python
#!/usr/bin/python3

def MyGenerator():
        value=yield 1
        yield value
        return done

gen=MyGenerator()
print(next(gen))
print(gen.send("I am Value"))
```

生成器内有一个方法send，可再次传入一个值。

------

上面那句可能听不懂，但是不要紧，我们先来看看代码，

```python
#!/usr/bin/python3
def MyGenerator():
        value=yield 1
        yield value
        return done

gen=MyGenerator()
print(next(gen))
print(gen.send("I am Value"))
```

代码分析，
在`MyGenerator`里，我们一共用了两次`yield`。
比较奇怪的是第一个`yield`的语句，`value=yield 1`。如果没看过这一语句的，肯定不知道`next`回到`yield`后，其实是有一个值的。

到这，我们先不急，运行结果，

```ruby
[penx@ali01 python]$ ./gen_send.py 
1
I am Value
[penx@ali01 python]$ 
```

运行过程，
用`next`启动了生成器`gen`，知道到`yield 1`时返回1。

然后我们再用`gen`的内部方法`send`进入`gen`，而且还带回来一个值“`I am Value`”。这时候，继续执行`yield 1`后的代码“`value=`”，把带回来的值“`I am Value`”赋给`value`。直到遇到`yield value`，把`value`返回。

------

其实，send和next的执行很像，只是send可以和生成器互动，传入一个值。

------

#### 5.1生成器的启动需要next

大家有没有想过，如果生成器还没启动过，就用send，会怎样？我们来试一下。
代码，



```python
#!/usr/bin/python3

def MyGenerator():
        value=yield 1
        yield value
        return done

gen=MyGenerator()
print(gen.send(3))
```

运行，



```ruby
[penx@ali01 python]$ ./gen_send.py 
Traceback (most recent call last):
  File "./test.py", line 9, in <module>
    print(gen.send(3))
TypeError: can't send non-None value to a just-started generator
[penx@ali01 python]$ 
```

结果，
报错，



```csharp
> TypeError: can’t send non-None value to a just-started generator
```

说生成器刚启动时，不能`send`一个不为`None`的值。

小结，
所以呢，我们在用生成器时，第一次要用`next`启动

------

#### 5.2生成器启动可用send（None）

其实上面报错已经说了，`can’t send non-None value`
所以啊，我们可以用`send（None）`来启动生成器。
代码:

```csharp
#!/usr/bin/python3

def MyGenerator():
        value=yield 1
        yield value
        return done

gen=MyGenerator()
print(gen.send(None))
print(gen.send(3))`</pre>

运行，

[penx@ali01 python]$ ./gen_send.py 
1
3
[penx@ali01 python]$

结果， 
正常运行。
```