本文所实现的爬取淘宝商品信息将实现以下功能：
对于某个类别的淘宝商品的页面

![image.png](http://upload-images.jianshu.io/upload_images/1234352-dffd38569e8b7df5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

爬取这个商品名称，比如“手机”搜索结果下的每个商品的信息，存储到数据结构中，并能将其输出显示。

如下的输出形式：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6a14f1c6edf53eb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


接下来，就看我们如何一步步实现这个小爬虫的吧

# 分析
- 目标：获取淘宝搜索页面的信息，提取其中的商品名称和价格
- 理解：
淘宝的搜索接口
翻页的处理

首先分析搜索接口，
很容易我们就可以发现，

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a84e471800ccd679.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

https://s.taobao.com/search?q=后面接我们的搜索词就可以

我们再研究翻页处理：
第二页

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ff1d89426aefc59a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第三页

![image.png](http://upload-images.jianshu.io/upload_images/1234352-365f92e01c5eebf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以发现，s参数指定了搜索页的跳转，每48条记录一次页面跳转。

有同学可能发现还会有其他很多的参数，这个我们先忽略，我们直接构造一个url，只包括搜索词和搜索页，看看能不能访问到正确页面：
比如
https://s.taobao.com/search?q=手机&s=48

我们将上述地址放到地址栏

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6ebe2858e3bbe54d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发现可以正确访问，所以我们现在就已经知道了如何确定访问接口

# 设计程序结构

主要有三步
- 步骤1：提交商品搜索请求，循环获取页面
- 步骤2：对于每个页面，提取商品名称和价格信息
- 步骤3：将信息输出到屏幕上

首先我们对于上面研究出的搜索接口给出基本的代码：
```python
#CrowTaobaoPrice.py
import requests
import re

def getHTMLText(url):

    
def parsePage(ilt, html):


def printGoodsList(ilt):

        
def main():
    goods = '手机'
    depth = 3
    start_url = 'https://s.taobao.com/search?q=' + goods
    infoList = []
    for i in range(depth):
        try:
            url = start_url + '&s=' + str(48*i)
            html = getHTMLText(url)
            parsePage(infoList, html)
        except:
            continue
    printGoodsList(infoList)
    
main()
```

对于获取页面源码的函数，我们已经写过很多次了，就是利用requets库抓取页面

```python
def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ""
```

# 核心代码
这个爬虫最核心的地方就在于对商品信息的获取，我们首先分析页面的源代码，我们搜索第一个商品的名字


![image.png](http://upload-images.jianshu.io/upload_images/1234352-94b39b052217a474.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基本上所有商品的信息，名称，价格，月销量都在这段数据结构里显示，是嵌入在js代码里的，所以我们就无法用bs4库来提取。
但我们可以直接用re库，正则表达式提取。
因为我们可以发现，所有的商品名称都是
“title”:"   "的格式，我们可以搜索确认一下：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-912719b9eeeb6a16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们搜索发现，正好是48条记录，也就是48个商品的信息，所以直接匹配就可以把所所有商品名称信息提取出来，是不是很简单。
对于商品价格 和月销量也是这么获取的

```python
def parsePage(ilt, html):
    try:
        plt = re.findall(r'\"price\"\:\"[\d\.]*\"',html)
        tlt = re.findall(r'\"title\"\:\".*?\"',html)
        mslt = re.findall(r'\"month_sales\"\:\"[\d\.]*\"',html)
        for i in range(len(plt)):
            price = eval(plt[i].split(':')[1])
            title = eval(tlt[i].split(':')[1])
            monthsales = eval(mslt[i].split(':')[1])
            ilt.append([price , title, monthsales])
    except:
        print("")
```

# 完整代码
```python
#CrowTaobaoPrice.py
import requests
import re

def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ""
    
def parsePage(ilt, html):
    try:
        plt = re.findall(r'\"price\"\:\"[\d\.]*\"',html)
        tlt = re.findall(r'\"title\"\:\".*?\"',html)
        mslt = re.findall(r'\"month_sales\"\:\"[\d\.]*\"',html)
        for i in range(len(plt)):
            price = eval(plt[i].split(':')[1])
            title = eval(tlt[i].split(':')[1])
            monthsales = eval(mslt[i].split(':')[1])
            ilt.append([price , title, monthsales])
    except:
        print("")

def printGoodsList(ilt):
    tplt = "{:4}\t{:8}\t{:16}\t{:16}"
    print(tplt.format("序号", "价格", "商品名称","月销量"))
    count = 0
    for g in ilt:
        count = count + 1
        print(tplt.format(count, g[0], g[1],g[2]))
        
def main():
    goods = '手机'
    depth = 3
    start_url = 'https://s.taobao.com/search?q=' + goods
    infoList = []
    for i in range(depth):
        try:
            url = start_url + '&s=' + str(48*i)
            html = getHTMLText(url)
            parsePage(infoList, html)
        except:
            continue
    printGoodsList(infoList)
    
main()

```

结果：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6edb30e925a73959.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


小伙伴也可以更改这个程序去搜索各种不同的商品的信息啦
