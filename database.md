# DBMS vs RDBMS vs NoSQL

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
```
my.cnf
query_cache_size
max_connection
innodb_buffer_pool_size
innodb_io_capacity
```

# OLTP Database

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
