# 网络层服务
网络层提供的服务是，主机与主机的数据传输。发送主机向接收主机发送数据段（ segment）。首先，发送主机将来自传输层的数据段封装到数据报中，然后传输给接收主机，途中可能会经过路由器，路由器和主机一样，都运行网络层的协议，路由器会根据ip数据报的头部信息选择转发路径。接收主机接收到数据报之后，再向上交付，交给传输层。
> 网络层核心功能-转发与路由

转发(forwarding):将分组从路由器的输入端口转移到合适的输出端口
路由(routing): 确定分组从源到目的经过的路径。

数据分组传输之前两端主机需要首先建立虚拟/逻辑连接。网络设备（如路由器）参与连接的建立。
网络层连接与传输层连接的对比:
* 网络层连接: 两个主机之间 (路径上的路由器等网络设备参与其中)
* 传输层连接: 两个应用进程之间（对中间网络设备透明）

## 网络层的服务类型
网路层服务类型主要有两种，分别是无连接服务(connection-less service)和连接服务(connection service)

### 无连接服务
* 不事先为系列分组的传输确定传输路径
* 每个分组独立确定传输路径
* 不同分组可能传输路径不同
* 典型的无连接服务代表是数据报网络(datagram network )

### 连接服务
* 首先为系列分组的传输确定从源到目的经过的路径(建立连接)
* 然后沿该路径（连接）传输系列分组
* 系列分组传输路径相同，所有分组的传输路径都是相同的
* 传输结束后拆除连接
* 典型的连接服务的代表就是虚电路网络(virtual-circuit network )

> 数据报(datagram)网络与虚电路(virtual-circuit)网络是典型两类分组交换网络
* 数据报网络提供网络层无连接服务
* 虚电路网络提供网络层连接服务
* 类似于传输层的无连接服务（ UDP）和面向连接服务（ TCP），但是网络层服务：主机到主机服务,网络核心实现

下面我们分别详细介绍这两种重要的服务，数据报网络和虚电路网络

# 虚电路网络
> 虚电路：一条从源主机到目的主机， 类似于电路的路径(逻辑连接)
* 分组交换
* 每个分组的传输利用链路的全部带宽
* 源到目的路径经过的网络层设备共同完成虚电路功能

简单的说，就是先逻辑上建立一条连接，确定一条传输的路径，然后所有分组的传输都是走这同一条路径，而且每个分组独占这条链路的全部带宽。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d35655d7d5559470.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虚电路的通信过程分为三步：呼叫建立(call setup)→数据传输→拆除呼叫

呼叫建立后，会唯一确定一条传输的链路，这条链路会有一个标识，随后每个分组携带的不是目的地址，而是这条链路的标识。每个分组携带虚电路标识(VCID)，而不是目的主机地址，虚电路经过的每个网络设备（ 如路由器） ， 维护每条经过它的虚电路连接状态。

## 虚电路VC的具体实现
1. 从源主机到目的主机的一条路径
2. 虚电路号（ VCID） ， 沿路每段链路一个编号
3. 沿路每个网络层设备（如路由器）， 利用转发表记录经过的每条虚电路

也就是说，路由器是根据虚电路号来进行转发的，所以分组携带的不是目的地址而是虚电路号。
沿某条虚电路传输的分组，携带对应虚电路的VCID，而不是目的地址
同一条VC ，在每段链路上的VCID通常不同
路由器转发分组时依据转发表改写/替换虚电路号


![image.png](http://upload-images.jianshu.io/upload_images/1234352-1a07bef80c5ae33e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-22bff9de7d45b9bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们家庭用的固定电话机通常就是用的虚电路。

# 数据报网络
> 网络层无连接
每个分组携带目的地址
路由器根据分组的目的地址转发分组
* 基于路由协议/算法构建转发表
* 检索转发表
* 每个分组独立选路

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ecfed338626ce99d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
数据报的转发表，我们最直观的想法当然是根据不同的目的地址确定转发出口，如图

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ca9b3e766e4d00bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但这样会造成一个问题，我们有四十多亿个ip地址，如果每个ip都这样配置一个输出链路，显然转发表会变的巨大无比，转发效率也会降低，所以我们采用的解决方法是通过ip地址范围进行转发

![image.png](http://upload-images.jianshu.io/upload_images/1234352-56dba307414bdf12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如下图的数据转发表

![image.png](http://upload-images.jianshu.io/upload_images/1234352-95ae6001bf24be04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在匹配的时候，我们采用最长前缀匹配策略，也就是尽可能选择细的地址范围。

> 最长前缀匹配优先：在检索转发表时，优先选择与分组目的地址匹配前缀最
长的入口（ entry）。


![image.png](http://upload-images.jianshu.io/upload_images/1234352-c370575d2e70b6cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 数据报网络与虚电路网络的对比
Internet (数据报网络)
* 计算机之间的数据交换
* “弹性” 服务，没有严格时间需求
* 链路类型众多
* 特点、性能各异
* 统一服务困难
* “智能” 端系统 (计算机)
* 可以自适应、性能控制、差错恢复
* 简化网络，复杂“边缘”

ATM (VC网络)
* 电话网络演化而来
* 核心业务是实时对话：
* 严格的时间、可靠性需求
* 需要有保障的服务
* “ 哑(dumb)” 端系统（非智能）
* 电话机
* 传真机
* 简化“边缘” ，复杂网络
