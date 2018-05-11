我们知道ipv4协议提供的IP地址是有限的，为了解决IP地址不足的问题，于是就有了网络地址转换(NAT)，它的思想就是给一个局域网络分配一个IP地址就够了，对于这个网络内的主机，则分配私有地址，这些私有地址对外是不可见的，他们对外的通信都要借助那个唯一分配的IP地址。


![image.png](http://upload-images.jianshu.io/upload_images/1234352-82a1ad4cf7045e72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所有离开本地网络去往Internet的数据报的源IP地址需替换为相同的NAT。
IP地址: 138.76.29.7以及不同的端口号。
本地网络内通信的IP数据报的源与目的IP地址均在子网10.0.0/24内。

# NAT动机:
* 只需/能从ISP申请一个IP地址
• IPv4地址耗尽
* 本地网络设备IP地址的变更，无需通告外界网络
* 变更ISP时，无需修改内部网络设备IP地址
* 内部网络设备对外界网络不可见，即不可直接寻址(安全)

# NAT 实现
NAT实现通过利用端口号对内部地址和端口号进行转换，并维护一个转换表。
* 替换
• 利用(NAT IP地址,新端口号)替换每个外出IP数据报的(源IP地址,源端口号)
* 记录
• 将每对(NAT IP地址, 新端口号) 与(源IP地址, 源端口号)的替换信息存储到NAT转换表中
* 替换
• 根据NAT转换表，利用(源IP地址, 源端口号)替换每个进入内网IP数据报的(目的IP地址,目的端口号)，即(NAT IP地址, 新端口号)

下面通过一个实例说明：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-972a4575ab1a711d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们假设内网中的主机10.0.0.1向128.119.40.186, 80发送数据报。

首先要进行NAT转换，转换为本网惟一一个的IP地址138.76.29.7及其对应的端口号。
NAT路由器将数据报的源地址与端口号修改为138.76.29.7,5001,并记录到NAT转换表中。这样就可以在internet中进行进一步的转发和通信。

对于响应的报文，会先转发到唯一的IP地址上，然后通过nat转换表记录的ip和端口信息，再回到所需要接受消息的主机。
NAT路由器修改数据报的目的地址与目的端口号为: 10.0.0.1,3345, 并向内网转发

我们通过端口号来区分不同的主机和端口请求
16-bit端口号字段:
* 可以同时支持60,000多并行连接！
NAT主要争议:
* 路由器应该只处理第3层功能
* 违背端到端通信原则
• 应用开发者必须考虑到NAT的存在， e.g., P2P应用
* 地址短缺问题应该由IPv6来解决

# NAT穿透问题
客户期望连接内网地址为10.0.0.1的服务器
* 客户不能直接利用地址10.0.0.1直接访问服务器
* 对外唯一可见的地址是NAT地址: 138.76.29.7


![image.png](http://upload-images.jianshu.io/upload_images/1234352-963f40a1474586dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 解决方案1
静态配置NAT，将特定端口的连接请求转发给服务器
 e.g., (138.76.29.7, 2500) 总是转发给(10.0.0.1, 25000)

## 解决方案2
利用UPnP(Universal Plug and Play)互联网网关设备协议 (IGDInternet Gateway Device )自动配置:
* 学习到NAT公共IP地址(138.76.29.7)
* 在NAT转换表中，增删端口映射
![image.png](http://upload-images.jianshu.io/upload_images/1234352-1549429ee621fada.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 解决方案3
中继(如Skype)
* NAT内部的客户与中继服务器建立连接
* 外部客户也与中继服务器建立连接
* 中继服务器桥接两个连接的分组


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8a6f78b1322a60f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


