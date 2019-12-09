# MySQL 学习笔记

## 下载及安装
``` shell
-- 配置环境 
百度

-- 登录 
mysql -uroot -p

-- 退出
exit;
```

## 基本语法
```mysql
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


### 总结

``` mysql
-- 增加
INSERT

-- 删除
DELETE

-- 修改
UPDATE

-- 查询
SELECT
```


## mysql建表约束


### 主键约束
``` mysql
能够唯一确定一张表中的一条记录，也就是通过给某个字段添加约束，就可以是的该字段不重复，且不为空。

创建一个主键约束表
create table user(
id int primary key,
name varchar(20)
);
插入1，张三
insert into user value(1,'张三');

mysql> insert into user value(1,'张三');
Query OK, 1 row affected (0.08 sec)

mysql> insert into user value(1,'张三');
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'


insert into user value(2,'张三')

mysql> select * from user;
+----+------+
| id | name |
+----+------+
|  1 | 张三 |
|  2 | 张三 |
+----+------+

insert into user value(NULL,'张三');
mysql> insert into user value(NULL,'张三');
ERROR 1048 (23000): Column 'id' cannot be null
```
## 联合主键
```mysql
-- 只要联合主键值加起来不重复就可以
create table user2(
id int,
name varchar(20),
password varchar(20),
primary key (id,name)
);


insert into user2 values(1,'张三','123');
insert into user2 values(2,'张三','123');

mysql> insert into user2 values(1,'张三','123');
Query OK, 1 row affected (0.06 sec)

mysql> insert into user2 values(1,'张三','123');
ERROR 1062 (23000): Duplicate entry '1-张三' for key 'PRIMARY'

mysql> insert into user2 values(2,'张三','123');
Query OK, 1 row affected (0.05 sec)

mysql> select * from user2
    -> ;
+----+------+----------+
| id | name | password |
+----+------+----------+
|  1 | 张三 | 123      |
|  2 | 张三 | 123      |
+----+------+----------+
2 rows in set (0.00 sec)


insert into user2 values(NULL,'张三','123');

mysql> insert into user2 values(NULL,'张三','123');
ERROR 1048 (23000): Column 'id' cannot be null
```

### 自增约束
```mysql
create table user3(
id int primary key auto_increment,
name varchar(20)
);

insert into user3 (name) values('Zhangsan');
mysql> insert into user3 (name) values('Zhangsan');
Query OK, 1 row affected (0.06 sec)

mysql> select * from user3;
+----+----------+
| id | name     |
+----+----------+
|  1 | Zhangsan |
+----+----------+
1 row in set (0.00 sec)

mysql> insert into user3 (name) values('Zhangsan');
Query OK, 1 row affected (0.05 sec)

mysql> select * from user3
    -> ;
+----+----------+
| id | name     |
+----+----------+
|  1 | Zhangsan |
|  2 | Zhangsan |
+----+----------+
2 rows in set (0.00 sec)

-- 忘记创建主键约束怎么办
create table user4(
id int,
name varchar(20)
);

-- 修改表结构，添加主键
alter table user4 add primary key(id);
mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.05 sec)

-- 删除？
alter table user4 drop primary key;

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.05 sec)

-- 使用modify修改字段，添加约束
alter table user4 modify id int primary key;
```



### 唯一约束
```mysql
-- 约束修饰的字段的值不可以重复

create table user5(
id int,
name varchar(20)
);

alter table user5 add unique(name);

mysql> alter table user5 add unique(name);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user5
    -> ;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

 







### 非空约束
修饰的字段不能为空 NULL

```mysql
create table user9(
id int,
name varchar(20) not null
);

mysql> create table user9(
    -> id int,
    -> name varchar(20) not null
    -> );
Query OK, 0 rows affected (0.31 sec)

mysql> desc user9
    -> ;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| mane  | varchar(20) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.05 sec)

insert into user9 (id) values (1);

insert into user9 values(1,'Zhangsan');

insert into user9 (name) values('Lisi');
```


### 默认约束
```mysql 
插入字段值的时候，如果没有传值，就会使用默认值

create table user10(
id int,
name varchar(20),
age int default 10
);

insert into user10 (id, name) values (1,'Zhangsan');

insert into user10 values (1,'Zhangsan', 20);

mysql> select * from user10;
+------+----------+------+
| id   | name     | age  |
+------+----------+------+
|    1 | Zhangsan |   10 |
|    1 | Zhangsan |   20 |
+------+----------+------+
2 rows in set (0.00 sec)
```

### 外键约束
涉及到两个表：父表和子表（或主表和副表）
```mysql

-- Classes
create table classes(
id int primary key,
name varchar(20)
);


-- Students
create table students(
id int primary key,
name varchar(20),
class_id int,
foreign key(class_id) references classes(id)
);


insert into classes values(1,'Class 1');
insert into classes values(2,'Class 2');
insert into classes values(3,'Class 3');
insert into classes values(4,'Class 4');

mysql> select * from classes;
+----+---------+
| id | name    |
+----+---------+
|  1 | Class 1 |
|  2 | Class 2 |
|  3 | Class 3 |
|  4 | Class 4 |
+----+---------+
4 rows in set (0.00 sec)

insert into students values(1001,'Zhangsan',1);
insert into students values(1002,'Zhangsan',2);
insert into students values(1003,'Zhangsan',3);
insert into students values(1004,'Zhangsan',4);

mysql> select * from students;
+------+----------+----------+
| id   | name     | class_id |
+------+----------+----------+
| 1001 | Zhangsan |        1 |
| 1002 | Zhangsan |        2 |
| 1003 | Zhangsan |        3 |
| 1004 | Zhangsan |        4 |
+------+----------+----------+
4 rows in set (0.00 sec)

insert into students values(1005,'Lisi',5);

mysql> insert into students values(1005,'Lisi',5);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`test`.`students`, CONSTRAINT `students_ibfk_1` FOREIGN KEY (`class_id`) REFERENCES `classes` (`id`))


-- 1. 主表classes中没有的数据值，在副表中是不可以使用的；
-- 2. 主表中的记录被副表引用，是不可以删除的。

delete from classes where id=4;

mysql> delete from classes where id=4;
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`test`.`students`, CONSTRAINT `students_ibfk_1` FOREIGN KEY (`class_id`) REFERENCES `classes` (`id`))

```


## 数据库的三大设计范式

### 第一范式 1NF

数据表中的所有字段都是不可分割的原子值
```mysql
create table student2(
id int primary key,
name varchar(20),
address varchar(20)
);

insert into student2 values(1,'Zhangsan','中国四川省成都市武侯区一环路南段24号');
insert into student2 values(2,'Lisi','中国四川省成都市武侯区一环路南段24号');
insert into student2 values(3,'Wangwu','中国四川省成都市高新区天府三街2号');

mysql> select * from student2;
+----+----------+--------------------------------------+
| id | name     | address                              |
+----+----------+--------------------------------------+
|  1 | Zhangsan | 中国四川省成都市武侯区一环路南段24号 |
|  2 | Lisi     | 中国四川省成都市武侯区一环路南段24号 |
|  3 | Wangwu   | 中国四川省成都市高新区天府三街2号    |
+----+----------+--------------------------------------+
3 rows in set (0.01 sec)

-- 还可以继续拆分，则不满足第一范式。


create table student3(
id int primary key,
name varchar(20),
country varchar(10),
province varchar(10),
city varchar(10),
details varchar(30)
);

insert into student3 values(1,'Zhangsan','中国','四川省','成都市','武侯区一环路南段24号');
insert into student3 values(2,'Lisi','中国','四川省','成都市','武侯区一环路南段24号');
insert into student3 values(3,'Wangwu','中国','四川省','成都市','高新区天府三街2号');

mysql> select * from student3;
+----+----------+---------+----------+--------+----------------------+
| id | name     | country | province | city   | details              |
+----+----------+---------+----------+--------+----------------------+
|  1 | Zhangsan | 中国    | 四川省   | 成都市 | 武侯区一环路南段24号 |
|  2 | Lisi     | 中国    | 四川省   | 成都市 | 武侯区一环路南段24号 |
|  3 | Wangwu   | 中国    | 四川省   | 成都市 | 高新区天府三街2号    |
+----+----------+---------+----------+--------+----------------------+
3 rows in set (0.00 sec)

-- 范式，设计的越详细，对于某些实际操作可能更好，但不一定都是好处。
```


### 第二范式 2NF

必须满足第一范式的前提下，第二范式要求，除主键外的每一列都必须完全依赖于主键。
如果要出现不完全依赖，只可能发生在联合主键的情况下。

```mysql
-- 订单表

create table myorder(
product_id int,
customer_id int,
product_name varchar(20),
customer_name varchar(20),
primary key(product_id, customer_id)
);

-- 问题？
-- 除主键外的其他列，只依赖于主键的部分字段
-- 拆表

create table myorder(
order_id int primary key,
product_id int,
customer_id int,
);

create table product(
id int primary key,
name varchar(20)
);

create table customer(
id int primary key,
name varchar(20)
);

-- 拆分成三个表则满足第二范式的设计！

```

### 第三范式 3NF

必须先满足第二范式，除开主键列的其他列之间不能有传递依赖关系。

```mysql

create table myorder(
order_id int primary key,
product_id int,
customer_id int,
customer_phone varchar(15)
);

-- 不满足第三范式
-- 拆分

CREATE TABLE myorder (
    order_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT
);

CREATE TABLE customer (
    id INT PRIMARY KEY,
    name VARCHAR(20),
    phone VARCHAR(15)
);


-- 满足第三范式！

```




## mysql查询练习
```mysql
	学生表 Student
	学号 
	姓名
	性别
	出生年月日
	所在班级
        
create table student(
	sno varchar(20) primary key,
	sname varchar(20) not Null,
	ssex varchar(10) not Null,
	sbirthday datetime,
	class varchar(20)
);



	课程表 Course
	课程号
	课程名称
	教师编号


	成绩表 Score
	学号
	课程号
	成绩


	教师表 Teacher
	教师编号
	教室名称
	教师性别
	出生年月日
	职称
	所在部门
```
