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


