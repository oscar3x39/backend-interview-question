# SQL vs NoSQL
- SQL
 - ACID
 - A = Atomicity
 - C = Consistency
 - I = Isolation
 - D = Durability
- NoSQL

> NoSQL 因為其特性難以保證 ACID

> 由於 NoSQL 比較常被用在分散式的儲存環境，相較於 ACID，更加注重的是下面的 CAP

- CAP
 - C =  Consistency
 - A = Availability
 - P = Partition tolerance
```
將資料儲存為類似 JSON 的文件
doucment 是 key-value 的有序集合
資料庫中的每個doucment 不需要具有相同的數據結構
```

# MariaDB vs. MySQL vs. PostgreSQL

# Database normalization

# MySQL Performance Tuning Optimization

* Use InnoDB, Not MyISAM
* Use the Latest Version of MySQL
* Consider Using an Automatic Performance Improvement Tool
 * tuning-primer (MySQL 5.5 - 5.7)
 * MySQLTuner
 * phpMyAdmin Advisor
* Optimize Queries
* Use Indexes Where Appropriate
* Functions in Predicates
```
SELECT * FROM MYTABLE WHERE UPPER(COL1)='123'
```
* Avoid % Wildcard in a Predicate
```
SELECT * FROM person WHERE name LIKE "ch%"
```
* Specify Columns in SELECT Function
* Use ORDER BY Appropriately
* GROUP BY Instead of SELECT DISTINCT
```
BAD  SELECT DISTINCT name, address FROM person
GOOD SELECT name, address FROM person GROUP BY name
```
* JOIN, WHERE, UNION, DISTINCT
```
SELECT * FROM table1 INNER JOIN table2 ON table1.id = table2.id
SELECT * FROM table1, table2 WHERE table1.id = table2.id
```
* Use the EXPLAIN Function
* MySQL Server Configuration

> /my.cnf

```
query_cache_size
max_connection
innodb_buffer_pool_size
innodb_io_capacity
```

# OLTP (On-Line Transaction Processing) Database
- 應用 : Customer-oriented
- 回應時間 (response time) 要求較高。
- 併發 (concurrency) 要求較多。
- 資料處理量 (volume) 少。
- 交易 (transaction) 完整性高。
- 安全性 (security) 要求較高。

# OLAP (On-Line Analytical Processing)
- 應用 : Market-oriented
- 回應時間 (response time) 要求較低。
- 併發 (concurrency) 要求較少。
- 資料處理量 (volume) 多。
- 交易 (transaction) 完整性低。
- 安全性 (security) 要求較低。

# Data warehouse
- 應用 : Subject-oriented
- 熱資料 (Hot) 本地、快取、粒度、一致性。
- 暖資料 (Warm) 分布、快取、複製。
- 冷資料 (Cold) 索引、壓縮、合併、備份。

# Store variable using mysql

### SET
```
SET @v1 := (SELECT COUNT(*) FROM user_rating);
SELECT @v1;
```
### INTO
```
SELECT count(*) FROM TABLE_NAME INTO @var_count;
SELECT @var_count;
```

# Design Schema
* composite primary key 複合主鍵
當你需要auto_increment特性的時候，你不應該再用複合主鍵，這樣喪失了auto_increment的唯一特性
* Multiple-Column Indexes 複合索引
建立複合索引時， Columns 的組合的順序會影響 Query是否使用 Index。

# Architecture
* Load balancing
 * HAProxy
 * Nginx
 * ProxySQL
* Read/Write Splitting 讀寫分離
* Master-Slave Replication 主從式架構
* Scale up
  * application
  * connection
  * database
  * file system
  * os kernel
  * hardware
* Scale out
  * Replication
  * Clustering
  * Sharding
  * Disaster Recovery
  * Multi Regional Resiliency
