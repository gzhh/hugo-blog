---
title: "MySQL 锁"
date: 2020-02-23T09:43:53+08:00
draft: false
tags: [MySQL]
categories: [Database]
---

### 锁介绍
假设我们有一个商品表 item，里面有两个字段 id 和 store
```
+----+-------+
| id | store |
+----+-------+
|  1 |     1 |
+----+-------+
```
一般我们在生成订单之前会先确定 store 商品库存是否大于你要购买的数量，然后更新库存

```
-- 查询
SELECT store FROM item WHERE id = 1;
-- 更新
UPDATE item SET store = store - 1 WHERE id = 1;
```

但是这样在用户大量并发操作的时候会是不安全的，比如第一个用户 SELECT 到的 store 是 1，但是当他准备 UPDATE 的时候，可能已经有人把库存 store 扣成了 0，那么第一个用户再去 UPDATE 的时候，就可能出现负库存的情况，因此我们必须通过事务和锁机制来确保读取及提交的数据都是正确的。

在 MySQL 终端我们可以这样测试
```
SET AUTOCOMMIT=0;
BEGIN;
SELECT store FROM item WHERE id=1 FOR UPDATE;
```
此时 item 表中 id=1 的这行数据被锁住（InnoDB 的 Row Lock 或 Table Lock）其他事务必须等待此次事务提交后才能执行；
`SELECT store FROM item WHERE id=1 FOR UPDATE; `可以确保库存 store 在别的事务读取到的数字是正确的；
然后我们修改库存，提交数据写入数据库，item 表中 id=1 的这行数据解锁。
```
UPDATE item SET store = store - 1 WHERE id=1; COMMIT;
```

**注：**

我们可以开启两个 MySQL 终端进行测试，关闭事务自动提交，SET AUTOCOMMIT=0。然后在第一个终端开启事务，并使用 SELECT ... FROM UPDATE 去查询某条记录的 store，然后在第二个终端尝试使用 SELECT ... FROM UPDATE 也去查询某条记录的 store，这时会发现第二个终端的查询会被锁住。 

### Row Lock 与 Table Lock
假设 item 表有 id、name、store 三个字段，id 是主键
1. (明确指定主键，并且有此数据，row lock)
    - SELECT * FROM item WHERE id='1' FOR UPDATE;
2. (明确指定主键，若查无此数据，无lock)
    - SELECT * FROM item WHERE id='-1' FOR UPDATE;
3. (无主键，table lock)
    - SELECT * FROM item WHERE name='Zhao' FOR UPDATE;
4. (主键不明确，table lock)
    - SELECT * FROM item WHERE id<>'1' FOR UPDATE;
5. (主键不明确，table lock)
    - SELECT * FROM item WHERE id LIKE '1' FOR UPDATE;

### 乐观锁和悲观锁策略
- 悲观锁：
    在读取数据时锁住那几行，其他对这几行的更新需要等到悲观锁结束时才能继续 。
- 乐观锁：
    读取数据时不锁，更新时检查是否数据已经被更新过，如果是则取消当前更新，一般在悲观锁的等待时间过长而不能接受时我们才会选择乐观锁。

### 参考
- [mysql事务，select for update，及数据的一致性处理](https://www.cnblogs.com/houweijian/p/5869243.html)
- [MysqL_select for update锁详解](https://www.cnblogs.com/wt645631686/p/7987149.html)

