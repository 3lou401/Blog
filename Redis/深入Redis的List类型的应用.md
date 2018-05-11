# Redis的List
Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

因为Redis的List同时支持头和尾的操作，所以实际上我们直接理解为List为一个双向的链表，即可用作栈，也可以用作队列。

# List的应用场景

我们假设要获得最新的10个用户的登录的信息，传统关系型数据库的话，我们可以如下操作：
```sql
select * from user order by logintime limit 10
```
我们可以很简单的用一个sql语句就完成这个查询操作。

但是问题在于，假设我们现在数据库的数据量很大，也就是用户的数量很多，那么我们遍历查询一次所需要的时间是很多的，也就是操作会变慢，而且对数据库的负载也增加了，同时如果我们对相应字段建立索引的话，那么对数据库的资源也是一种消耗。

这个时候就轮到nosql上场了，我们可以利用redis的list类型，在list中只保留最新的10个数据，每进来一个新数据，就删除一个旧数据，这样我们在list中维护的就永远是最新登录的十个用户。

# Redis 列表命令

## Lpush 命令
Redis Lpush 命令将一个或多个值插入到列表头部。 如果 key 不存在，一个空列表会被创建并执行 LPUSH 操作。 当 key 存在但不是列表类型时，返回一个错误。
redis Lpush 命令基本语法如下：
```
redis 127.0.0.1:6379> LPUSH KEY_NAME VALUE1.. VALUEN
```

## Rpop 命令
Redis Rpop 命令用于移除并返回列表的最后一个元素。
语法
redis Rpop 命令基本语法如下：
```
redis 127.0.0.1:6379> RPOP KEY_NAME 
```
返回值:
列表的最后一个元素。 当列表不存在时，返回 nil 。

## Lrange 命令
Redis Lrange 返回列表中指定区间内的元素，区间以偏移量 START 和 END 指定。 其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推。
语法
redis Lrange 命令基本语法如下：
```
redis 127.0.0.1:6379> LRANGE KEY_NAME START END
```
返回值
一个列表，包含指定区间内的元素。

## ltrim
Redis Ltrim 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。
下标 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推。
语法
redis Ltrim 命令基本语法如下：
```
redis 127.0.0.1:6379> LTRIM KEY_NAME START STOP
```
返回值
命令执行成功时，返回 ok 。

##  Lrem 命令

Redis Lrem 根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素。
COUNT 的值可以是以下几种：
- count > 0 : 从表头开始向表尾搜索，移除与 VALUE 相等的元素，数量为 COUNT 。
- count < 0 : 从表尾开始向表头搜索，移除与 VALUE 相等的元素，数量为 COUNT 的绝对值。
- count = 0 : 移除表中所有与 VALUE 相等的值。


语法
redis Lrem 命令基本语法如下：
```
redis 127.0.0.1:6379> LREM KEY_NAME COUNT VALUE
```
返回值
被移除元素的数量。 列表不存在时返回 0 。


## Linsert 命令

Redis Linsert 命令用于在列表的元素前或者后插入元素。 当指定元素不存在于列表中时，不执行任何操作。 当列表不存在时，被视为空列表，不执行任何操作。 如果 key 不是列表类型，返回一个错误。
语法
redis Linsert 命令基本语法如下：
```
redis 127.0.0.1:6379> LINSERT KEY_NAME BEFORE EXISTING_VALUE NEW_VALUE 
```
返回值
如果命令执行成功，返回插入操作完成之后，列表的长度。 如果没有找到指定元素 ，返回 -1 。 如果 key 不存在或为空列表，返回 0 。

## Lset 命令

Redis Lset 通过索引来设置元素的值。
当索引参数超出范围，或对一个空列表进行 LSET 时，返回一个错误。
关于列表下标的更多信息，请参考 [LINDEX 命令](http://www.runoob.com/redis/lists-lindex.html)。
语法
redis Lset 命令基本语法如下：
```
redis 127.0.0.1:6379> LSET KEY_NAME INDEX VALUE
```
返回值
操作成功返回 ok ，否则返回错误信息。
