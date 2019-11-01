# mysql存储引擎
mysql存储引擎有多种，常见的有MyISAM和InnoDB
+ 1.InnoDB支持事务、外键，MyISAM不支持
+ 2.Inn.DB是mysql5.5后默认的
+ 3.MyISAM适合查询以及插入为主的应用；InnoDB适合频繁修改以及涉及到安全性较高的应用
+ 4.InnoDB不支持FULLTEXT类型索引，不保存表的行数
+ 5.自增长字段，InnoDB必须包含只有该字段的索引，但是MyISAM表中可以和其他字段一起建立联合索引
+ 6.清空整张表时，InnoDB是一行行删除，很慢；MyISAM则是重建表
+ 7.InnoDB支持行锁，对于like之类的sql还是锁整张表

# 数据库事务的特性，怎么理解隔离性
+ 原子性 要么完成，要么全不完成
+ 一致性 实际并发的结果与理论串行结果一致
+ 隔离性 同一时间仅有一个事务处理同一数据，处理这一数据的其他事务串行化处理，失败的事务不影响数据。
+ 持久性 以提交的事务永久地更改数据保存在数据库中，不会回滚。

实现的方式有两种，write ahead logging 和 shadow paging<br>
后者简单开销大，一个事务修改的块都提交到磁盘，很难并发<br>
前者对数据的修改必须记录到日志中，日志被冲刷到永久存储器中。不需要冲刷数据页。出现崩溃的时候，从日志恢复数据库。<br>
SQL标准中的四个隔离级别<br>
Read-uncommited Read-commited Repeatable-read serializable<br>
问题：脏读、非重复读 幻读（查询了未提交插入的表数据）<br>
mysql默认repeatable-read，其他大多read-commited<br>
