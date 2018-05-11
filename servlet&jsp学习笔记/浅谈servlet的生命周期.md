servlet的生命周期是servlet相关知识中很重要的一部分。
***
servlet从被加载到销毁经历了多个阶段，其中需要我们十分了解每个阶段的意义作用，才能更好地编写相关的servlet程序。

# servlet的生命周期详解

下图很好的说明了servlet的各个阶段
***
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-33b299fc37ef3b44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
***
* 首先，容器加载servlet类，从class字节码加载类
* 随后初始化servlet，使之成为一个对象！servlet的无参构造函数运行，这里不需要我们自己写构造函数，只需要使用编译器的提供的默认构造函数即可（** 相当于new操作 ，成为一个对象**），值得注意的是，**此处的只是一个普通的对象，还不具备成为一个完整servlet的一些信息和功能，所以我们要进行下一步，也就是init（）方法。**
* 调用init（）方法，此方法只在servlet的一生中调用一次，而且必须在容器调用service（）之前完成。这一步主要是让上一步对象加上一些东西，使之不再是一个普通的对象，而是一个特殊的servlet对象。
* 调用service（）方法，servlet的一生主要都在这里度过，处理用户请求，每个请求在一个单独的线程里运行。
* 调用destroy（）方法，容器调用这个方法，从而在servlet被杀死之前有机会清理资源。与init一样，destroy也只能调用一次。

# servlet生命周期中三大重要的时刻

1.  init（）
何时调用：servlet实例创建后，并在servlet能为客户请求提供service服务前，容器要对servlet调用init。
作用： 使你在servlet处理客户请求之前有机会对其进行初始化
是否覆盖： 有可能。如果由初始化代码（如得到一个数据库连接），就要调用init（）方法

2. service（）
何时调用：
第一个客户请求到来时，容器会开始一个新线程，或者从线程池分配一个线程，并调用servlet的service（）方法。
作用：
这个方法会查看请求，确定http方法
是否覆盖：
不太可能

3. doGet或者doPost
何时调用：
service方法根据请求的http方法调用doGet或者doPost。
作用：
要在这里写代码，你的web需要实现的业务逻辑等
是否覆盖：
一定要覆盖其中之一。

** 每个请求在一个单独的线程里运行。容器不关心是谁的请求，每个到来的请求意味着一个新的线程。
