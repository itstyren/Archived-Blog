---
layout: post
title: Python写爬虫常用网页解析工具
categories: Python
description: Python写爬虫常用网页解析工具
keywords: python 爬虫 XPATH Beautiful Soup 
---


**参考资料：**

[W3C关于Xpah的教程](http://www.w3school.com.cn/xpath/index.asp)

[阮一峰关于Xpath的文章](http://www.ruanyifeng.com/blog/2009/07/xpath_path_expressions.html)

[崔庆才关于lxml的博客](https://cuiqingcai.com/2621.html)

[lxml python 官方文档](https://lxml.de/index.html)

[Beautiful Soup官方文档](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/)

[崔庆才关于bs的博客](https://cuiqingcai.com/1319.html)

[pyquery](https://pythonhosted.org/pyquery/)

* * *

>&emsp;&emsp;网页解析可以用正则表达式解析，也可以用解析库解析，利用其强大的方法可以完成各种解析，比如XPath解析以及CSS选择器解析。

[TOC]

### 1 Xpath及lxml

#### 1.1 什么是Xpath

&emsp;&emsp;XPath全称为XML Path Language，即XML路径语言。是一门在 XML 文档中查找信息的语言。XPath 可用来在 XML 文档中对元素和属性进行遍历。他是一个标准的函数库，也是W3C的一个标准。
　　它最初是用在搜寻XML文档的，但是它**同样适用于HTML文档的检索**。　　
  
#### 1.2 节点

&emsp;&emsp; **简单说，xpath就是选择XML文件中节点的方法。** 所谓节点（node），就是XML文件的最小构成单位，一共分成7种。
- element（元素节点）
- attribute（属性节点）
- text （文本节点）
- namespace （名称空间节点）
- processing-instruction （处理命令节点）
- comment （注释节点）
- root （根节点）

#### 1.3 路径表达式

&emsp;&emsp; Xpath通过"路径表达式"（Path Expression）来选择节点。在形式上，"路径表达式"与传统的文件系统非常类似。其基本格式如下：

- 斜杠（/）：作为路径内部的分割符,每一步都被斜杠分割。
- 同一个节点有绝对路径和相对路径两种写法（路径的表示方法写在表达式的最开头）。
- 绝对路径（ / ）：从根节点选取
- 相对路径（//）：从任意位置选择节点，也可以不写//也不写也表示相对路径。

其中用/分割的成为步（step），步的语法如下：
>    轴名称::节点测试[谓语]

* 轴（axis）：定义所选节点与当前节点之间的树关系
* 节点测试（node-test）：识别某个轴内部的节点零个或者更多
* 谓语（predicate）：更深入地提炼所选的节点集

```
/html/body/form/input      选取所有input节点
 //input                            选取所有的input节点
```

#### 1.4 选择节点的基本规则


| **规则** | **描述**  |
| --- | --- |
| **nodename** | 选取此节点的所有子节点 |
| **@** | 选取属性  |
| **.** | 选取当前节点。 | 
| **..** | 选取当前节点的父节点。|

#### 1.5 谓语条件

&emsp;&emsp; 所谓"谓语条件"，就是对**路径表达式的附加条件**。所有的条件，都写在**方括号"[]"** 中，表示对节点进行进一步的筛选。

示例如下：

| **路径表达式** | **结果** |
| --- | --- |
| /bookstore/book[1] | 选取属于 bookstore 子元素的第一个 book 元素。|
| /bookstore/book[last()] | 选取属于 bookstore 子元素的最后一个 book 元素。
| /bookstore/book[last()-1] | 选取属于 bookstore 子元素的倒数第二个 book 元素。
| /bookstore/book[position()<3] | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。| 
| //title[@lang] |选取所有拥有名为 lang 的属性的 title 元素。|
/bookstore/book[price>35.00] | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于35.00。| 
| /bookstore/book[price>35.00]/title |选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。|

#### 1.6 通配符

- " * "：表示匹配任何元素节点。
- " @* "：表示匹配任何属性值。
-  node()：表示匹配任何类型的节点。

#### 1.7 节点轴选择器

&emsp;&emsp; 通过Xpath提供的轴选择器方法，可以获取子元素、兄弟元素、父元素、祖先元素等。
　　其中，轴可定义相对于当前节点的节点集。可用轴如下：
  
| **轴名称** | **结果** |
| --- | --- |
| **ancestor** | 选取当前节点的所有祖先节点。|
| **ancestor-or-self** | 选取当前节点的所有祖先节点以及当前节点本身。|
| **attribute**| 选取当前节点的所有属性。| 
| **child** | 选取当前节点的所有子元素 。|  
| **descendant** | 选取当前节点的所有后代元素。|
| **descendant-or-self** | 选取当前节点的所有后代元素以及当前节点本身。| 
| **following** |选取文档中当前节点的结束标签之后的所有节点。|
| **namespace** | 选取当前节点的所有命名空间节点。|
| **parent** | 选取当前节点的父节点。|
| **preceding** | 选取文档中当前节点的开始标签之前的所有节点。||
|**preceding-sibling** | 选取当前节点之前的所有同级节点。self选取当前节点。|


示例如下：

|**例子** |**结果**|
|---|---|
|**child::book**|选取所有属于当前节点的子元素的 book 节点。|
|**attribute::lang**|选取当前节点的 lang 属性。|
|**child::*** | 选取当前节点的所有子元素|


#### 1.8 lxml插件的使用
##### (1) 安装lxml

`pip install lxml`

##### (2) 读取和初始化

&emsp;&emsp; 首先我们使用 lxml 的 **etree 库**，然后利用**etree.HTML** 初始化，然后我们将其打印出来。在这里初始化库可以帮我们自动修正代码。

```python

from lxml import etree
text = '''
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </ul>
</div>
'''
html = etree.HTML(text)
result = etree.tostring(html)
print(result)

```
&emsp;&emsp;我们同样可以使用**etree.parase()**,传入两个参数：

* 第一个是文件的相对路径
* 第二个是etree,HTMLParser()

##### (3)利用xpath匹配
&emsp;&emsp;初始化完成后可以使用xpath获取节点信息，示例如下：
```python
from lxml import etree
html = etree.parse('hello.html',etree.HTMLParaser())
print type(html)
result = html.xpath('//li')
print result
print len(result)
print type(result)
print type(result[0])

# 结果如下
<type 'lxml.etree._ElementTree'>
[<Element li at 0x1014e0e18>, <Element li at 0x1014e0ef0>, <Element li at 0x1014e0f38>, <Element li at 0x1014e0f80>, <Element li at 0x1014e0fc8>]
5
<type 'list'>
<type 'lxml.etree._Element'>
```

### 2  Beautiful Soup
&emsp;&emsp;Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库。它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式。
#### 2.1 与lxml区别
&emsp;&emsp;相比与，lxml，BeautifulSoup是Python开发的，同时原理不同，**BeautifulSoup是基于DOM的**，会载入整个文档，解析整个DOM树，因此时间和内存开销都会大很多。而lxml只会局部遍历。
　　使用xpath 要求一定清楚文档层次结构，它通过元素和属性进行导航，可以使用绝对路径或相对路径查找，而beautifulsoup 不必清楚文档结构，可以直接找某些标签。
  
#### 2.2 Beautiful Soup初始化

##### （1）安装bs
　　`pip install beautifulsoup4`
&emsp;&emsp;现在的项目中使用Beautiful Soup 4，不过它已经被移植到BS4了，也就是说导入时 **我们需要 import bs4** 。

##### （2）解析器的选择
&emsp;&emsp;Beautiful Soup在解析时需要依赖解析器，它支持的解析器如下表所示

| **解析器** | **使用方法** | **优势**  | **劣势** |
|---|---|---|---|
| Python标准库 | BeautifulSoup(markup, "html.parser")|Python的内置标准库执行速度适中文档容错能力强 | Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差|
| **lxml HTML 解析器** | BeautifulSoup(markup, "lxml") | 速度快文档容错能力强 | 需要安装C语言库|
| **lxml XML 解析器** | BeautifulSoup(markup, ["lxml-xml"])BeautifulSoup(markup, "xml") | 速度快唯一支持XML的解析器 | 需要安装C语言库 |
| html5lib | BeautifulSoup(markup, "html5lib") | 最好的容错性以浏览器的方式解析文档生成HTML5格式的文档| 速度慢不依赖外部扩展 |

&emsp;&emsp;推荐使用lxml解析器，他的效率更高。

#### 2.3 Beautiful Soup对象种类

&emsp;&emsp;Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种: **Tag** , **NavigableString** , **BeautifulSoup** , **Comment** 。
##### （1）BeautifulSoup
&emsp;&emsp;BeautifulSoup 对象表示的是一个文档的全部内容.大部分时候,可以把它当作 Tag 对象。

##### （2）Tag
&emsp;&emsp;其代表的就是HTML或XML原生文档中的节点。

##### （3）NavigableString
&emsp;&emsp;代表来包装tag中的字符串。

##### （4）Comment
&emsp;&emsp;Comment 对象是一个特殊类型的 NavigableString 对象，其代表了文档中的注释及特殊字符串。

#### 2.4 遍历文档树
##### 2.4.1 基础用法
###### （1）节点名字选择

&emsp;&emsp;操作文档树最简单的方法就是告诉它你想获取的tag的name。
```python
soup.body.b  
# <b>The Dormouse's story</b>
```
###### （2）获取内容

&emsp;&emsp;提取信息方式有一下三种属性：

* **.string**: tag只有一个 NavigableString 类型子节点。（如果调用string属性的tag包含了多个子节点,tag就无法确定 .string 方法应该调用哪个子节点的内容, .string 的输出结果是 None有）
* **.strings**: 获取多个内容，不过需要遍历获取。
* **.stripped_strings**: 输出的字符串中可能包含了很多空格或空行,使用 该属性去除多余空白内容。
&emsp;&emsp;示例代码如下：

```python

head_tag.string
# u'The Dormouse's story'

for string in soup.strings:
    print(repr(string))
    # u"The Dormouse's story"
    # u'\n\n'
    # u"The Dormouse's story"
    # u'\n\n'
    
for string in soup.stripped_strings:
    print(repr(string))
    # u"The Dormouse's story"
    # u"The Dormouse's story"
```

##### 2.4.2 子节点与子孙节点
###### （1）.contents 和 .childrens
&emsp;&emsp;这两个属性得到的都是当前节点的直接子节点，不同在于**前者**是直接以列表形式输出,可以利用索引获取元素；**后者**返回的是一个 list 生成器对象。如下实例所示

```python

print soup.head.contents 
#[<title>The Dormouse's story</title>]

for child in  soup.body.children:
    print child

#<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
#<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
```
###### （2）.descendants
&emsp;&emsp;返回的同样是**生成器**，但是是返回了所有子孙节点的生成器。
```python
for child in head_tag.descendants:
    print(child)
    # <title>The Dormouse's story</title>
    # The Dormouse's story
```
##### 2.4.3 父节点和祖先节点
###### （1）.parent
&emsp;&emsp;获取某个元素的父节点。

###### （2） .parents
&emsp;&emsp;获取所有的祖先节点，返回的同样是列表生成器。

##### 2.4.4 兄弟节点
###### （1）.next_sibling 和 .previous_sibling
&emsp;&emsp;分别用来获取节点的下一个或者上一个兄弟元素。

###### （2）.next_siblings 和 .previous_siblings
&emsp;&emsp;分别返回后面和前面的兄弟节点，这里是所有兄弟节点，以生成器形式返回。

```python

for sibling in soup.a.next_siblings:
    print(repr(sibling))
    # u',\n'
    # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    # u' and\n'
```

#### 2.5 搜索文档树
##### 2.5.1 find_all()
&emsp;&emsp;find_all() 方法搜索当前tag的所有tag子节点,并判断是否符合过滤器的条件。
　　参数格式如下：
  `find_all( name , attrs , recursive , text , **kwargs )`
 

**（1）name 参数**
&emsp;&emsp;查找所有名字为 name 的tag,字符串对象会被自动忽略掉。其中可以传入的形式如下：

* **字符串**

```python
soup.find_all('b')
# [<b>The Dormouse's story</b>]
```
* **正则表达式**
Beautiful Soup会通过正则表达式的 match() 来匹配内容

```python
import re
for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
# body
# b
```
* **列表**
Bautiful Soup会将与列表中任一元素匹配的内容返回.
```python
soup.find_all(["a", "b"])
# [<b>The Dormouse's story</b>,
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
```

* **True**
True 可以匹配任何值,下面代码查找到所有的tag,但是不会返回字符串节点。
```python
for tag in soup.find_all(True):
    print(tag.name)
    # html# head# title# body# p# b
```

* **方法**
如果没有合适过滤器,那么还可以定义一个方法,方法只接受一个元素参数  ,如果这个方法返回 True 表示当前元素匹配并且被找到,如果不是则反回 False。
```python
def has_class_but_no_id(tag):
    return tag.has_attr('class') and not tag.has_attr('id')
    soup.find_all(has_class_but_no_id)
# [<p class="title"><b>The Dormouse's story</b></p>,
#  <p class="story">Once upon a time there were...</p>,
#  <p class="story">...</p>]
```

**（2）keyword 参数**
&emsp;&emsp;如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作指定名字tag的属性来搜索。
```python
soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```
&emsp;&emsp;搜索指定名字的属性时可以使用的参数值包括:字符串 , 正则表达式 , 列表, True。
&emsp;&emsp;使用多个指定名字的参数可以同时过滤tag的多个属性。
```python
soup.find_all(href=re.compile("elsie"), id='link1')
# [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]
```
>class 在Python中是保留字,应通过 class_ 参数搜索有指定CSS类名的tag，其也可以接受不同类型的过滤器。


**（3）attrs参数**
&emsp;&emsp;attrs 参数定义一个字典参数来搜索包含特殊属性的tag，一般的id、name等属性也可以放在里面。
```python
data_soup.find_all(attrs={"data-foo": "value"})# [<div data-foo="value">foo!</div>]
```

**（4）string 参数**
&emsp;&emsp;attrs通过 string 参数可以搜搜文档中的字符串内容，其可接受过滤器与name相同。其可以与其他参数混合使用。比如，下面代码用来搜索内容里面包含“Elsie”的a标签:
```python
soup.find_all("a", string="Elsie")# [<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>]
```
**(4）limit 参数**
&emsp;&emsp;attrs使用 limit 参数限制返回结果的数量。当搜索到的结果数量达到 limit 的限制时,就停止搜索返回结果。


**（5）recursive 参数**
&emsp;&emsp;若只想搜索tag的直接子节点,可以使用参数 recursive=False .

##### 2.5.2 简写find_all()
&emsp;&emsp;find_all() 几乎是Beautiful Soup中最常用的搜索方法,所以我们定义了它的简写方法.。BeautifulSoup 对象和 tag 对象可以被当作一个方法来使用，下面两行代码是等价的:
```python
soup.title.find_all(string=True)
soup.title(string=True)
```

##### 2.5.3 find（）
&emsp;&emsp;它与 find_all() 方法唯一的区别是 find_all() 方法的返回结果是值包含一个元素的列表,而 find() 方法直接返回结果

##### 2.5.4 CSS选择器
&emsp;&emsp;除恶利用find_all()，在这里我们也可以利用soup.select()通过选择器筛选元素，返回类型是 list。
###### （1）通过标签名
```python
print soup.select('title') 
#[<title>The Dormouse's story</title>]
```
###### （2）通过类名
```python
print soup.select('.sister')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, ]
```
###### （3）通过id名
```python
print soup.select('#link1')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
```
###### （4）组合查找
&emsp;&emsp;组合原理和我们写css时候一样，如下
```python
print soup.select('p #link1')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
```
###### （5）属性查找
&emsp;&emsp;属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。
```python
print soup.select('a[href="http://example.com/elsie"]')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
```
### 3 pyquery
这个我觉得用不上，不整理了。