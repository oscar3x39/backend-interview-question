# MyISAM vs InnoDB

# MariaDB vs. MySQL vs. PostgreSQL

# Database normalization

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