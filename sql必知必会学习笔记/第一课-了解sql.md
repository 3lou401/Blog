# 准备
首先阅读这本书，必须先了解这本书里sql语句所操作的数据库表。
也就是书附录中的样例表，深入理解这个样例表不仅可以帮助我们设计好的数据库结构，也能打下基础，更好的实践本书中的代码。

# 样例表
本书使用的样例表的是假想一个玩具经销商使用的一个订单录入系统，这个系统的数据库主要用来完成一下这些任务：
* 管理供应商
* 管理产品目录
* 管理顾客列表
* 录入顾客订单

粗看上面这些任务，我们自然会考虑到至少需要这么几个表：
* 供应商表，存储供应商的信息
* 产品表，存储产品信息
* 顾客表，存储顾客信息
* 订单表，存储订单信息
但是，嘻嘻仔细分析，会发现这里的订单信息无法用一个表存储，一个订单可能会包括很多的产品，而这些产品的数量是不固定的，无法用一个表来表示，所以我们考虑添加一个表来表示订单。
这里采用的是一个订单表，不存储订单的细节，另外一个订单item表存储订单的物品和细节。

所以，经过以上的分析，建立了五个表。
下面我们一一介绍这五个表：
* Vendors表
这个表存储供应商的信息，每个供应商对应一条记录，用唯一的供应商ID来标识。并且以ID作为这个表的主键
具体的表结构如下：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-4e7562e39a4ce94f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Products表
products表用来表示产品目录，存储产品信息，每个产品有一个产品ＩＤ但同时借助vend_id关联到供应商。设置主键和外键

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-967c480f060a2e73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* customers表
customers表用来存储顾客的信息，每个记录表示一条顾客的信息，用ID作为其主键，每个顾客的ID都是唯一的。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-fbe2a829e4735bf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Orders表
orders表存储订单，但不是订单的细节，这个订单表，只有三个字段，一个是每个订单唯一的编号也就是ID，一个订单的日期，一个是关联到这个订单的顾客ID所以设置一个外键，关联到相应的顾客。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-672ceedbdf7fe8a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* orderitems表
这个表存储每个订单中的实际物品，每个订单的每个物品一行。所以对于orders表的每一行也就是每个订单，由于物品数量的不一样，所以在orderitem表中，有一行或者很多行，每个订单物品由订单号加订单物品号唯一标识（也就是表示该订单的第一个物品，第二个物品之类的），每个订单物品包好该物品的产品ID。
所以这个表采用联合主键，分别是订单号（关联到order表），和订单物品号为主键
然后还要设计两个外键，分别是订单号，和产品ID

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-418b3d5453fd5d3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

总的表的结构关系如下图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-25c88b7675f71bc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# sql初探
数据库的基本概念：数据库，表，列和数据类型，行，主键。
这里提一下主键需要满足的条件：
* 任意两行的主键值不能相同
* 每一行必须有一个主键，不能为null
* 主键的值不允许修改或者更新
* 主键值不能重用，某行从表格中删除，他的值不能赋给新行
Sql是一种专门与数据库沟通的语言。
