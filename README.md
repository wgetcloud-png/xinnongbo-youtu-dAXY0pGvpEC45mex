
# BeautifulSoup(bs4\)



> BeautifulSoup是python的一个库,最主要的功能是从网页爬取数据,官方是这样解释的:BeautifulSoup提供一些简单,python式函数来处理导航,搜索,修改分析树等功能,其是一个工具库,通过解析文档为用户提供需要抓取的数据,**因为简单,所有不需要多少代码就可以写出一个完整的程序**


# bs4安装



> 直接使用`pip install`命令安装



```
pip install beautifulsoup4

```

# lxml解析器



> `lxml`是一个高性能的`Python`库,用于处理`XML`与`HTML`文档,与bs4相比之下lxml具有更强大的功能与更高的性能,特别是处理大型文档时尤为明显.`lxml`可以与`bs4`结合使用,也可以单独使用


## lxml安装



> 同样使用`pip install` 安装



```
pip install lxml

```

其用于在接下来会结合bs4进行讲解


# BeautifulSoup浏览浏览器结构化方法


* `.title`:获取title标签



```
html_doc="""....
""""
# 创建beautifulsoup对象 解析器为lxml
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.title)
#output->The Dormouse's story

```
* `.name`获取文件或标签类型名称



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.title.name)
print(soup.name)
#output->title
#[document]

```
* `.string/.text`:获取标签中的文字内容



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.title.string)
print(soup.title.text)
#output->The Dormouse's story
#The Dormouse's story

```
* `.p`:获取标签



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.p)
#output->The Dormouse's story



```
* `.find_all(name,attrs={})`:获取所有标签,参数:标签名,如`’a’`a标签,`’p’`p标签等等,`attrs={}`:属性值筛选器字典如`attrs={'class': 'story'}`



```
# 创建beautifulsoup对象 解析器为lxml
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all('p'))
print(soup.find_all('p', attrs={'class': 'title'}))


```
* `.find(name,attrs={})`:获取第一次匹配条件的元素



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find(id="link1"))
#output->[Elsie](https://example.com/elsie)

```
* `.parent`:获取父级标签



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.title.parent)
#output->The Dormouse's story

```
* `.p['class']` :获取class的值



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.p["class"])
#output->['title']

```
* `.get_text()`:获取文档中所有文字内容



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.get_text())
The Dormouse's story

The Dormouse's story
Once upon a time there were three little sisters; and their names were
Elsie,
Lacie and
Tillie;
and they lived at the bottom of a well.
...

```
* 从文档中找到所有标签的链接



```
a_tags = soup.find_all('a')
for a_tag in a_tags:
    print(a_tag.get("href"))
#output->https://example.com/elsie
#https://example.com/lacie
#https://example.com/tillie

```


# BeautifulSoup的对象种类



> 当你使用`BeautifulSoup` 解析一个HTML或XML文档时,BeautifulSoup会整个文档转换为一个树形结构,其中每个结点(标签,文本,注释)都被表示为一个python对象


## BeautifulSoup的树形结构



> 在HTML文档中,根结点通常是标签,其余的标签和文本内容则是其子结点


若有以下一个HTML文档:



```
<html>
    <head>
        <title>The Dormouse's storytitle>
    head>
    <body>
        <h1>The Dormouse's storyh1>
        <p>Once upon a time...p>
    body>
html>

```

* 经过BeautifulSoup的解析后,是根结点,与相邻的与是其子结点,同理可得是子结点,`#`与是子结点


## 对象类型



> BeautifulSoup有四种主要类型,`Tag`,`NavigableString`,`BeautifulSoup`,`Comment`


### Tag



> **`Tag`对象与HTML或XML原生文档中的标签相同**,每个`Tag`对象都可以包含其他标签,文本内容和属性



```
soup = BeautifulSoup(html_doc, 'lxml')
tag = soup.title
print(type(tag))
#output->

```

### NavigableString



> `NavigableString`对象表示标签内的文本内容,是一个不可变字符串,可以提供`Tag`对象的`.string`获取



```
soup = BeautifulSoup(html_doc, 'lxml')
tag = soup.title
print(type(tag.string))
#output-> 

```

### BeautifulSoup



> `BeautifulSoup`对象表示整个文档的内容.其可以被视为一个特殊的Tag对象,但没有名称与属性.其提供了对整个文档的遍历,搜索和修改的功能



```
soup = BeautifulSoup(html_doc, 'lxml')
print(type(soup))
#output-> 

```

### Comment



> Comment对象是一个特殊类型的NavigableString对象,表示HTML和XML中的注释部分



```
# 
soup = BeautifulSoup(html_doc, 'lxml')
print(type(soup.b.string))
#output-> 

```

# BeautifulSoup遍历文档树



> `BeautifulSoup`提供了许多方法来遍历解析后的文档树


### 导航父节点


* `.parent`与`.parents`:`.parent`可以获取当前节点的上一级父节点,`.parents`可以遍历获取当前节点的所有父辈节点



```
soup = BeautifulSoup(html_doc, 'lxml')
title_tag = soup.title
print(title_tag.parent)
#The Dormouse's story

```


```
soup = BeautifulSoup(html_doc, 'lxml')
body_tag = soup.body
for parent in body_tag.parents:
    print(parent)
#The Dormouse's story
#
#The Dormouse's story


#Once upon a time there were three little sisters; and their names were


#[Elsie](https://example.com/elsie),
#....

```

### 导航子结点


* `.contents`:可以获取当前结点的所有子结点



```
soup = BeautifulSoup(html_doc, 'lxml')
head_contents = soup.head.contents
print(head_contents)
#output-> [The Dormouse's story]

```

* `.children`:可以遍历当前结点的所有子结点,返回一个list



```
soup = BeautifulSoup(html_doc, 'lxml')
body_children = soup.body.children

for child in body_children:
    print(child)
#output->The Dormouse's story


#[Elsie](https://example.com/elsie),
#[Tillie](https://example.com/tillie);
#and they lived at the bottom of a well.
#.....

```

* 字符串没有`.children`与`.contents`属性


### 导航所有后代节点



> `.contents`与`.children`属性仅包含tag直接子结点,例如标签只有一个直接子结点



```
#[The Dormouse's story]

```

但标签也包含一个子结点:字符串”The Dormouse's story”,字符串”The Dormouse's story”是标签的子孙结点


* `.descendants`属性可以遍历当前结点的所有后代结点(层遍历)



```
soup = BeautifulSoup(html_doc, 'lxml')
for descendant in soup.descendants:
    print(descendant)

```

### 节点内容


* **`.string`**


	+ 如果tag只有一个`NavigableString`类型子节点,那么这个tag可以使用.string得到其子节点.
	
	
	
	```
	soup = BeautifulSoup(html_doc, 'lxml')
	print(soup.head.string)
	#The Dormouse's story
	print(soup.title.string)
	#The Dormouse's story
	
	```
	+ 但若tag中包含了多个子节点,tag就无法确定string方法应该调用哪一个字节的内容,则会输出None
	
	
	
	```
	soup = BeautifulSoup(html_doc, 'lxml')
	print(soup.body.string)
	#None
	
	```
* `.strings`和`.stripped_strings`


	+ `.strings`可以遍历获取标签中的所有文本内容,`.stripped_strings`可以除去多余的空白字符
```
soup = BeautifulSoup(html_doc, 'lxml')
for string in soup.strings:
    print(string)
#The Dormouse's story
......

#The Dormouse's story

```


```
soup = BeautifulSoup(html_doc, 'lxml')
for string in soup.stripped_strings:
    print(string)
#The Dormouse's story
#The Dormouse's story
#Once upon a time there were three little sisters; and their names were
#Elsie
#,
...

```


# BeautifulSoup搜索文档树



> `BeautifulSoup`提供了多种方法来搜索解析后的文档树


## find\_all(name , attrs , recursive , string , \*\*kwargs)


* `find_all()`方法搜索当前tag的所有tag子节点



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all("title"))  # 查找所有的title标签
print(soup.find_all("p", "title"))  # 查找p标签中class为title的标签

print(soup.find_all("a"))  # 查找所有的a标签

print(soup.find_all(id="link2"))  # 查找id为link2的标签
#[The Dormouse's story]
#[The Dormouse's story

]
#[[Elsie](https://example.com/elsie), [Lacie](https://example.com/lacie), [Tillie](https://example.com/tillie)]
#[[Lacie](https://example.com/lacie)]

```


> 接下来我们来详细解析一下每个参数的含义


## name参数



> `name`参数可以查找所有名字为name的tag,字符串对象名字会自动被忽略


* eg



```
soup.find_all("title")
# [The Dormouse's story]

```
* name参数可以为任意类型的过滤器,如字符串,正则表达式,列表,方法等等
* **传字符串**



> 传入字符串是最简单的过滤器,在搜索方法中传入一个字符串参数,BeautifulSoup会查找与字符串匹配的内容


	+ 下面的例子用于查找文档中所有的标签
```
soup.find_all('b')
# [The Dormouse's story]

```
* **传入正则表达式**



> 若传入正则表达式作为参数,BeautifulSoup会通过正则表达式match()来匹配内容


	+ 查找b开头的标签,这表示和**标签都应该被找到**
```
soup = BeautifulSoup(html_doc, 'lxml')
for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
# body
# b

```
* **传入列表**



> 如果传入列表参数,Beautiful Soup会将与列表中任一元素匹配的内容返回


	+ 找到文档中所有标签和标签
```
soup = BeautifulSoup(html_doc, 'lxml')
for tag in soup.find_all(['a', 'b']):
    print(tag.name)
#b
#a
#a
#a
#b

```


## \*\***kwargs参数**



> 在BeautifulSoup中,`**kwargs`(即关键字参数)可用于通过标签的属性来查找特定的标签.这些关键字参数可以直接传递给`find`,`find_all`方法,使得搜索更加强大.标签的属性名作为关键字参数，值可以是字符串、正则表达式或列表


### 使用字典


* 可以使用`key=’word’`传入参数



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(id='link1'))
#[[Elsie](https://example.com/elsie)]


```

### 使用正则表达式


* 使用`Python`的`re`模块中的正则表达式来匹配属性值,使搜索更灵活



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all('a', href=re.compile("elsie")))  # 查找href属性中包含elsie的a标签
print(soup.find_all(string=re.compile("^The")))  # 查找文本中The开头的标签
#[[Elsie](https://example.com/elsie)]
#["The Dormouse's story", "The Dormouse's story"]

```

### 使用列表


* 可以传递一个列表作为关键字参数的值.`BeautifulSoup`会匹配列表中的任意一个值



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find('a', id=['link1', 'link2']))  # 查找id为link1或者link2的a标签
print(soup.find_all(class_=['sister', 'story']))  # 查找class为sister或者story的标签
#[Elsie](https://example.com/elsie)
#[Once upon a time there were three little sisters; and their names were


#...

```

### 特殊属性名称



> HTML的属性名称与Python的保留字冲突,为了防止冲突,BeautifulSoup提供了一些特殊的替代名称


* `class_`:用于匹配`class`属性
* `data-*`:用于匹配自定义的`data-*`属性



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all('p', class_="title"))  # 查找所有class为title的p标签
print(soup.find_all('p', attrs={'data-p', 'story'}))  # 查找所有class为story的p标签
#[The Dormouse's story

]
#[Once upon a time there were three little sisters; and their names were


#[Elsie](https://example.com/elsie),
#[Lacie](https://example.com/lacie) and

```

## text/string参数



> `text/string`参数允许操作者根据标签的文本内容进行搜索,与`name`参数类似,`text`参数也支持多种类型的值,包括正则表达式,字符串列表和`True`,早期`bs4`支持`text`,近期`bs4`将`text`都改为`string`


### 使用字符串匹配



> 你可以直接传递一个字符串作为 `string`参数的值，BeautifulSoup 会查找所有包含该字符串的标签



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(string='Elsie'))
#['Elsie']


```

### 使用正则表达式匹配



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(string=re.compile('sister'), limit=2))  # 查找前两个包含sister的字符串
print(soup.find_all(string=re.compile('Dormouse')))  # 查找包含Dormouse的字符串
#['Once upon a time there were three little sisters; and their names were\n']
#["The Dormouse's story", "The Dormouse's story"]


```

### 使用列表匹配



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(string=['Elsie', 'Lacie', 'Tillie']))
#['Elsie', 'Lacie', 'Tillie']

```

## limit参数



> `BeautifulSoup`中的`limit`参数用于限制`find_all`方法结果的返回数量,当只需要查询前几个标签时,使用limit参数可以提高搜索搜索效率,效果与SQL中的limit关键字类似,当搜索到的结果数量达到 `limit` 的限制时,就停止搜索返回结果



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all('a', limit=2))  # 查找所有a标签，限制输出2个
#[[Elsie](https://example.com/elsie), [Lacie](https://example.com/lacie)]


```

## **find\_parents() 和 find\_parent()**



> `BeautifulSoup` 提供了 `find_parents()` 和 `find_parent()` 方法,用于在解析后的文档树中向上查找父标签.两个方法的主要区别在于返回的结果数量


* `find_parent(name=None, attrs={}, **kwargs)`:只返回最接近的父标签(即第一个匹配的父标签)
* `find_parents(name=None, attrs={}, limit=None, **kwargs)`:返回所有符合条件的祖先标签,按从近到远的顺序排列



```
soup = BeautifulSoup(html_doc, 'lxml')
a_string = soup.find(string='Lacie')
print(a_string.find_parent())  # 查找父节点
print('-----------------')
print(a_string.find_parents())  # 查找所有父节点
#[Lacie](https://example.com/lacie)
#-----------------
#[[Lacie](https://example.com/lacie), Once upon a time there were three little sisters; and their names were


#[Elsie](https://example.com/elsie),
#[Lacie](https://example.com/lacie) and
#and they lived at the bottom of a well., ....]

```

# BeautifulSoup的CSS选择器



> 我们在写CSS时,标签名不加任何修饰,类名前加点,id名前加\#,BeautifulSoup中也可以使用类似的方法来筛选元素,


## select(selector, namespaces\=None, limit\=None, \*\*kwargs)



> `BeautifulSoup`中的`select()`方法允许使用CSS选择器来查找HTML文档元素,其返回一个包含所有匹配元素的列表类似与`find_all()`方法


* selector:一个字符串,表示将要选择的CSS选择器,可以是简单标签选择器,类选择器,id选择器


### 通过标签名查找



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('b'))
#[The Dormouse's story, ]

```

### 通过类名查找



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('.title'))
#[The Dormouse's story

]

```

### **id名查找**



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('#link1'))
#[[Elsie](https://example.com/elsie)]

```

### **组合查找**


* 组合查找即与写class时一致,标签名与类名id名进行组合的原理一样


eg:查找p标签中id为link1的内容



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('p #link1'))
#[[Elsie](https://example.com/elsie)]

```

* 查找类选择器时也可以使用id选择器的标签



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('.story#text'))

```

* 查找有多个class选择器和一个id选择器的标签



```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select(".story .sister#link1"))
#[[Elsie](https://example.com/elsie)]

```

### **属性查找**



> 选择具有特定属性或属性值的标签


* **简单属性选择器**


	+ 选择具有特定属性的标签
```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select("a[href='https://example.com/elsie']"))  # 选择a标签中href属性为https://example.com/elsie的标签
#[[Elsie](https://example.com/elsie)]

```
* **属性值选择器**



> 选择具有特定属性值的标签


	+ 精确匹配:`[attribute="value"]`
	+ 部分匹配
		- 包含特定值:`[attribute~="value"]` 选择属性值包含特定单词的标签。
		- 以特定值开头:`[attribute^="value"]` 选择属性值以特定字符串开头的标签
		- 以特定值结尾:`[attribute$="value"]` 选择属性值以特定字符串结尾的标签。
		- 包含特定子字符串:`[attribute*="value"]` 选择属性值包含特定子字符串的标签
```
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.select('a[href^="https://example.com"]'))  # 选择href以https://example.com开头的a标签
#[[Elsie](https://example.com/elsie), [Lacie](https://example.com/lacie), [Tillie](https://example.com/tillie)]

```


  * [BeautifulSoup(bs4\)](#beautifulsoupbs4)
* [bs4安装](#bs4%E5%AE%89%E8%A3%85)
* [lxml解析器](#lxml%E8%A7%A3%E6%9E%90%E5%99%A8)
* [lxml安装](#lxml%E5%AE%89%E8%A3%85)
* [BeautifulSoup浏览浏览器结构化方法](#beautifulsoup%E6%B5%8F%E8%A7%88%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BB%93%E6%9E%84%E5%8C%96%E6%96%B9%E6%B3%95)
* [BeautifulSoup的对象种类](#beautifulsoup%E7%9A%84%E5%AF%B9%E8%B1%A1%E7%A7%8D%E7%B1%BB)
* [BeautifulSoup的树形结构](#beautifulsoup%E7%9A%84%E6%A0%91%E5%BD%A2%E7%BB%93%E6%9E%84)
* [对象类型](#%E5%AF%B9%E8%B1%A1%E7%B1%BB%E5%9E%8B)
* [Tag](#tag):[veee加速器](https://youhaochi.com)
* [NavigableString](#navigablestring)
* [BeautifulSoup](#beautifulsoup)
* [Comment](#comment)
* [BeautifulSoup遍历文档树](#beautifulsoup%E9%81%8D%E5%8E%86%E6%96%87%E6%A1%A3%E6%A0%91)
* [导航父节点](#%E5%AF%BC%E8%88%AA%E7%88%B6%E8%8A%82%E7%82%B9)
* [导航子结点](#%E5%AF%BC%E8%88%AA%E5%AD%90%E7%BB%93%E7%82%B9)
* [导航所有后代节点](#%E5%AF%BC%E8%88%AA%E6%89%80%E6%9C%89%E5%90%8E%E4%BB%A3%E8%8A%82%E7%82%B9)
* [节点内容](#%E8%8A%82%E7%82%B9%E5%86%85%E5%AE%B9)
* [BeautifulSoup搜索文档树](#beautifulsoup%E6%90%9C%E7%B4%A2%E6%96%87%E6%A1%A3%E6%A0%91)
* [find\_all(name , attrs , recursive , string , \*\*kwargs)](#find_allname--attrs--recursive--string--kwargs)
* [name参数](#name%E5%8F%82%E6%95%B0)
* [\*\*kwargs参数](#kwargs%E5%8F%82%E6%95%B0)
* [使用字典](#%E4%BD%BF%E7%94%A8%E5%AD%97%E5%85%B8)
* [使用正则表达式](#%E4%BD%BF%E7%94%A8%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)
* [使用列表](#%E4%BD%BF%E7%94%A8%E5%88%97%E8%A1%A8)
* [特殊属性名称](#%E7%89%B9%E6%AE%8A%E5%B1%9E%E6%80%A7%E5%90%8D%E7%A7%B0)
* [text/string参数](#textstring%E5%8F%82%E6%95%B0)
* [使用字符串匹配](#%E4%BD%BF%E7%94%A8%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8C%B9%E9%85%8D)
* [使用正则表达式匹配](#%E4%BD%BF%E7%94%A8%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%8C%B9%E9%85%8D)
* [使用列表匹配](#%E4%BD%BF%E7%94%A8%E5%88%97%E8%A1%A8%E5%8C%B9%E9%85%8D)
* [limit参数](#limit%E5%8F%82%E6%95%B0)
* [find\_parents() 和 find\_parent()](#find_parents-%E5%92%8C-find_parent)
* [BeautifulSoup的CSS选择器](#beautifulsoup%E7%9A%84css%E9%80%89%E6%8B%A9%E5%99%A8)
* [select(selector, namespaces\=None, limit\=None, \*\*kwargs)](#selectselector-namespacesnone-limitnone-kwargs)
* [通过标签名查找](#%E9%80%9A%E8%BF%87%E6%A0%87%E7%AD%BE%E5%90%8D%E6%9F%A5%E6%89%BE)
* [通过类名查找](#%E9%80%9A%E8%BF%87%E7%B1%BB%E5%90%8D%E6%9F%A5%E6%89%BE)
* [id名查找](#id%E5%90%8D%E6%9F%A5%E6%89%BE)
* [组合查找](#%E7%BB%84%E5%90%88%E6%9F%A5%E6%89%BE)
* [属性查找](#%E5%B1%9E%E6%80%A7%E6%9F%A5%E6%89%BE)

   \_\_EOF\_\_

       - **本文作者：** [ihave2carryon](https://github.com)
 - **本文链接：** [https://github.com/ihave2carryon/p/18578873](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
