
# 信息标记

- 标记后的信息可形成信息组织结构，增加了信息维度
- 标记的结构与信息一样具有重要价值
- 标记后的信息可用于通信、存储或展示
- 标记后的信息更利于程序理解和运用


![image.png](http://upload-images.jianshu.io/upload_images/1234352-2caf7a690216180a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

HTML通过预定义的<>…</>标签形式组织不同类型的信息

# 信息标记的种类

- XML
- JSON
- YAML

## XML
![image.png](http://upload-images.jianshu.io/upload_images/1234352-398bd28c9531c028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-b85707c767a96f82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d772ebd267cc34ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##JSON

![image.png](http://upload-images.jianshu.io/upload_images/1234352-e4d46a729cf7cd4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-1cc2c9eb8cf6eef1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d00089b4f86f6779.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-7d75ae276f46d11f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-5199d10201dd5a7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## YAML


![image.png](http://upload-images.jianshu.io/upload_images/1234352-815cb467655ec6c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-12871bc1f4cb2fc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-bd0419ef8151a240.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-ed1a07b7bc6c295c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-e06ab304e5640d9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-c0170415b9a267a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三种标记类型的比较


- XML 最早的通用信息标记语言，可扩展性好，但繁
- JSON 信息有类型，适合程序处理(js)，较XML简洁
- YAML 信息无类型，文本信息比例最高，可读性好


- XML Internet上的信息交互与传递
- JSON 移动应用云端和节点的信息通信，无注释
- YAML 各类系统的配置文件，有注释易读

# 信息提取

从标记后的信息中提取所关注的内容

- 方法一：完整解析信息的标记形式，再提取关键信息
XML JSON YAML
需要标记解析器，例如：bs4库的标签树遍历
优点：信息解析准确
缺点：提取过程繁琐，速度慢

- 方法二：无视标记形式，直接搜索关键信息
搜索
对信息的文本查找函数即可
优点：提取过程简洁，速度较快
缺点：提取结果准确性与信息内容相关

- 融合方法：结合形式解析与搜索方法，提取关键信息
XML JSON YAML 搜索
需要标记解析器及文本查找函数

## 实例
提取HTML中所有URL链接

思路：
- 1) 搜索到所有<a>标签
- 2) 解析<a>标签格式，提取href后的链接内容


![image.png](http://upload-images.jianshu.io/upload_images/1234352-90513978fdc8e165.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 基于bs4的html信息提取的实例


![image.png](http://upload-images.jianshu.io/upload_images/1234352-074262c2d1b78367.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> <>.find_all(name, attrs, recursive, string, **kwargs)
∙ name : 对标签名称的检索字符串
返回一个列表类型，存储查找的结果


![image.png](http://upload-images.jianshu.io/upload_images/1234352-94335fa6ebbc10ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-f0437d65dd6460f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> <>.find_all(name, attrs, recursive, string, **kwargs)
∙ name : 对标签名称的检索字符串
∙ attrs: 对标签属性值的检索字符串，可标注属性检索


![image.png](http://upload-images.jianshu.io/upload_images/1234352-f01d7d62cbc54644.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
>>> soup.find_all('p', 'course')
[<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:

<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>]
>>> soup.find_all('p','course')
[<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:

<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>]
>>> soup.find_all('p','title')
[<p class="title"><b>The demo python introduces several python courses.</b></p>]
>>> soup.find_all(id = 'link1')
[<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>]
>>> 
```

> <>.find_all(name, attrs, recursive, string, **kwargs)
∙ name : 对标签名称的检索字符串
∙ attrs: 对标签属性值的检索字符串，可标注属性检索

```python
>>> soup.find_all(id = re.compile('link'))
[<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]
```

> <>.find_all(name, attrs, recursive, string, **kwargs)
∙ name : 对标签名称的检索字符串
∙ attrs: 对标签属性值的检索字符串，可标注属性检索
∙ recursive: 是否对子孙全部检索，默认True

```python
>>> soup.find_all('a')
[<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]
>>> soup.find_all('a',recursive=False)
[]
```

> <>.find_all(name, attrs, recursive, string, **kwargs)
∙ name : 对标签名称的检索字符串
∙ attrs: 对标签属性值的检索字符串，可标注属性检索
∙ recursive: 是否对子孙全部检索，默认True
∙ string: <>…</>中字符串区域的检索字符串

```python
>>> soup
<html><head><title>This is a python demo page</title></head>
<body>
<p class="title"><b>The demo python introduces several python courses.</b></p>
<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:

<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
</body></html>
>>> soup.find_all(string='Basic Python')
['Basic Python']
>>> import re
>>> soup.find_all(string=re.compile('python'))
['This is a python demo page', 'The demo python introduces several python courses.']
```

<tag>(..) 等价于 <tag>.find_all(..)
soup(..) 等价于 soup.find_all(..)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-e7736be3c52033d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 小结

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d5fb7dff09c6e298.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
