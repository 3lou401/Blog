IP分组首部中有两个极其重要的字段，就是源地址和目的地址
* 源地址(SA)-从哪儿来
* 目的地址(DA)-到哪儿去

接口(interface): 主机/路由器与物理链路的连接
* 实现网络层功能
* 路由器通常有多个接口
* 主机通常只有一个或两个接口 (e.g.，有线的以太网接口，无线的802.11接口)

IP地址: 32比特(IPv4)编号标识主机、路由器的接口
** IP地址与每个接口关联 **


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9f577397c6ca2286.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

怎样为接口分配IP地址呢？

我们不是直接给每个主机分配ip地址，而是依旧借鉴常用的划分的思想，首先分类，然后将在类里面具体细分，所以这就引出了子网的概念。

# IP子网（ Subnets）
* IP地址具有相同网络号的设备接口
* 不跨越路由器（第三及以上层网络设备）可以彼此物理联通的接口

我们将ip地址分为两部分，高位比特部分，我们当作网络号，凡是相同的，则说明属于同一个子网，地位比特分为主机号，区分不同的特定主机接口。

IP地址:
* 网络号(NetID) – 高位比特
* 主机号(HostID) – 低位比特


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-185473d86f6d34a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 有类IP地址


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e011c5154fda9eae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-dc246722fe4311a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# IP子网划分与子网掩码
为了进一步细分，有时候我们需要更多的分类，所以如何对子网进一步进行划分也是一个问题。
我们采取与之前相同的策略，将主机号的一部分比特位提出来作为子网号

IP地址:
* 网络号(NetID) – 高位比特
* 子网号(SubID) – 原网络主机号部分比特
* 主机号(HostID) – 低位比特


![image.png](http://upload-images.jianshu.io/upload_images/1234352-727c32e2eadd5bc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们拿两个比特位作为子网号，那么就可以分出四个子网

![image.png](http://upload-images.jianshu.io/upload_images/1234352-64d83a1821a6cc5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么问题又来了，如何确定是否划分了子网？利用多少位划分子网？
解答这个问题就需要利用子网掩码，这是一个非常重要的概念
子网掩码
形如IP地址:
* 32位
* 点分十进制形式

取值：
* NetID、 SubID位全取1
* HostID位全取0

> 子网地址+子网掩码→准确确定子网大小

例如：
* A网的默认子网掩码为： 255.0.0.0
* B网的默认子网掩码为： 255.255.0.0
* C网的默认子网掩码为： 255.255.255.0
* 借用3比特划分子网的B网的子网掩码为： 255.255.224.0

例如：
* 子网201.2.3.0， 255.255.255.0，划分为等长的4个子网
那么就要利用两个比特位：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d5e66e8cfa875be3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

路由器如何确定应该将IP分组转发到哪个子网？
将IP分组的目的IP地址与子网掩码按位与运算，提取子网地址

例如：
* 目的IP地址： 172.32.1.112，子网掩码： 255.255.254.0

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a157aa88f2c52f3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 子网地址： 172.32.0.0(子网掩码： 255.255.254.0)
* 地址范围： 172.32.0.0~172.32.1.255
* 可分配地址范围： 172.32.0.1~172.32.1.254
* 广播地址： 172.32.1.255
