# SQL (Colume-Based DB) vs NoSQL (Document DB)
- SQL
 - ACID
 - A = Atomicity
 - C = Consistency
 - I = Isolation
 - D = Durability

# 優點
```
業務同時讀取 multiple row 時效率高
特定的業務場景(海量數據進行統計)下才能體現
節省 I/O
具備更高的儲存壓縮比
```
# 缺點
```
RDBMS 通過 index 來達到快速查詢的目的
如果通過 index 來滿足，則 index 的數量會非常多
而 like 查詢是 full table scan，效率非常低
```

- NoSQL (Document DB)

> NoSQL 因為其特性難以保證 ACID

> 由於 NoSQL 比較常被用在分散式的儲存環境，相較於 ACID，更加注重的是下面的 CAP

- CAP
 - C = Consistency 一致性 - 保證在各個有儲存資料的分區都一定拿得到最新版本的資料。
 - A = Availability 可用性 - 當存取資料庫時一定可以拿到資料，不會出錯（不會出現拿不到資料的狀況，但拿到的資料不保證是最新版本。）
 - P = Partition tolerance 分區容忍 - 即使任一節點發生錯誤，整個系統還是能正常運作。
```
將資料儲存為類似 JSON 的文件
doucment 是 key-value 的有序集合
資料庫中的每個doucment 不需要具有相同的數據結構
```

# CAP 組合情境
### AP Database — 犧牲一致性的分散式資料庫
```
在分區容忍的前提下（因為都已經採用分散式架構了），犧牲一致性來提高可用性（盡可能每次都得到回應），不過雖然犧牲了一致性，卻仍可以達到最終一致性（Eventual Consistency，例如總統大選開票時每家新聞台的即時票數都不一致，但在選舉結束後的票數還是會達成一致。），適合在需要快速讀寫，但資料對於一致性的需求較低的場景，例如臉書或各大媒體平台的按讚系統。

例如：Amazon DynamoDB
```
### CP Database — 犧牲可用性的分散式資料庫
```
在分區容忍的前提下，依然可以得到最新版本的資料，常用於貼文系統、訊息系統等不能犧牲一致性的系統。

例如：Google BigTable、MongoDB、分散式的 RDBMS
```
### CA Database — 犧牲分區容忍性的資料庫
```
這樣就違反了分散式架構的初衷，因此基本上不會存在於分散式系統中。

例如：單機或是經過 Sharding 的資料庫（不能承受單機損壞）。
```

# 常見的 NoSQL 方案分為 4 類：
```
Key/Value store：解決 RDBMS 無法儲存數據結構的問題，以 redis 為代表。
Document DB：解決 RDBMS 強 schema 約束的問題，以 MongoDB 為代表。
Colume-Based DB：解決 RDBMS 大數據場景下的 I/O 問題，以 HBase 為代表。
全文搜尋引擎：解決 RDBMS 的全文搜尋性能問題，以 Elasticsearch 為代表。
```

- NoSQL優點
```
容易擴展的設計架構，由於資料沒有關聯性，因此相較於 RDBMS 更易於 scaling。
簡單快速的達到高可用性（HA）: Hash Function: O(1), Distributed Hash Funciton: O(log n)
容易進行 Schema 的變更，因為根本沒有 Schema，適合時常變更架構的系統。
```

- NoSQL缺點
```
沒有固定的 Schema : 這點其實嚴格來說可以是優點也可以是缺點，得看需求來定論。
關聯式資料庫常見的 Transaction 與 JOIN 也就非常難在 NoSQL 實踐。
資料量不夠大時，其實效能普遍比 RDBMS 來要差
```
# MyISAM vs InnoDB

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

# 分庫分表
# 樂觀鎖 悲觀鎖
# Cache
# Transaction

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
