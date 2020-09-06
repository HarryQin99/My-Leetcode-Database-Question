## Transaction(事务）
### Definition
Transaction is a logical unit of work that must either be entirely completed or aborted.
事务指的是满足 ACID 特性的一组操作，可以通过 Commit 提交一个事务，也可以使用 Rollback 进行回滚。
### Transaction Properties(ACID)
+ Atomicity(原子性): A transaction is treated as a single, indivisible, logical unit of work. All operations in a transaction must be completed; if not them the transaction si aborted.

+ Consistency(一致性): Constraints that hold before a transaction must also hold after it, multiple users accessing the same data see the same value.

+ Isolation(隔离性): Changes made during execution of a transaction connot be seen by other transactions unitil this one is completed.

+ Durability(持久性): When a transaction is complete, the changes made to the database are permanent, even if the system fails.

[中文解释 By JavaGuide](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/MySQL.md#%E4%BA%8B%E7%89%A9%E7%9A%84%E5%9B%9B%E5%A4%A7%E7%89%B9%E6%80%A7acid)

### Problem caused by Concurrent access(并发事务带来的问题)
+ The Lost Update problem(**丢失修改**): 一个事务的更新覆盖了另一个事务的更新
举例理解: 两个人P1 和 P2 同时想取钱, 原余额为1000。 P1取了800, 剩下200。P2想取100, 剩下900。P1提出修改并且生效以后P2也进行修改操作。这时候P1的修改就被丢失了。
+ The Uncommitted Data problem/Dirty read(**脏读**): 一个事务读取了另一个事务未提交的数据。
Uncommitted data occurs when two transactions execute concurrently and the first is rolled back after the second has already assessed the uncommitted data

The Inconsistent Retrieval problem:

Occurs when one transaction calculates some aggregate functions over a set of data, while other transactions are updating the data.
+ **不可重复读**: 一个事务两次读取同一个数据，两次读取的数据不一致。
+ **幻读**: 一个事务两次读取一个范围的记录，两次读取的记录数不一致。
[中文解释](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B3%BB%E7%BB%9F%E5%8E%9F%E7%90%86.md#%E4%BA%8C%E5%B9%B6%E5%8F%91%E4%B8%80%E8%87%B4%E6%80%A7%E9%97%AE%E9%A2%98)

### Concurrency control methods
1. Lock
+ Guarantees exclusive use of a data item to a current transaction
+ Required to prevent another transaction from reading inconsistent data
2. Timestamp
+ 对于每个事务, 给予一个独一的t时间戳. 当某一事务想要读或者写的时候, DBMS会比较这个事务的时间戳与上一次写这个这个数据上的事务的时间戳, 也就是这个数据的W_TS(X)来判断这个事务是否可以读或写.
3. Optimistic
+ Based on the assumption that the majority of database operations do not conflict.

#### Lock Granularity
+ Page-level lock(Not commonly used now)
+ Row-level lock
  + Improves data availability but with high overhead
  + Currently the most popular approach
+ Field-level lock
  + Most flexible lock but requires an extremely high level of overhead
  + Not commonly used

#### Types of Locks
+ Binary Locks
  + has onlu two states: *locked* and *unlocked*
  + eliminates "Lost Update" problem(The lock is not released until the statement is completed)
+ The locks allowed both Shared and Exclusive Lockes

#### Shared and Exclusive Locks
+ Exclusive lock(写锁)
  + must be used when transaction intends to WRITE
  + granted if and only if no other locks are held on the data item
  要求：加锁时没有认识其他锁在当中
+ Shared lock(读锁)
  + other transactions are also granted Read access
  + issued when a transaction wants to READ data, no Exclusive lock is hold on the data item
可以存在多个读锁，但是一旦有一个写锁就无法再加任何锁

#### Deadlock
Candition that occurs when two transactions wait for each other to unlock data
+ Each waits to get a data item which the other transaction is already holding
+ Could wait forever if not dealt with
+ Only happends with exclusive locks



