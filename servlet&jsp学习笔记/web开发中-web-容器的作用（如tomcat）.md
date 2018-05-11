我们讲到servlet可以理解服务器端处理数据的java小程序，那么谁来负责管理servlet呢？这时候我们就要用到web容器。它帮助我们管理着servlet等，使我们只需要将重心专注于业务逻辑。

# 什么是web容器？

servlet没有main方法，那我们如何启动一个servlet，如何结束一个servlet，如何寻找一个servlet等等，都受控于另一个java应用，这个应用我们就称之为web容器。

我们最常见的tomcat就是这样一个容器。如果web服务器应用得到一个指向某个servlet的请求，此时服务器不是把servlet交给servlet本身，而是交给部署该servlet的容器。要有容器向servlet提供http请求和响应，而且要由容器调用servlet的方法，如doPost或者doGet。

# web容器的作用
servlet需要由web容器来管理，那么采取这种机制有什么好处呢？

* **通信支持**
利用容器提供的方法，你可以简单的实现servlet与web服务器的对话。否则你就要自己建立server搜创可贴，监听端口，创建新的流等等一系列复杂的操作。而容器的存在就帮我们封装这一系列复杂的操作。使我们能够专注于servlet中的业务逻辑的实现。

* **生命周期管理**
容器负责servlet的整个生命周期。如何加载类，实例化和初始化servlet，调用servlet方法，并使servlet实例能够被垃圾回收。有了容器，我们就不用花精力去考虑这些资源管理垃圾回收之类的事情。

* **多线程支持**
容器会自动为接收的每个servlet请求创建一个新的java线程，servlet运行完之后，容器会自动结束这个线程。

* **声明式实现安全**
利用容器，可以使用xml部署描述文件来配置安全性，而不必将其硬编码到servlet中。

* **jsp支持**
容器将jsp翻译成java！

# 容器如何处理请求


![How the Container handles a request01.PNG](http://upload-images.jianshu.io/upload_images/1234352-056ee4a6e5fb1f54.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

client点击一个URL，其URL指向一个servlet而不是静态界面。


![How the Container handles a request02.PNG](http://upload-images.jianshu.io/upload_images/1234352-1728c5eec180de6b.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

容器识别出这个请求索要的是一个servlet，所以创建两个对象：
* httpservletrequest
* httpservletresponse


![03.PNG](http://upload-images.jianshu.io/upload_images/1234352-7e183ac063f86193.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

容器根据请求中的URL找到对应的servlet，为这个请求创建或分配一个线程，并把两个对象request和response传递到servlet线程中。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-885908623cef4b0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

容器调用servlet的service（）方法。根据请求的不同类型，service（）方法会调用doGet（）或者doPost（）方法。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7fd4a0dba797fca1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

doGet（）方法生成动态页面，然后把这个页面填入到response对象中，此时，容器仍然拥有response对象的引用。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-2e666cfaed47c7c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

线程结束。容器把response对象转换成http响应，传回client，并销毁response和request对象。

# URL与servlet映射模式

```
<servlet>
    <servlet-name>Ch1Servlet</servlet-name>
    <servlet-class>ch1Servlet.Ch1Servlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>Ch1Servlet</servlet-name>
    <url-pattern>/Ch1Servlet</url-pattern>
  </servlet-mapping>
```
servlet有三个名字：
* 客户知道的URL名 `<url-pattern>/Ch1Servlet</url-pattern>`
* 部署人员知道的秘密的内部名 `    <servlet-name>Ch1Servlet</servlet-name>`
* 实际文件名 ` <servlet-class>ch1Servlet.Ch1Servlet</servlet-class>`
