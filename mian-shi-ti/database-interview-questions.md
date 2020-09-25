# 数据库面试题

1、事务的四个特性。【[链接](../database/mysql/transaction.md#1-shi-wu-de-si-da-te-xing-acid)】

2、三个范式。

3、事务的隔离级别？各种隔离级别解决的问题。【链接】

4、为什么索引要用B 树不用二叉树？

5、数据库索引？索引原理。

5、知不知道字典树，字典树能不能作为索引？

## MySQL

1、innodb 引擎的结构

* Innodb中的索引用了什么数据结构？

2、什么情况下会用到哈希索引，会存到硬盘吗？

3、给了一个查询语句问怎么建索引。

4、慢查询。

5、InnoDB的锁机制

讲了粒度分类的表锁行锁意向锁，讲了排斥性分类的读锁写锁，四种锁算法，`record lock`、`gap lock`、`next-key lock`、`previous-key lock`。

6、事务的四个隔离级别？MySQL InnoDB默认的是啥？【[链接](../database/mysql/transaction.md#3-shi-wu-de-ge-li-ji-bie)】

7、脏读是什么？怎么解决脏读？

8、幻读是什么？怎么解决幻读？（next-key lock锁和mvcc原理）

9、主键索引和普通索引有什么区别？

10、索引什么时候会失效？

## Redis

1、zset的底层是啥？跳表。

2、redis持久化的方式。【链接】

3、rdb结束或者aof重写结束的时候，子进程如何通知父进程？

4、redis为什么快？内存+单线程。

5、Redis key要设置过期时间吗？为什么？

6、管理100w个连接也很困难，epoll是怎么做到的？

7、redis如何压缩内存的？

8、redis主从复制方式？

