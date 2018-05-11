主机、路由器网络层主要功能：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-fb6d8c18e715ca7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们将在这篇文章详细介绍ip数据报的格式
首先，ip数据报分为两部分，首部和数据

![image.png](http://upload-images.jianshu.io/upload_images/1234352-7b3e870cd03f1794.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们详细分析首部各字段的意义

![image.png](http://upload-images.jianshu.io/upload_images/1234352-5f5670eb604000a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 版本号字段占4位： IP协议的版本号，一般有两个值，如果为4就代表是IPv4，6就代表是IPv6协议。 4→IPv4， 6 → IPv6
***
> 首部长度字段占4位： IP分组首部长度，这里是以四个字节为单位，如果值为5，则表示首部长度为20个字节（5×4），从图中也可以看到，ip首部长度最短应该是20个字节，除掉可变部分，固定部分就是20个字节。
***
服务类型(TOS)字段占8位：指示期望获得哪种类型的服务
* 1998 年这个字段改名为区分服务
* 只有在网络提供区分服务(DiffServ)时使用
* 一般情况下不使用，通常IP分组的该字段(第2字节)的值为00H
***
总长度字段占16位： IP分组的总字节数(首部+数据)
* 最大IP分组的总长度： 65535B
* 最小的IP分组首部： 20B
* IP分组可以封装的最大数据： 65535-20=65515B
***
生存时间（ TTL） 字段占8位： IP分组在网络中可以通过的路由器数（或跳步数）
* 路由器转发一次分组， TTL减1
* 如果TTL=0，路由器则丢弃该IP分组
***
协议字段占8位： 指示IP分组封装的是哪个协议的数据包
* 实现复用/分解
* E.g. 6为TCP，表示封装的为TCP段； 17为UDP， 表示封装的是UDP数据报
***
首部校验和字段占16位：实现对IP分组首部的差错检测
* 计算校验和时，该字段置全0
* 采用反码算数运算求和，和的反码作为首部校验和字段
* 逐跳计算、逐跳校验
***
源IP地址、目的IP地址字段各占32位：分别标识发送分组的源主机/路由器(网络接口)和接收分组的目的主机/路由器（网络接口）的IP地址
***
选项字段占长度可变，范围在1~40B之间：携带安全、源选路径、时间戳和路由记录等内容 ** 实际上很少被使用 **
***
填充字段占长度可变，范围在0~3B之间：目的是补齐整个
首部，符合32位对齐，即保证首部长度是4字节的倍数

# ip分片
在介绍ip数据报首部字段的时候，我们忽略了第二行字段的介绍，因为这一行的字段涉及到ip数据报的分片，我们将先介绍ip数据报的分片，再来介绍这几个字段的含义。

网络链路存在MTU (最大传输单元)—链路层数据帧可封装数据的上限。** 不同链路的MTU不同 **。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-0f3a9d179b676904.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

大IP分组向较小MTU链路转发时， 可以被“分片” (fragmented)
* 1个IP分组分为多片IP分组
* IP分片到达目的主机后进行“重组”(reassembled)

IP首部的相关字段用于标识分片以及确定分片的相对顺序
* 总长度、标识、标志位和片偏移


![image.png](http://upload-images.jianshu.io/upload_images/1234352-0398c81ffbdeece2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-6d108150df41b0bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 标识字段占16位：标识一个IP分组
* IP协议利用一个计数器，每产生IP分组计数器加1，作为该IP分组的标识
***
 标志位字段占3位：
* DF (Don't Fragment)
* MF (More Fragment)

> ![image.png](http://upload-images.jianshu.io/upload_images/1234352-03fde6b3408a43c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>* DF =1：禁止分片；
DF =0：允许分片
* MF =1：非最后一片；
MF =0：最后一片(或未分片)
***
片偏移字段占13位：一个IP分组分片封装原IP分组数据的
相对偏移量
* 片偏移字段以8字节为单位

# ip分片过程
* 假设原IP分组总长度为L，待转发链路的MTU为M
* 若L>M，且DF=0，则可以/需要分片
* 分片时每个分片的标识复制原IP分组的标识
* 通常分片时，除最后一个分片，其他分片均分为MTU允许的最大分片
* 一个最大分片可封装的数据应该是8的倍数， 因此， 一个最大分片可封装的数据为：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-d2fb86a4b65ae61b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 需要的总片数为：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-de75b79861a64756.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-5288fd4f55058b44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3ad438056f04dbbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
