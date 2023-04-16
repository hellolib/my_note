# Django models 之字段详解

- 参考https://www.cnblogs.com/geogre123/p/10621364.html

## Django models 的字段类型

1、**models.AutoField**   ---自增列 = int(11)    如果没有的话，默认会生成一个名称为 id 的列，如果要显示的自定义一个自增列，必须将给列设置为主键 primary_key=True。
2、**models.CharField**   ---字符串字段  单行输入，用于较短的字符串，如要保存大量文本, 使用 TextField。必须 max_length 参数，django会根据这个参数在数据库层和校验层限制该字段所允许的最大字符数。

3、**models.BooleanField**   ---布尔类型=tinyint(1)   不能为空，Blank=True

4、**models.ComaSeparatedIntegerField**   ---用逗号分割的数字=varchar   继承CharField，所以必须 max_lenght 参数，

5、**models.DateField**   ---日期类型 date   对于参数，auto_now = True 则每次更新都会更新这个时间；auto_now_add 则只是第一次创建添加，之后的更新不再改变。
6、**models.DateTimeField**   ---日期类型 datetime   同DateField的参数
7、**models.Decimal**   ---十进制小数类型 = decimal   必须指定整数位max_digits和小数位decimal_places
8、**models.EmailField**   ---字符串类型（正则表达式邮箱） =varchar   对字符串进行正则表达式   一个带有检查 Email 合法性的 CharField，不接受 maxlength 参数。
9、**models.FloatField**   ---浮点类型 = double   浮点型字段。 必须提供两个 参数， 参数描述：

　　**max_digits**：总位数(不包括小数点和符号）

　　**decimal_places**：小数位数。如：要保存最大值为 999 (小数点后保存2位)，你要这样定义字段：models.FloatField(…，max_digits=5， decimal_places=2)，要保存最大值一百万(小数点后保存10位)的话，你要这样定义：models.FloatField(…，max_digits=19， decimal_places=10)

10、**models.IntegerField**   ---整形   用于保存一个整数
11、**models.BigIntegerField**   ---长整形
　　integer_field_ranges = {
　　　　'SmallIntegerField': (-32768, 32767),
　　　　'IntegerField': (-2147483648, 2147483647),
　　　　'BigIntegerField': (-9223372036854775808, 9223372036854775807),
　　　　'PositiveSmallIntegerField': (0, 32767),
　　　　'PositiveIntegerField': (0, 2147483647),
　　}
12、**models.IPAddressField**   ---字符串类型（ip4正则表达式）   一个字符串形式的 IP 地址， (如 “202.1241.30″)。
13、**models.GenericIPAddressField**   ---字符串类型（ip4和ip6是可选的）   参数protocol可以是：both、ipv4、ipv6   验证时，会根据设置报错
14、models.NullBooleanField   ---允许为空的布尔类型   类似 BooleanField， 不过允许 NULL 作为其中一个选项。 推荐使用这个字段而不要用 BooleanField 加 null=True 选项。 admin 用一个选择框 　　　　<select> (三个可选择的值： “Unknown”， “Yes” 和 “No” ) 来表示这种字段数据。
15、models.PositiveIntegerField   ---正Integer   类似 IntegerField， 但取值范围为非负整数（这个字段应该是允许0值的…可以理解为无符号整数）
16、**models.PositiveSmallIntegerField**   ---正smallInteger  正小整型字段，类似 PositiveIntegerField， 取值范围较小(数据库相关)SlugField“Slug” 是一个报纸术语。 slug 是某个东西的小小标记(短签)， 只包　　含字母，数字，下划线和连字符。它们通常用于URLs。 若你使用 Django 开发版本，你可以指定 maxlength。 若 maxlength 未指定， Django 会使用默认长度： 50，它接受一个额外的参数：　　

　　prepopulate_from: 来源于slug的自动预置列表

17、**models.SlugField**   ---减号、下划线、字母、数字   它们通常用于URLs。
18、**models.SmallIntegerField**   ---数字   数据库中的字段有：tinyint、smallint、int、bigint.   类似 IntegerField， 不过只允许某个取值范围内的整数。(依赖数据库)
19、**models.TextField**   ---字符串=longtext ，一个容量很大的文本字段， admin 管理界面用 <textarea>多行编辑框表示该字段数据。

20、**models.TimeField**   ---时间 HH:MM[:ss[.uuuuuu]]   时间字段，类似于 DateField 和 DateTimeField。
21、**models.URLField**   ---字符串，地址正则表达式   用于保存URL。若 verify_exists 参数为 True (默认)， 给定的 URL 会预先检查是否存在(即URL是否被有效装入且没有返回404响应).
22、**models.BinaryField**   ---二进制
23、**models.ImageField**   ---图片   类似 FileField， 不过要校验上传对象是否是一个合法图片。它有两个可选参数：height_field 和 width_field，如果提供这两个参数，则图片将按提供的高度和宽度规格保存。 　　该字段要求 Python Imaging 库。
24、**models.FilePathField**   ---选择指定目录按限制规则选择文件，有三个参数可选， 其中”path”必需的，这三个参数可以同时使用， 参数描述：

　　path：必需参数，一个目录的绝对文件系统路径。 FilePathField 据此得到可选项目。 Example： “/home/images”；

　　match：可选参数， 一个正则表达式， 作为一个字符串， FilePathField 将使用它过滤文件名。 注意这个正则表达式只会应用到 base filename 而不是路径全名。 Example： “foo。*\。txt^”， 将匹配文件 foo23.txt 却不匹配 bar.txt 或 foo23.gif；
　　recursive：可选参数， 是否包括 path 下全部子目录，True 或 False，默认值为 False。

 

　　match 仅应用于 base filename， 而不是路径全名。 如：FilePathField(path=”/home/images”， match=”foo.*”， recursive=True)…会匹配 /home/images/foo.gif 而不匹配 /home/images/foo/bar.gif

 

25、**models.FileField**   ---文件上传字段。 要求一个必须有的参数： upload_to， 一个用于保存上载文件的本地文件系统路径。 这个路径必须包含 strftime formatting， 该格式将被上载文件的 date/time 替换(so that uploaded files don’t fill up the given directory)。在一个 model 中使用 FileField 或 ImageField 需要以下步骤：在你的 settings 文件中， 定义一个完整路径给 MEDIA_ROOT 以便让 Django在此处保存上传文件。 (出于性能考虑，这些文件并不保存到数据库。) 定义 MEDIA_URL 作为该目录的公共 URL。 要确保该目录对 WEB 服务器用户帐号是可写的。在你的 model 中添加 FileField 或 ImageField， 并确保定义了 upload_to 选项，以告诉 Django 使用 MEDIA_ROOT 的哪个子目录保存上传文件。你的数据库中要保存的只是文件的路径(相对于 MEDIA_ROOT)。 出于习惯你一定很想使用 Django 提供的 get_<fieldname>_url 函数。举例来说，如果你的 ImageField 叫作 mug_shot， 你就可以在模板中以 {{ object。get_mug_shot_url }} 这样的方式得到图像的绝对路径。

 

26、**models.PhoneNumberField**   ---一个带有合法美国风格电话号码校验的 CharField(格式：XXX-XXX-XXXX)

27、**models.USStateField**   ---美国州名缩写，由两个字母组成（天朝人民无视）。

28、**models.XMLField**   ---XML字符字段，校验值是否为合法XML的 TextField，必须提供参数：

　　schema_path：校验文本的 RelaxNG schema 的文件系统路径。

## Field 选项中的参数意义

1. null ：缺省设置为false.通常不将其用于字符型字段上，比如    CharField,TextField上.字符型字段如果没有值会返回空字符串。
2. blank：该字段是否可以为空。如果为假，则必须有值
3. choices：一个用来选择值的2维元组。第一个值是实际存储的值，第二个用来方便进行选择。如SEX_CHOICES= ((‘F’,'Female’),(‘M’,'Male’),)
4. core：db_column，db_index 如果为真将为此字段创建索引
5. default：设定缺省值
6. editable：如果为假，admin模式下将不能改写。缺省为真
7. help_text：admin模式下帮助文档
8. primary_key：设置主键，如果没有设置django创建表时会自动加上：
9. radio_admin：用于admin模式下将select转换为radio显示。只用于ForeignKey或者设置了choices
10. unique：数据唯一 unique=True. Only one 
11. unique_for_date：日期唯一，如下例中系统将不允许title和pub_date两个都相同的数据重复出现
12. title = meta.CharField(maxlength=30,unique_for_date=’pub_date’)
13. unique_for_month / unique_for_year：用法同上
14. validator_list：有效性检查。非有效产生 django.core.validators.ValidationError 错误

## meta详解

1.**abstract**

这个属性是定义当前的模型是不是一个抽象类。所谓抽象类是不会对应数据库表的。一般我们用它来归纳一些公共属性字段，然后继承它的子类可以继承这些字段。

Options.abstract
如果abstract = True 这个model就是一个抽象类

2.**app_label**

这个选型只在一种情况下使用，就是你的模型不在默认的应用程序包下的models.py文件中，这时候需要指定你这个模型是哪个应用程序的。

Options.app_label
如果一个model定义在默认的models.py，例如如果你的app的models在myapp.models子模块下，你必须定义app_label让Django知道它属于哪一个app
app_label = 'myapp'

3.**db_table**

db_table是指定自定义数据库表名的。Django有一套默认的按照一定规则生成数据模型对应的数据库表明。
Options.db_table
定义该model在数据库中的表名称
　　db_table = 'Students'
如果你想使用自定义的表名，可以通过以下该属性
　　table_name = 'my_owner_table'

4.**db_teblespace**

Options.db_teblespace
定义这个model所使用的数据库表空间。如果在项目的settin中定义那么它会使用这个值

5.**get_latest_by**

Options.get_latest_by
在model中指定一个DateField或者DateTimeField。这个设置让你在使用model的Manager上的lastest方法时，默认使用指定字段来排序

6.**managed**

Options.managed
默认值为True，这意味着Django可以使用syncdb和reset命令来创建或移除对应的数据库。默认值为True,如果你不希望这么做，可以把manage的值设置为False

7.**order_with_respect_to**

这个选项一般用于多对多的关系中，它指向一个关联对象，就是说关联对象找到这个对象后它是经过排序的。指定这个属性后你会得到一个get_xxx_order()和set_xxx_order()的方法，通过它们你可以设置或者回去排序的对象

8.**ordering**

这个字段是告诉Django模型对象返回的记录结果集是按照哪个字段排序的。这是一个字符串的元组或列表，没有一个字符串都是一个字段和用一个可选的表明降序的'-'构成。当字段名前面没有'-'时，将默认使用升序排列。使用'?'将会随机排列

ordering=['order_date'] # 按订单升序排列
ordering=['-order_date'] # 按订单降序排列，-表示降序
ordering=['?order_date'] # 随机排序，？表示随机
ordering=['-pub_date','author'] # 以pub_date为降序，在以author升序排列
9.**permissions**

permissions主要是为了在Django Admin管理模块下使用的，如果你设置了这个属性可以让指定的方法权限描述更清晰可读。Django自动为每个设置了admin的对象创建添加，删除和修改的权限。
permissions = (('can_deliver_pizzas','Can deliver pizzas'))

10.proxy

这是为了实现代理模型使用的，如果proxy = True,表示model是其父的代理 model 

11.**unique_together**

unique_together这个选项用于：当你需要通过两个字段保持唯一性时使用。比如假设你希望，一个Person的FirstName和LastName两者的组合必须是唯一的，那么需要这样设置：
unique_together = (("first_name", "last_name"),)
一个ManyToManyField不能包含在unique_together中。如果你需要验证关联到ManyToManyField字段的唯一验证，尝试使用signal(信号)或者明确指定through属性。

12.**verbose_name**

verbose_name的意思很简单，就是给你的模型类起一个更可读的名字一般定义为中文，我们：
verbose_name = "学校"

13.**verbose_name_plural**

这个选项是指定，模型的复数形式是什么，比如：
verbose_name_plural = "学校"
如果不指定Django会自动在模型名称后加一个’s’



