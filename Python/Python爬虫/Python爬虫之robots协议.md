网络爬虫有时候也会引发很多的问题
* 由于编写的爬虫的性能和其他原因，可能会对Web服务器带来巨大的资源开销
* 服务器上的数据有产权归属网络爬虫获取数据后牟利将带来法律风险
* 网络爬虫可能具备突破简单访问控制的能力，获得被保护数据从而泄露个人隐私

所以，一般部分网站会给出限制网路爬虫的协议，这就是robots协议。

* 来源审查：判断User‐Agent进行限制
检查来访HTTP协议头的User‐Agent域，只响应浏览器或友好爬虫的访问
* 发布公告：Robots协议
告知所有爬虫网站的爬取策略，要求爬虫遵守

> robots协议的全名为Robots Exclusion Standard，网络爬虫排除标准
作用：
网站告知网络爬虫哪些页面可以抓取，哪些不行
形式：
在网站根目录下的robots.txt文件

# 案例
* 京东的robots协议
https://www.jd.com/robots.txt

![image.png](http://upload-images.jianshu.io/upload_images/1234352-b7cb16b0a8aff72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-025b448f6a3641f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

意思就是
对于所有的user-agent：
不可以访问一下url
Disallow: /?* 
Disallow: /pop/*.html 
Disallow: /pinpai/*.html?*
对于其他几个user-agent是禁止爬虫的，我们看一下就是一淘这些淘宝的搜索引擎，也难怪京东和淘宝哈哈哈

实际中如何遵守robots协议
* 网络爬虫：
自动或人工识别robots.txt，再进行内容爬取
* 约束性：
Robots协议是建议但非约束性，网络爬虫可以不遵守，但存在法律风险


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3422f9b346ce8183.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
