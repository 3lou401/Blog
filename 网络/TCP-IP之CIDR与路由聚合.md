
# CIDR
(CIDR: Classless InterDomain Routing)无类域间路由
* 消除传统的 A 类、 B 类和 C 类地址界限
* NetID+SubID→Network Prefix (Prefix)可以任意长度
* 融合子网地址与子网掩码，方便子网划分
* 无类地址格式： a.b.c.d/x，其中x为前缀长度

例如

![image.png](http://upload-images.jianshu.io/upload_images/1234352-9a7a12ef8608b389.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 子网201.2.3.64， 255.255.255.192→201.2.3.64/26

无类域间路由(CIDR: Classless InterDomain Routing)
* 提高IPv4 地址空间分配效率
* 提高路由效率
* 将多个子网聚合为一个较大的子网
* 构造超网（ supernetting）

# 路由聚合（ route aggregation）
就是将可以归纳到相同的子网的聚合到一起，这样可以减轻路由表的负担，加快路由效率，如下面这个例子：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-7c4f60ef63f9f70b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-9e0f43fec24e9068.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

层级编址使得路由信息通告更高效:

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a49bad920b34397b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选用更具体的路由： 最长前缀匹配优先！

![image.png](http://upload-images.jianshu.io/upload_images/1234352-309f87aa7a4516e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
