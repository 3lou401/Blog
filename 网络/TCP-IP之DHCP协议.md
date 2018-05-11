首先，我们考虑一个主机如何获取IP地址？

有两种获取方法，一种是静态配置，就是从网络管理员获取一个给定的IP地址，也叫硬编码，还有一种就是动态配置IP地址，这就是我们即将要讲的DHCP协议，动态主机配置协议。

# 静态配置
硬编码。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-5db277759a91bfdf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

硬编码，就是像windows系统中选择给定的IP地址，我们需要指定IP地址，子网掩码和默认网关。默认网关是什么呢？默认网关就是属于这个子网的所有主机，如果将来要离开这个子网，就会被统一转发给这个默认网关指定的IP，我们可以认为它是一个转发路由。如果有多个默认网关，则可以选择其中一个就可以了。


![image.png](http://upload-images.jianshu.io/upload_images/1234352-4cd7a163058d2916.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# DHCP协议
* 从服务器动态获取：
• IP地址
• 子网掩码
• 默认网关地址
• DNS服务器名称与IP地址

* “ 即插即用”
* 允许地址重用
* 支持在用地址续租
* 支持移动用户加入网络

DHCP协议交换信息的基本步骤
* 主机广播 “ DHCP discover” (发现报文)
* DHCP服务器利用 “ DHCP offer” (提供报文) 进行响应
* 主机请求IP地址: “ DHCP request” (请求报文)
* DHCP服务器分配IP地址: “ DHCP ack” (确认报文)

DHCP工作过程示例：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-c8b5ccb8726e5cb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* DHCP discover
客户端源地址四个0表示本机，端口号68和服务器的67都是DHCP指定的。发送一个广播报文。在这个网络中的所有主机包括服务器都会收到这个发现报文，但是只有DHCP服务器才会对其进行响应。

* DHCP offer
我们看到服务器的提供报文里，也是通过广播地址发送的，这很容易理解，因为这个是时候请求主机还没有IP地址，所以需要进行广播，请求主机才能够收到，提供报文会将所分配的IP地址加在报文里，图中的223.1.2.4。那么主机如何知道这是它请求的服务器发来的提供报文呢，通过transaction ID来确认。

* DHCP request
这里有一个问题，就是为什么主机发送依然是采取广播的方式，实际上这里的作用是，因为整个网络中，不止一个DHCP服务器，所以采取广播的方式，同时也在告诉其他的dhcp服务器，我现在已经确定了我所需要请求的dhcp服务器了，你们别给我发消息了。

*  DHCP ack
服务器收到请求后，会发送确认报文，主机收到之后，就确认可以使用223.1.2.4作为它的IP地址了。

我们可以看到实际上dhcp之间的信息交换，可以分为两块，一块是主机线广播确认找到提供IP的dhcp服务器，然后再从确认的dhcp服务器收取IP地址。

DHCP协议在应用层实现
* 请求报文封装到UDP数据报中
* IP广播
* 链路层广播(e.g. 以太网广播)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-9dec3c5aa91361cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* DHCP服务器构造ACK报文
* 包括分配给客户的IP地址、子网掩码、默认网关、 DNS服务器地址


![image.png](http://upload-images.jianshu.io/upload_images/1234352-f68914d54f67e58a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
