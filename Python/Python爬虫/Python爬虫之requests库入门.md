网络爬虫就是提取网页的信息。
网络爬虫的原则就是谨记“the website is API”,就是我们所面对的对象和信息来源都是各个website。现在python由于其特性已经越来越被广泛的用于网络爬虫领域。

我们先从最简单的python爬虫库requests库开始讲起。

首先我们从官网下载并安装好requests库。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b93f8a3dc24edf28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# requests库的get方法

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ae38e16f7c8437ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们调用requests的get方法就是构造一个向服务器请求资源的requests对象，这个对象会返回一个包含服务器资源的response对象，随后我们就可以从response对象中获取我们需要的信息。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-06932a0d7347f377.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5dfaa9003f70a4ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
>>> import requests
>>> res = requests.get("http://www.baidu.com")
>>> res.status_code
200
>>> type(res)
<class 'requests.models.Response'>
>>> res.headers
{'Server': 'bfe/1.0.8.18', 'Date': 'Sat, 15 Apr 2017 03:20:29 GMT', 'Content-Type': 'text/html', 'Last-Modified': 'Mon, 23 Jan 2017 13:28:16 GMT', 'Transfer-Encoding': 'chunked', 'Connection': 'Keep-Alive', 'Cache-Control': 'private, no-cache, no-store, proxy-revalidate, no-transform', 'Pragma': 'no-cache', 'Set-Cookie': 'BDORZ=27315; max-age=86400; domain=.baidu.com; path=/', 'Content-Encoding': 'gzip'}
```

我们可以看到response对象包含返回的信息，同时也包括请求时的头部信息

我们接下来了解一下response对象的属性


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-4054b6672c97609f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-8bdee275130cf666.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-714351275048062f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6d501732400ee7b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* r.encoding:如果headers信息不存在也就是没有指明编码信息，则默认编码为：ISO-8859-1，而且r.text会根据r.encoding的值来显示内容，所以我们有时候如果出现乱码，那么可能就是因为headers未指明charset
* r.apparent_encoding:是根据网页分析出的实际编码方式

# 理解requests库的异常
网页爬虫的时候，一个很重要的问题就是异常处理，因为网络连接有时候是不稳定的，所以我们需要处理这些情况。

首先了解requests库的异常

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-df2c7fefecedae24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-eaa487ecdf4e7f2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以这个方法很适合我们用于异常处理，他会在内部帮我们判断，状态码是否等于200，如果不等于就抛出httperror

# 爬去网页通用的代码框架

```
# -*- coding:utf-8 -*-

import requests

def getHTMLText(url):
    try:
        r = requests.get(url,timeout = 30)
        r.raise_for_status() 
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return "产生异常"

if __name__ == "__main__":
    url = "http://www.baidu.com"
    print(getHTMLText(url))
```

# resquests库主要方法的解析
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f0fae71bfa32f186.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从这个表中我们可以看出实际上只有一个request方法，其他六个方法都是以传参数的方式在调用request方法

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-88257c451c709f91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们来分析了解一下request方法的可选参数：

* params: 字典或字节序列，作为参数增加到url中
```
>>> kv = {'ie':'UTF-8','wd':'刘德华'}
>>> r = requests.get("http://www.baidu.com/s",params = kv)
Traceback (most recent call last):
  File "<pyshell#1>", line 1, in <module>
    r = requests.get("http://www.baidu.com/s",params = kv)
NameError: name 'requests' is not defined
>>> import requests
>>> r = requests.get("http://www.baidu.com/s",params = kv)
>>> r.url
'http://www.baidu.com/s?ie=UTF-8&wd=%E5%88%98%E5%BE%B7%E5%8D%8E'
>>> print(r.text[:200])
```

* data : 字典、字节序列或文件对象，作为Request的内容
```
>>> kv = {'key1': 'value1', 'key2': 'value2'}
>>> r = requests.request('POST', 'http://python123.io/ws', data=kv)
>>> body = '主体内容'
>>> r = requests.request('POST', 'http://python123.io/ws', data=body)
```

* json : JSON格式的数据，作为Request的内容
```
>>> kv = {'key1': 'value1'}
>>> r = requests.request('POST', 'http://python123.io/ws', json=kv)
```

* headers : 字典，HTTP定制头
可以用来模拟浏览器登录
```
>>> hd = {'user‐agent': 'Chrome/10'}
>>> r = requests.request('POST', 'http://python123.io/ws', headers=hd)
```
* files : 字典类型，传输文件
```
>>> fs = {'file': open('data.xls', 'rb')}
>>> r = requests.request('POST', 'http://python123.io/ws', files=fs)
```

* timeout : 设定超时时间，秒为单位
```
>>> r = requests.request('GET', 'http://www.baidu.com', timeout=10)
```

* proxies : 字典类型，设定访问代理服务器，可以增加登录认证
```
>>> pxs = { 'http': 'http://user:pass@10.10.10.1:1234'
'https': 'https://10.10.10.1:4321' }
>>> r = requests.request('GET', 'http://www.baidu.com', proxies=pxs)
```
