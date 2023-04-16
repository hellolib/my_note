## python-docx-template 文档

### 快速入门

要使用 pip 安装：

```
pip install docxtpl
```

或使用康达：

```
conda install docxtpl --channel conda-forge
```

使用：

```python
from docxtpl import DocxTemplate

doc = DocxTemplate("my_word_template.docx")
context = { 'company_name' : "World company" }
doc.render(context)
doc.save("generated_doc.docx")
```



### 介绍

此包使用 2 个主要包：

- 用于读取、编写和创建子文档的 python 文档
- 用于管理插入模板文档的标记的 jinja2



### Jinja2 语法

由于使用 Jinja2 包，可以使用单词文档中的所有 jinja2 标记和筛选器。然而，有一些限制和扩展，使其在单词文档中工作：



#### 限制

通常的 jinja2 标记，只能在同一段落的同一运行中使用，它不能跨多个段落、表行、行使用。如果要使用其样式管理段落、表行和整个运行，则必须使用特殊标记语法，如下一章所述。

**注意：**

Microsoft Word 的"运行"是具有相同样式的字符序列。例如，如果创建具有相同样式的所有字符的段落，MS Word 将在段落中仅创建一个"运行"。现在，如果在本段中间用粗体字，单词会将以前的"运行"转换为 3 个不同的"运行"（普通 - 粗体 - 普通）。

**重要：**

始终在 jinja2 开始 var/tag 分隔符和结束之前放一个空格：

避免：

```
{{myvariable}}
{%if something%}
```

请改为使用：

```
{{ myvariable }}
{% if something %}
```



#### 扩展



##### 标签

为了管理段落、表行、表列、运行，必须使用特殊语法

```
{%p jinja2_tag %} for paragraphs
{%tr jinja2_tag %} for table rows
{%tc jinja2_tag %} for table columns
{%r jinja2_tag %} for runs
```

通过使用这些标记，python-docx-模板将小心地将真正的 jinja2 标记放在正确的位置放入文档的 xml 源代码中。此外，这些标记还告诉 python-docx-模板删除段落、表行、表列或运行开始标记和结束标记的位置，并仅注意介于两者之间的内容。

**重要** 提示：不要在同一段落、行、列或运行中使用 或两次。例子：`{%p` `{%tr` `{%tc` `{%r`

不要使用此

```
{%p if display_paragraph %}Here is my paragraph {%p endif %}
```

但是在文档模板中改为使用此选项

```
{%p if display_paragraph %}
Here is my paragraph
{%p endif %}
```

此语法是可能的，因为 MS Word 将每行视为新段落，并且标记不在第二种情况下位于同一段落中。`{%p`



##### 拆分和合并文本

- 您可以使用使用`{%-`
- 您可以使用使用`-%}`

如果太长，包含 Jinja2 标记的文本可能无法读取：

```
My house is located {% if living_in_town %} in urban area {% else %} in countryside {% endif %} and I love it.
```

可以使用 Enter*或* *SHIFT+Enter*拆分如下文本，然后使用 并告诉 docxtpl 合并整个内容：`{%-` `-%}`

```
My house is located
{%- if living_in_town -%}
 in urban area
{%- else -%}
 in countryside
{%- endif -%}
 and I love it.
```

**重要提示：**当在行开始或结尾需要空格时，使用牢不可破的空间 （*CTRL+SHIFT+*空格）。

**重要 2 ：**标记必须单独在一行中： 不要在同一行之前或之后添加一些文本。`{%- xxx -%}`



##### 显示变量

作为 jinja2 的一部分，可以使用双大括号：

```
{{ <var> }}
```

如果 是字符串 ， 和 将分别转换为换行符、新段落、制表符和分页符`<var>` `\n` `\a` `\t` `\f`

但如果是[RichText 对象](https://github.com/elapouya/python-docx-template/blob/master/docs/index.rst#richtext)，则必须指定要更改实际的"运行"：`<var>`

```
{{r <var> }}
```

请注意打开大括号后右侧。`r`

**重要**提示：请勿在模板中使用该变量，因为可以解释为未指定的变量。但是，您可以使用以"r"为起点的较大变量名称。例如，将解释为不是 。`r` `{{r}}` `{{r` `{{render_color}}` `{{ render_color }}` `{{r ender_color}}`

**重要**提示：不要在同一次运行中使用 2 次。使用 RichText.add（） 方法将多个字符串和样式串联在 python 端，而模板端只有一个字符串和样式。`{{r` `{{r`



##### 单元格颜色

当您想要更改表格单元格的背景颜色时，有特殊情况，则必须在单元格的开头将以下标记放在：

```
{% cellbg <var> %}
```

<var> 必须包含颜色的十六进制代码，*而不*带哈希符号



##### 生成列

如果要动态跨多列的表单元格（当具有动态列计数的表时，这非常有用），则必须将以下标记放在单元格的开头以跨越：

```
{% colspan <var> %}
```

<var> 必须包含要跨越的列数的整数。有关示例test_files/dynamic_table.py/测试。



##### 转码

要显示 、 或 ，可以使用：`{%` `%}` `{{` `}}`

```
{_%, %_}, {_{ or  }_}
```



### 丰富的文本

在模板中使用标记时，它将被 var 变量中包含的字符串替换。但它将保持当前风格。如果要添加动态可变样式，必须同时使用 ：var 变量中的标记和对象。您可以更改颜色、粗体、半式、大小等，但最好的方式是使用 Microsoft Word 定义自己的*字符*样式（主页选项卡 -> 修改样式 -> 管理样式按钮 -> 新样式，在窗体中选择"字符样式"），请参阅测试/富文本中的示例.py而不是使用 ， 可以使用其快捷方式：`{{ <var> }}` `{{r <var> }}` `RichText` `RichText()` `R()`

**重要**提示：当您使用它从 docx 模板中删除当前字符样式时，这意味着如果不在 中指定样式，该样式将返回 Microsoft 字默认样式。这将只影响字符样式，而不影响段落样式（MSWord 管理这 2 种样式）。`{{r }}``RichText()`



#### 带富文本的超链接

您可以使用具有此语法的 Richtext 向文本添加超链接：

```
tpl=DocxTemplate('your_template.docx')
rt = RichText('You can add an hyperlink, here to ')
rt.add('google',url_id=tpl.build_url_id('http://google.com'))
```

放入您的上下文，然后在模板中使用`rt``{{r rt}}`



### 内联图像

您可以将一个或多个图像动态添加到文档中（使用 JPEG 和 PNG 文件进行测试）。只需在模板中添加标记， doxtpl 的实例在哪里。内联图像：`{{ <var> }}``<var>`

```
myimage = InlineImage(tpl,'test_files/python_logo.png',width=Mm(20))
```

您只需指定模板对象、图像文件路径以及选项宽度和/或高度。对于高度和宽度，您必须使用毫米（毫米）、英寸（英寸）或点（Pt）类。有关示例，请参阅inline_image.py/说明。



### 子文档

模板变量可以包含一个复杂的内容，并且使用 python-docx 字文档从头开始构建。为此，首先从模板对象获取子文档对象，并用作 python-docx 文档对象，请参阅测试/子文档.py。



### 转义，新行，新段落，列出

使用 时，正在修改**XML**字文档，这意味着不能使用所有字符，尤其是 和 。为了使用它们，你必须逃脱它们。有 4 种方法：`{{ <var> }}``<``>``&`

> - `context = { 'var':R('my text') }`在模板中（请注意 ）`{{r <var> }}``r`
> - `context = { 'var':'my text'}`和在你的字模板`{{ <var>|e }}`
> - `context = { 'var':escape('my text')}`并在模板中。`{{ <var> }}`
> - 在调用渲染方法时启用自动回用： （默认值为自动逃生 = 错误）`tpl.render(context, autoescape=True)`

or 提供新行、新段落和分页符功能：只需在文本中使用 ，或在文本中，它们将被相应地转换。`RichText()``R()``\n``\a``\t``\f`

有关详细信息，请参阅.py转义"示例。

另一个解决方案，如果要在文档中包含一个列表，就是转义文本并管理 n、a 和 f，您可以使用 类 ：`Listing`

在 python 代码中：

```
context = { 'mylisting':Listing('the listing\nwith\nsome\nlines \a and some paragraph \a and special chars : <>&') }
```

在文档模板中，仅使用 With ，您将保留当前字符样式（在开始新段落时，除 在 之后）。`{{ mylisting }}``Listing()``\a`



### 替换文档图片

无法动态添加页眉/页脚的图像，但您可以更改它们。其理念是将虚拟图片放在模板中，照常呈现模板，然后将虚拟图片替换为另一幅。您可以同时为所有媒体这样做。注意：纵横比与替换的图像 Note2 相同：指定用于在 docx 模板中插入图像的文件名（仅其基名，而不是完整路径）

替换dummy_header_pic.jpg：

```
tpl.replace_pic('dummy_header_pic.jpg','header_pic_i_want.jpg')
```

替换发生在页眉、页脚和整个文档的正文中。



### 更换文档介质

无法动态添加标题/页脚图像以外的其他媒体，但您可以更改它们。其理念是将虚拟媒体放入模板中，照常呈现模板，然后将虚拟媒体替换为另一个媒体。您可以同时为所有媒体这样做。注意：对于图像，纵横比与替换的图像 Note2 相同：必须拥有源媒体文件，因为它们需要计算其 CRC 才能在 docx 中查找它们。（虚拟文件名并不重要）

替换dummy_header_pic.jpg：

```
tpl.replace_media('dummy_header_pic.jpg','header_pic_i_want.jpg')
```

警告：与 replace_pic（） 方法不同，dummy_header_pic.jpg和保存生成的 docx 时，模板目录中必须存在此方法。它必须与在 docx 模板中手动插入的文件相同。替换发生在页眉、页脚和整个文档的正文中。



### 替换嵌入对象

它的工作方式与介质替换类似，只不过它适用于嵌入式文档等嵌入对象。

替换embedded_dummy.docx：

```
tpl.replace_embedded('embdded_dummy.docx','embdded_docx_i_want.docx')
```

警告：与 replace_pic（） 方法不同，embdded_dummy.docx和保存生成的 docx 时，模板目录中必须存在。它必须与在 docx 模板中手动插入的文件相同。替换发生在页眉、页脚和整个文档的正文中。

请注意，replace_embedded（））可能不能处理嵌入文档以外的其他文档。相反，您应该使用 zipname 替换：

```
tpl.replace_zipname(
    'word/embeddings/Feuille_Microsoft_Office_Excel1.xlsx',
    'my_excel_file.xlsx')
```

当您使用 WinZip、7zip （Windows） 或解压缩 -l （Linux） 打开文档时，可以找到 zipname。邮政编码以"单词/嵌入/"开头。请注意，要替换的文件由 MSWord 重命名，因此您必须猜测一点点...

这适用于嵌入的 MSWord 文件（如 Excel 或 PowerPoint 文件），但无法用于其他文件（如 PDF、Python 甚至文本文件）：对于这些文件，MSWord 会生成一个 oleObjectNNN.bin 文件，该文件在编码时无需替换。



### 微软 Word 2016 特殊情况

MS Word 2016 将忽略制表。这是该版本的特别。自由办公室或 Wordpad 没有此问题。对于以 jinja2 标记表示空格开头的行，也会发生同样的事情：它们将被忽略。要解决这些问题，解决方案是使用 Richtext：`\t`

```
tpl.render({
    'test_space_r' : RichText('          '),
    'test_tabs_r': RichText(5*'\t'),
})
```

在模板中，使用\r 表示法：

```
{{r test_space_r}} Spaces will be preserved
{{r test_tabs_r}} Tabs will be displayed
```



### 表

通过使用标记（请参阅测试/测试/测试），可以通过两种方式水平跨表dynamic_table.py）：`colspan`

```
{% colspan <number of column to span> %}
```

或在 for 循环中（请参阅测试/horizontal_merge.py）：

```
{% hm %}
```

您还可以在 for 循环中垂直合并单元格（请参阅测试/vertical_merge.py）：

```
{% vm %}
```



### 金贾自定义过滤器

```
render()`接受选项参数：您可以传递 jinja 环境对象。这样，您将能够添加一些自定义 jinja 筛选器：`jinja_env
from docxtpl import DocxTemplate
import jinja2

def multiply_by(value, by):
   return value * by

doc = DocxTemplate("my_word_template.docx")
context = { 'price_dollars' : 5.00 }
jinja_env = jinja2.Environment()
jinja_env.filters['multiply_by'] = multiply_by
doc.render(context,jinja_env)
doc.save("generated_doc.docx")
```

然后，在模板中，您将能够使用：

```
Euros price : {{ price_dollars|multiply_by(0.88) }}
```



### 例子

查看其工作原理的最佳方法是读取示例，它们位于测试/目录中。Docx 测试模板在测试/模板/中。要生成最终文档文件：

```
cd tests/
python runtests.py
```

生成的文件位于测试/输出目录中。

如果您不确定 python 环境，python-docx 模板会为此提供 Pipfile：

```
pip install pipenv (if not already done)
cd python-docx-template (where Pipfiles are)
pipenv install --python 3.6 -d
pipenv shell
cd tests/
python runtests.py
```



###  