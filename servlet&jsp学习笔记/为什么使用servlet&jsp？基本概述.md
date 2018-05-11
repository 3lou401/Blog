
![
![webtest.PNG](http://upload-images.jianshu.io/upload_images/1234352-f8a036be574ff7d3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
](http://upload-images.jianshu.io/upload_images/1234352-3f91bd6deac44d6f.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
](http://upload-images.jianshu.io/upload_images/1234352-e011f7229bf7e78f.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# web client做些什么
向服务器请求某项资源，并得到服务器的返回结果

![what client do.PNG](http://upload-images.jianshu.io/upload_images/1234352-67ca56a11e0eb7a2.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# HTTP && HTML
client和server都知道HTTP和HTML。
HTML告诉浏览器怎样向用户显示内容
http是web上客户与服务器之间进行通信的网络协议

## http分为http request 和http response

* http request

![key elements of request stream .PNG](http://upload-images.jianshu.io/upload_images/1234352-957f652ba1762abb.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
request的关键元素：
1.  http方法
2.  需要访问资源的URL
3. 需要传递的参数

** http response **


![key elements of response stream.PNG](http://upload-images.jianshu.io/upload_images/1234352-3cc6358675684458.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
response的关键元素：
1.状态码
2.内容类型
3.返回的内容

## HTML是http相应的一部分
http响应分为http首部和http体。响应的HTML在http体中，属于http响应的一部分。

# request中的get和post方法
具体get和post方法的区别将在以后详细讲到
## get方法


![anatomy of Get.PNG](http://upload-images.jianshu.io/upload_images/1234352-b55e7fe4f3d404d2.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## post方法


![anatomy of post.PNG](http://upload-images.jianshu.io/upload_images/1234352-26b6d4875f23f749.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# MIME类型
MIME类型告诉浏览器要接收的数据是什么类型，以便于浏览器显示数据。

对于上述内容，我们可以用一张图进行总结：


![summary of simple http .PNG](http://upload-images.jianshu.io/upload_images/1234352-67632c97f36409ea.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 下面将实现一个简单的servlet程序
在MyEclipse平台下，直接新建一个web project，用其默认的内容，直接run on server。
尝试多次发现结果显示404 ，寻找web.xml

![webxml.PNG](http://upload-images.jianshu.io/upload_images/1234352-195eefdab64aa11a.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从配置文件的url可知，访问servlet的路径应该是/Ch1Servlet
但却显示404，后来发现细节上出现了错误。


![webtest.PNG](http://upload-images.jianshu.io/upload_images/1234352-6cc7b01308a6c0ab.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**需要在前面加上web应用名才可运行。**

# 总结 

为什么要使用servlet&jsp？

** 服务器擅长提供静态的界面。举个例子，如果我们需要在html中加上一段代码，让其动态的显示当前的时间，那么静态界面显然是无法满足要求的，这时候我们就需要一个辅助应用帮忙处理显示动态的时间，然后将处理后的结果插入到HTML中，再交给服务器返回，对服务器来说，它处理的仍然是自己所以为的静态界面。servlet就是服务器端的这种辅助应用，java小程序动态的处理各种结果。 ** 
