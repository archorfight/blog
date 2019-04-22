# 事务
 ## 什么是事务
> 事务是数据库操作的最小单元，是作为单个逻辑工作单元执行的一系列操作；事务时以组不可再分割的操作集合（工作逻辑单元）；
1. 通俗来说就是 事务中的每一个操作 都是全部失败或者全部成功，不会出现第三种状态。
2. 典型事务场景（转账）：
```
update user_account set balance=balance-1000 where userID =3;
udate user_account set balance=balance+1000 where userID =1;
```
3. mysql中怎么开启事务：
```
begin/ start transaction  -- 手工
commit /rollback   -- 事务提交或回滚
set session autocommit= on/off -- 设定事务是否自动开启
```
  + 3.1 JDBC编程
```java
connection.setAutoCommit(boolean);
```
	+3.2 spring 事务AOP编程
```
expression=execution(com.gpedu.*.*(..))
```
4. 事务ADID特性
+ 原子性（Atomicity）
		最小的工作单元，整个工作单元要么一起提交成功，要么全部失败回滚
+ 一致性（Consistency）
		事务中操作的数据及状态改变是一致的，即写入资料的结果必须完全符合预设的规则，不会因为出现系统意外等原因导致状态的不一致
+ 隔离性（Isolation）
		一个事务所操作的数据在提交之前，对其他事务的可见性设定（一般设定为不可见）
+ 持久性（Durability）
		事务所作的修改就会永久保存，不会因为系统意外导致数据的丢失
5. 事务并发带来的问题
+ 脏读
![脏读](../images/脏读.png)
A事务 查询到了 B事务更新的数据，之后B事务将数据回滚，造成次数据在数据库不存在，而A事务查询到了的情况，谓之脏读（常见于update）。

+ 不可重复读
![不可重复读](../images/不可重复读.png)
A事务 查询了一次数据后，B事务将数据修改了，A事务再去查询的时候前后的 数据不一致，谓之不可重复读（常见于update）。
+  幻读
![幻读](../images/幻读.png)
A事务 查询数据，查询到了1条数据，


# 锁

# MVCC