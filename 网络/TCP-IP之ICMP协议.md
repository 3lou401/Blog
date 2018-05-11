互联网控制报文协议(ICMP)

在互联网传输过程中，IP数据报难免会出现差错，通常出现差错，处理方法就是丢弃，但是一般，出现差错后，会发送ICMP报文给主机，告诉它一些差错信息，以及对当前的网络状态进行一个探寻。

两类ICMP 报文:
# 差错报告报文(5种)
• 目的不可达
• 源抑制(Source Quench)
当路由器发现自己的缓存已满，就会发送源抑制报文，告诉它降低发送速率
• 超时/超期
就是ttl超时
• 参数问题
如果发现IP数据报首部某些参数出现错误
• 重定向 (Redirect)
如果发现源主机发错了，就发这个，让源主机重新定向
# 网络探询报文(2组)
• 回声(Echo)请求与应答报文(Reply)
• 时间戳请求与应答报文

![image.png](http://upload-images.jianshu.io/upload_images/1234352-8c37cec58b2b4bdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 例外情况
## 几种不发送 ICMP差错报告报文的特殊情况：
* 对ICMP差错报告报文不再发送 ICMP差错报告报文
* 除第1个IP数据报分片外， 对所有后续分片均不发送ICMP差错报告报文
* 对所有多播IP数据报均不发送 ICMP差错报告报文
* 对具有特殊地址（ 如127.0.0.0 或 0.0.0.0） 的IP数据报不发送ICMP 差错报告报文
## 几种 ICMP 报文已不再使用
* 信息请求与应答报文
* 子网掩码请求和应答报文
* 路由器询问和通告报文

# ICMP报文封装到IP数据报中传输

![image.png](http://upload-images.jianshu.io/upload_images/1234352-cf8950e310fbf9f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ICMP差错报告报文数据封装

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6d65f0b4a44fd8eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果是udp段，qianbagezijie前八个字节就是udp头，如果是tcp，前八个字节封装了源端口号和目的端口号

# ICMP的应用举例： Traceroute

源主机向目的主机发送一系列UDP数据报
* 第1组IP数据报TTL =1
* 第2组IP数据报TTL=2, etc.
* 目的端口号为不可能使用的端口号

当第n组数据报(TTL=n)到达第n个路由器时：
* 路由器丢弃数据报
* 向源主机发送ICMP报文(type=11, code=0)
* ICMP报文携带路由器名称和IP地址信息
* 当ICMP报文返回到源主机时，记录RTT

停止准则:
* UDP数据报最终到达目的主机
* 目的主机返回“目的端口不可达” ICMP报文 (type=3,code=3) 源主机停止
