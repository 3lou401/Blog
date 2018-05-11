Mysql 数据库中，最常用的两种引擎是 innordb 和 myisam。InnoDB 是 Mysql 的默
认存储引擎。

1. 事务处理上方面
MyISAM 强调的是性能，查询的速度比 InnoDB 类型更快，但是不提供事务支持。
InnoDB 提供事务支持事务。

2. 外键
MyISAM 不支持外键，InnoDB 支持外键。

3. 锁
MyISAM 只支持表级锁，InnoDB 支持行级锁和表级锁，默认是行级锁，行锁大幅度提高了多用户并发操作的性能。innodb 比较适合于插入和更新操作比较多的情况，而 myisam 则适合用于频繁查询的情况。另外，InnoDB 表的行锁也不是绝对的，如果在执行一个 SQL 语句时，MySQL 不能确定要扫描的范围，InnoDB 表同样会锁全表，例如 update table set num=1 where name like “%aaa%”。

4. 全文索引
MyISAM 支持全文索引， InnoDB 不支持全文索引。innodb 从 mysql5.6 版本开始提供对全文索引的支持。

5. 表主键
MyISAM：允许没有主键的表存在。
InnoDB：如果没有设定主键，就会自动生成一个 6 字节的主键(用户不可见)。

6. 表的具体行数
MyISAM：select count(*) from table,MyISAM 只要简单的读出保存好的行数。因为
MyISAM 内置了一个计数器，count(*)时它直接从计数器中读。
InnoDB：不保存表的具体行数，也就是说，执行 select count(*) from table 时，InnoDB要扫描一遍整个表来计算有多少行。


一张表,里面有 ID 自增主键,当 insert 了 17 条记录之后,删除了第 15,16,17 条记录,再把 Mysql 重启,再 insert 一条记录,这条记录的 ID 是 18 还是 15 ？

如果表的类型是 MyISAM， 那么是 18。因为 MyISAM 表会把自增主键的最大 ID 记录到数据文件里， 重启MySQL 自增主键的最大 ID 也不会丢失。

如果表的类型是 InnoDB， 那么是 15。InnoDB 表只是把自增主键的最大 ID 记录到内存中， 所以重启数据库会导致最大 ID 丢失
