# mysql 学习笔记

## 下载及安装
配置环境
登录
退出

## 基本语法
```
-- 显示所有数据库
show databases; (mysql里一定要打分号，区别python)

-- 创建数据库
CREATE DATABASE test;

-- 切换
use test;

-- 显示数据库databases中所有的数据表tables
show tables;

-- 创建数据表
mysql> CREATE TABLE pet (
    -> name VARCHAR(20),
    -> owner VARCHAR(20),
    -> species VARCHAR(20),
    -> sex CHAR(1),
    -> birth DATE,
    -> death DATE
    -> );
Query OK, 0 rows affected (0.10 sec)

-- 查看数据表结构
mysql> describe pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.01 sec)

或者

mysql> desc pet;

-- 查询表
mysql> SELECT *from pet;
Empty set (0.02 sec)

-- 插入表
INSERT INTO pet VALUES ('puffball', 'Diane', 'hamster', 'f', '1993-03-30', NULL);

mysql> SELECT *from pet;
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| puffball | Diane | hamster | f    | 1993-03-30 | NULL  |
+----------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)

-- 修改数据
UPDATE pet SET name = 'squirrel' where owner = 'Diane';

mysql> SELECT *from pet;
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| squirrel | Diane | hamster | f    | 1993-03-30 | NULL  |
+----------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)

-- 删除数据
DELETE FROM pet where name = 'squirrel';

mysql> SELECT *from pet;
Empty set (0.00 sec)

-- 删除表
DROP TABLE myorder;
```



# MySQL学习笔记

## 登录和退出MySQL服务器

```shell
# 登录MySQL
$ mysql -u root -p12345612

# 退出MySQL数据库服务器
exit;
```

## 基本语法

```mysql
-- 显示所有数据库
show databases;

-- 创建数据库
CREATE DATABASE test;

-- 切换数据库
use test;

-- 显示数据库中的所有表
show tables;

-- 创建数据表
CREATE TABLE pet (
    name VARCHAR(20),
    owner VARCHAR(20),
    species VARCHAR(20),
    sex CHAR(1),
    birth DATE,
    death DATE
);

-- 查看数据表结构
-- describe pet;
desc pet;

-- 查询表
SELECT * from pet;

-- 插入数据
INSERT INTO pet VALUES ('puffball', 'Diane', 'hamster', 'f', '1990-03-30', NULL);

-- 修改数据
UPDATE pet SET name = 'squirrel' where owner = 'Diane';

-- 删除数据
DELETE FROM pet where name = 'squirrel';

-- 删除表
DROP TABLE myorder;
```

## 建表约束

### 主键约束

```mysql
-- 主键约束
-- 使某个字段不重复且不得为空，确保表内所有数据的唯一性。
CREATE TABLE user (
    id INT PRIMARY KEY,
    name VARCHAR(20)
);
