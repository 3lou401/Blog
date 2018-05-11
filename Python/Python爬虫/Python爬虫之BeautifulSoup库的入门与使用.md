上篇文章中，[Python爬虫之requests库网络爬取简单实战](http://www.jianshu.com/p/170c52af02a1)
我们学习了如何利用requets库快速获取页面的源代码信息。我们在具体的爬虫实践的时候，第一步就是获取到页面的源代码，但是仅仅是获取源代码是不够的，我们还需要从页面的源代码中提取出我们所需要的那一部分的信息。所以，爬虫的难点就在于对源代码的信息的提取与处理。
[Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) 是一个可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.Beautiful Soup会帮你节省数小时甚至数天的工作时间.

具体的BeautifulSoup的安装与介绍比较简单，我们可以参考https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/#id17

# Beautiful Soup库的理解


![](http://upload-images.jianshu.io/upload_images/1234352-767bc690cf6cee59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

简单的说，BeautifulSoup库可以将一个html文档转换成一个BeautifulSoup类，然后我们就可以使用BeautifulSoup的各种方法提取出我们所需要的元素

> Beautiful Soup库是解析、遍历、维护“标签树”的功能库

要理解与使用BeautifulSoup库我们就需要对html文档有了解


![image.png](http://upload-images.jianshu.io/upload_images/1234352-68ab5efb3e775cb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# Beautiful Soup库的引用

Beautiful Soup库，也叫beautifulsoup4 或 bs4
约定引用方式如下，即主要是用BeautifulSoup类
```
from bs4 import BeautifulSoup
import bs4
```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3fa9821b80da29f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

BeautifulSoup对应一个HTML/XML文档的全部内容

Beautiful Soup库解析器

```
soup = BeautifulSoup('<html>data</html>'，'html.parser')
```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-504088d412b8bace.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# BeautifulSoup类的基本元素

![image.png](http://upload-images.jianshu.io/upload_images/1234352-dc6479abd1615280.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# BeautifulSoup解析实例

我们先用requests库获取一个简单的页面
http://python123.io/ws/demo.html


![image.png](http://upload-images.jianshu.io/upload_images/1234352-4aeb9c2bb2e340fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```python
import requests

r = requests.get("http://python123.io/ws/demo.html")

demo = r.text

print(demo)
```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-bfcf1b8c1c4c5be2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup(demo, 'html.parser')
>>> soup.prettify()
```
我们可以利用BeautifulSoup库对页面进行解析和提取

## Tag 标签


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8bc634ce04567124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```python
>>> soup.title
<title>This is a python demo page</title>
>>> tag = soup.a
>>> tag
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
```

> 任何存在于HTML语法中的标签都可以用soup.<tag>访问获得
当HTML文档中存在多个相同<tag>对应内容时，soup.<tag>返回第一个

## Tag的name（名字）


![image.png](http://upload-images.jianshu.io/upload_images/1234352-425b35a0be4d932f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
>>> soup.a.name
'a'
>>> soup.a.parent.name
'p'
>>> 
```

每个<tag>都有自己的名字，通过<tag>.name获取，字符串类型

## Tag的attrs（属性）


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8f7c8c63f649117a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Tag的NavigableString


![image.png](http://upload-images.jianshu.io/upload_images/1234352-2c7baf82af992d29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-489a530b1cabed75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Tag的Comment

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ebcb18b666765060.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-cf7b16cc68bb8efd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 基于bs4库的HTML内容遍历方法


![image.png](http://upload-images.jianshu.io/upload_images/1234352-0158491b3a85ac1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![image.png](http://upload-images.jianshu.io/upload_images/1234352-650cb6b3de60f01b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 标签树的下行遍历


![image.png](http://upload-images.jianshu.io/upload_images/1234352-c8485b9260a3845c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

BeautifulSoup类型是标签树的根节点

标签树的下行遍历
![image.png](http://upload-images.jianshu.io/upload_images/1234352-bb33f1428b6adb28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-fed9aae64b09402f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 标签树的上行遍历


![image.png](http://upload-images.jianshu.io/upload_images/1234352-07c2919f6be6e2eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-74f191391970822c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-a050699599e715e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 标签树的平行遍历


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d423fa3057a10e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-50f040e29fe8c15e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-02a97d209e0eaa72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8881b8f4883d3312.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-9d32c67f5eeb7b43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 小结

![image.png](http://upload-images.jianshu.io/upload_images/1234352-63126bf22e33ba3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
