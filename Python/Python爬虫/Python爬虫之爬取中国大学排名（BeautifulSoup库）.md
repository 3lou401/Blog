首先，我们确定需要爬取的网页
http://www.zuihaodaxue.cn/zuihaodaxuepaiming2016.html


![image.png](http://upload-images.jianshu.io/upload_images/1234352-b097b5ddf0334589.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们需要打开网页源代码，查看此网页的信息是写在html代码中，还是由js文件动态生成的，如果是后者，那么我们目前仅仅采用requests和BeautifulSoup还很难爬取到排名的信息。

查看网页源代码，我们可以发现，排名信息是写在html页面中的，这时候我们利用BeautifulSoup库就可以对信息进行提取


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8588582f9d8c5141.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

爬虫实现的目标：

输入：大学排名URL链接
输出：大学排名信息的屏幕输出（排名，大学名称，总分）

技术路线：requests‐bs4
定向爬虫：仅对输入URL进行爬取，不扩展爬取

# 分析
首先，我们要获取到这个网页的源码，我们可以利用requests库抓取到该网页的源码信息。然后利用bs4库将网页中大学排名的信息提取出来，输入到数据结构中，最后将数据结构中存储的数据输出

主要就是一下三步：
- 步骤1：从网络上获取大学排名网页内容
- 步骤2：提取网页内容中信息到合适的数据结构
- 步骤3：利用数据结构展示并输出结果

程序设计
- getHTMLText()
- fillUnivList()
- printUnivList()

首先我们先忽略代码的具体实现，写出爬取的逻辑：
```python
#CrawUnivRankingB.py
import requests
from bs4 import BeautifulSoup
import bs4

def getHTMLText(url):
   

def fillUnivList(ulist, html):
    

def printUnivList(ulist, num):
   
    
def main():
    uinfo = []
    url = 'http://www.zuihaodaxue.cn/zuihaodaxuepaiming2017.html'
    html = getHTMLText(url)
    fillUnivList(uinfo, html)
    printUnivList(uinfo, 20) # 20 univs
main()
```

然后我们来实现每个函数
首先第一个函数很好实现，就是requests库直接抓取网页
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
然后对于信息的提取，我们就需要对网页进行仔细的分析


![image.png](http://upload-images.jianshu.io/upload_images/1234352-be9e2fea4b35404f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将源码格式化后就是如下这样：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-7606be9092df6f31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现所有的排名信息都在一个tbody的标签里面，然后每个tr标签又存储了每个大学的信息，具体的信息存在每个td标签里。
所以，思路救出来了
第一步，提取出tbody标签，也就是页面中第一个tbodybiaoqian
第二步，提取出里面所有的tr标签
第三步，对每个tr标签里的td信息存储到相应的数据结构里

```python
#CrawUnivRankingB.py
import requests
from bs4 import BeautifulSoup
import bs4

def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ""

def fillUnivList(ulist, html):
    soup = BeautifulSoup(html, "html.parser")
    for tr in soup.find('tbody').children:
        if isinstance(tr, bs4.element.Tag):
            tds = tr('td')
            ulist.append([tds[0].string, tds[1].string, tds[3].string])

def printUnivList(ulist, num):
    tplt = "{0:^10}\t{1:{3}^10}\t{2:^10}"
    print(tplt.format("排名","学校名称","总分",chr(12288)))
    for i in range(num):
        u=ulist[i]
        print(tplt.format(u[0],u[1],u[2],chr(12288)))
    
def main():
    uinfo = []
    url = 'http://www.zuihaodaxue.cn/zuihaodaxuepaiming2016.html'
    html = getHTMLText(url)
    fillUnivList(uinfo, html)
    printUnivList(uinfo, 20) # 20 univs
main()


```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d51823105469914f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
