# 事务

#### ACID
+ 原子性（Atomicity）
+ 一致性（Consistency）
+ 隔离性（Isolation）
+ 持久性（Durability）

#### 隔离级别
+ READ UNCOMMITTED
+ READ COMMITTED
+ REPEATABLE READ（mysql默认）
+ SERIALIZABLE


+ 脏读：一个事务可以看到其他事务未修改的数据
+ 不可重复读：一个事务可以看到其他事务已修改的数据，一个事务中多次读同一个数据，结果不同
+ 幻读：一个事务会读取到其他事务的insert/delete数据

| 隔离级别         | 脏读  | 不可重复读 | 幻读   |
| ----           | ---- | ----     | ----  |
|READ UNCOMMITTED| yes  | yes      | yes   |
|READ COMMITTED  | no   | yes      | yes   |
|REPEATABLE READ | no   | no       | yes ? |
|SERIALIZABLE    | no   | no       | no    |

#### Innodb 通过mvcc来解决幻读问题

#### 死锁，两个或多个事务在同一资源上相互占用，并请求对方占用的资源
+ 死锁检测和死锁超时机制
+ Innodb将持有最少行及排他锁的事务回滚

#### 

  