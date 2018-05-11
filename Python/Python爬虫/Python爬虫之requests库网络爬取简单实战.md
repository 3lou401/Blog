> * 实例1：直接爬取网页
> * 实例2 ： 构造headers，突破访问限制，模拟浏览器爬取网页
> * 实例3 ： 分析请求参数，构造请求参数爬取所需网页
> * 实例4： 爬取图片
> * 实例5： 分析请求参数，构造请求参数爬取所需信息

# 实例1：京东商品页面的爬取

![image.png](http://upload-images.jianshu.io/upload_images/1234352-097f96cfa22bd654.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们利用requests库爬取这个页面的商品信息

首先引入requests库
```python
 import requests
```
然后爬取页面
```python
r =requests.get("https://item.jd.com/4645290.html")
```
然后我们测试状态码,编码和内容
```python
r.status_code
r.encoding
r.text[:1000]
```

![image.png](http://upload-images.jianshu.io/upload_images/1234352-64604e70ca124550.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到此，说明我们已经成功利用requests库获取到了商品页面的信息。

完整的爬取代码
```python
import requests
url = "https://item.jd.com/4645290.html"
try:
    r = requests.get(url)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text[:1000])
except:
    print("爬取失败")
```

# 实例2 ： 亚马逊商品页面爬取

我们选取如下界面进行爬取

![image.png](http://upload-images.jianshu.io/upload_images/1234352-68084ec3d09a382c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，我们按照之前的步骤进行爬取
引入requests库，然后get，判断status_code
```python
r = requests.get("https://www.amazon.cn/dp/B0011F7WU4/ref=s9_acss_bw_cg_JAVA_1a1_w?m=A1AJ19PSB66TGU&pf_rd_m=A1U5RCOVU0NYF2&pf_rd_s=merchandised-search-6&pf_rd_r=D9MK8AMFACZGHMFJGRXP&pf_rd_t=101&pf_rd_p=f411a6d2-b2c5-4105-abd9-69dbe8c05f1c&pf_rd_i=1899860071")
r.status_code
```
显示503，说明服务器错误，
503   （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。

我们查看编码发现
```python
r.encoding
'ISO-8859-1'
```
我们需要转换编码
```python
r.encoding = r.apparent_encoding
```
然后显示爬取内容，发现

![image.png](http://upload-images.jianshu.io/upload_images/1234352-e93de6a62007898e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发现出现了错误。

网页告诉我们出现了错误，但只要我们正确获取到了网页的内容，就说明网路方面肯定是没有错误的。这说明亚马逊对爬虫有限制，一般对爬虫限制的话，就是robots协议，其中还可以对访问对象进行限制，限制只能通过相应的浏览器访问，而限制爬虫的访问。

我们通过request.header查看我们发给亚马逊的请求头部到底是什么内容

![image.png](http://upload-images.jianshu.io/upload_images/1234352-509dfedafa67880f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到信息中的user-agent的信息是python。这说明我们的程序诚实的告诉亚马逊，这个程序是python的requests库发起的请求。
亚马逊的服务器看到这是个爬虫请求，所以就返回错误的信息。

那么我们如何才能访问呢？
我们都知道requests库可以更改请求的头部信息，我们可以模拟一个浏览器的请求

我们构造一个键值对
```python
kv = {'user-agent':'Mozilla/5.0'}
url = "https://www.amazon.cn/dp/B0011F7WU4/ref=s9_acss_bw_cg_JAVA_1a1_w?m=A1AJ19PSB66TGU&pf_rd_m=A1U5RCOVU0NYF2&pf_rd_s=merchandised-search-6&pf_rd_r=D9MK8AMFACZGHMFJGRXP&pf_rd_t=101&pf_rd_p=f411a6d2-b2c5-4105-abd9-69dbe8c05f1c&pf_rd_i=1899860071"
r = requests.get(url, headers = kv)
```
我们查看状态码，发现为200，说明这一次成功获取到了页面的内容

![image.png](http://upload-images.jianshu.io/upload_images/1234352-4efd84851391d510.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


完整的爬取代码
```python
import requests
url = "https://www.amazon.cn/dp/B0011F7WU4/ref=s9_acss_bw_cg_JAVA_1a1_w?m=A1AJ19PSB66TGU&pf_rd_m=A1U5RCOVU0NYF2&pf_rd_s=merchandised-search-6&pf_rd_r=D9MK8AMFACZGHMFJGRXP&pf_rd_t=101&pf_rd_p=f411a6d2-b2c5-4105-abd9-69dbe8c05f1c&pf_rd_i=1899860071"
try:
    kv = {'user-agent':'Mozilla/5.0'}
    r = requests.get(url, headers = kv)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text[1000:2000])
except:
    print("爬取失败")
```

# 实例3： 百度/360搜索关键词提交爬虫
搜索关键词提交的接口：
```
https://www.baidu.com/s?ie=UTF-8&wd=keyword
```

通过requests的params参数，构造查询参数

完整的代码
```python
import requests

keyword = "刘德华"
url = "http://www.baidu.com/s?ie=UTF-8"

try:
    kv = {"wd":keyword}
    r = requests.get(url, params = kv)
    print(r.request.url)
    r.raise_for_status()
    print(len(r.text))
    print(r.text)
except:
    print("爬取失败")
```

# 实例4 网络图片的爬取和存储

网络中图片连接的格式
```
http://www.example.com/picture.jpg
```

假设我们现在要爬取
```
http://www.nationalgeographic.com.cn/
```
图片连接:
```
http://image.nationalgeographic.com.cn/2015/0121/20150121033625957.jpg
```

完整的爬取代码：
```python
import requests
import os

url = "http://image.nationalgeographic.com.cn/2015/0121/20150121033625957.jpg"
root = "D://pics//"
path = root + url.split('/')[-1]

try:
    if not os.path.exists(root):
        os.mkdir(root)
    if not os.path.exists(path):
        r = requests.get(url)
        with open(path,'wb') as f:
            f.write(r.content)
            f.close()
            print("文件保存成功")
    else :
        print("文件已存在")
except:
    print("爬取失败")
```

# 实例5 IP地址归属地查询

此网站可以查询IP地址归属地
http://m.ip138.com/ip.asp

我们分析它请求的过程，发现它的请求接口就是在地址后附加参数，类似于百度搜索
```
http://m.ip138.com/ip.asp?ip=125.220.159.160
```
所以我们可以构造查询参数，发送给服务器，然后获取返回的结果

完整代码
```python
import requests
url = "http://m.ip138.com/ip.asp?"
ip = "125.220.159.160"
kv = {"ip":ip}

try:
    r = requests.get(url, params = kv)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    print(r.text)
except:
    print("爬取失败")
```

