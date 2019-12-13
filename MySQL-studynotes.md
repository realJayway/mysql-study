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

### 数据准备

```mysql

-- 学生表 Student
-- 学号 
-- 姓名
-- 性别
-- 出生年月日
-- 所在班级
create table student(
	sno varchar(20) primary key,
	sname varchar(20) not Null,
	ssex varchar(10) not Null,
	sbirthday datetime,
	class varchar(20)
);

-- 教师表 Teacher
-- 教师编号
-- 教室名称
-- 教师性别
-- 出生年月日
-- 职称
-- 所在部门

create table Teacher(
	tno varchar(20) primary key,
	tname varchar(20) not Null,
	tsex varchar(10) not Null,
	tbirthday datetime,
	prof varchar(20) not null,
	depart varchar(20) not null
);
-- 课程表 Course
-- 课程号
-- 课程名称
-- 教师编号

create table Course( 
	cno varchar(20) primary key,
	cname varchar(20) not null,
	tno varchar(20) not null,
	foreign key(tno) references Teacher(tno)
);

-- 成绩表 Score
-- 学号
-- 课程号
-- 成绩

create table score(
	sno varchar(20) not null,
	cno varchar(20) not null,
	degree decimal,
	foreign key(sno) references student(sno),
	foreign key(cno) references course(cno),
	primary key(sno, cno)
);


mysql> show tables;
+-----------------+
| Tables_in_test1 |
+-----------------+
| course          |
| score           |
| student         |
| teacher         |
+-----------------+
4 rows in set (0.00 sec)


-- 添加学生表数据
INSERT INTO student VALUES('101', '曾华', '男', '1977-09-01', '95033');
INSERT INTO student VALUES('102', '匡明', '男', '1975-10-02', '95031');
INSERT INTO student VALUES('103', '王丽', '女', '1976-01-23', '95033');
INSERT INTO student VALUES('104', '李军', '男', '1976-02-20', '95033');
INSERT INTO student VALUES('105', '王芳', '女', '1975-02-10', '95031');
INSERT INTO student VALUES('106', '陆军', '男', '1974-06-03', '95031');
INSERT INTO student VALUES('107', '王尼玛', '男', '1976-02-20', '95033');
INSERT INTO student VALUES('108', '张全蛋', '男', '1975-02-10', '95031');
INSERT INTO student VALUES('109', '赵铁柱', '男', '1974-06-03', '95031');

-- 添加教师表数据
INSERT INTO teacher VALUES('804', '李诚', '男', '1958-12-02', '副教授', '计算机系');
INSERT INTO teacher VALUES('856', '张旭', '男', '1969-03-12', '讲师', '电子工程系');
INSERT INTO teacher VALUES('825', '王萍', '女', '1972-05-05', '助教', '计算机系');
INSERT INTO teacher VALUES('831', '刘冰', '女', '1977-08-14', '助教', '电子工程系');

-- 添加课程表数据
INSERT INTO course VALUES('3-105', '计算机导论', '825');
INSERT INTO course VALUES('3-245', '操作系统', '804');
INSERT INTO course VALUES('6-166', '数字电路', '856');
INSERT INTO course VALUES('9-888', '高等数学', '831');

-- 添加添加成绩表数据
INSERT INTO score VALUES('103', '3-105', '92');
INSERT INTO score VALUES('103', '3-245', '86');
INSERT INTO score VALUES('103', '6-166', '85');
INSERT INTO score VALUES('105', '3-105', '88');
INSERT INTO score VALUES('105', '3-245', '75');
INSERT INTO score VALUES('105', '6-166', '79');
INSERT INTO score VALUES('109', '3-105', '76');
INSERT INTO score VALUES('109', '3-245', '68');
INSERT INTO score VALUES('109', '6-166', '81');

```


### 查询练习1-10


```mysql
-- 1. 查询 student 表的所有行
SELECT * FROM student;

mysql> SELECT * FROM student;
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
9 rows in set (0.00 sec)

-- 2. 查询 student 表中的 name、sex 和 class 字段的所有行
SELECT sname, ssex, class FROM student;

mysql> select sname,ssex,class from student;
+--------+------+-------+
| sname  | ssex | class |
+--------+------+-------+
| 曾华   | 男   | 95033 |
| 匡明   | 男   | 95031 |
| 王丽   | 女   | 95033 |
| 李军   | 男   | 95033 |
| 王芳   | 女   | 95031 |
| 陆军   | 男   | 95031 |
| 王尼玛 | 男   | 95033 |
| 张全蛋 | 男   | 95031 |
| 赵铁柱 | 男   | 95031 |
+--------+------+-------+
9 rows in set (0.00 sec)

-- 3. 查询 teacher 表中不重复的 depart 列 （distinct 排除重复）
SELECT DISTINCT department FROM teacher;

mysql> SELECT DISTINCT depart FROM teacher;
+------------+
| depart     |
+------------+
| 计算机系   |
| 电子工程系 |
+------------+
2 rows in set (0.00 sec)

-- 4. 查询 score 表中成绩在60-80之间的所有行（区间查询和运算符查询）
-- BETWEEN xx AND xx: 查询区间
SELECT * FROM score WHERE degree BETWEEN 60 AND 80;
SELECT * FROM score WHERE degree > 60 AND degree < 80;

mysql> SELECT * FROM score WHERE degree BETWEEN 60 AND 80;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 105 | 3-245 |     75 |
| 105 | 6-166 |     79 |
| 109 | 3-105 |     76 |
| 109 | 3-245 |     68 |
+-----+-------+--------+
4 rows in set (0.05 sec)

-- 5. 查询 score 表中成绩为 85, 86 或 88 的行
-- IN: 查询规定中的多个值
SELECT * FROM score WHERE degree IN (85, 86, 88);

mysql> SELECT * FROM score WHERE degree IN (85, 86, 88);
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-245 |     86 |
| 103 | 6-166 |     85 |
| 105 | 3-105 |     88 |
+-----+-------+--------+
3 rows in set (0.01 sec)

-- 6. 查询 student 表中 '95031' 班或性别为 '女' 的所有行
SELECT * FROM student WHERE class = '95031' or ssex = '女';

mysql> SELECT * FROM student WHERE class = '95031' or ssex = '女';
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
6 rows in set (0.00 sec)

-- 7. 以 class 降序的方式查询 student 表的所有行(DESC-降序；ASC-升序，默认使用ASC)

SELECT * FROM student ORDER BY class DESC;
SELECT * FROM student ORDER BY class ASC;

mysql> SELECT * FROM student ORDER BY class DESC;
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
9 rows in set (0.00 sec)

mysql> SELECT * FROM student ORDER BY class; #默认就是升序
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
+-----+--------+------+---------------------+-------+
9 rows in set (0.00 sec)

-- 8. 以 cno 升序、degree 降序查询 score 表的所有行(按照顺序进行重要度排序)
SELECT * FROM score ORDER BY cno ASC, degree DESC;

mysql> SELECT * FROM score ORDER BY cno ASC, degree DESC;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
| 105 | 3-105 |     88 |
| 109 | 3-105 |     76 |
| 103 | 3-245 |     86 |
| 105 | 3-245 |     75 |
| 109 | 3-245 |     68 |
| 103 | 6-166 |     85 |
| 109 | 6-166 |     81 |
| 105 | 6-166 |     79 |
+-----+-------+--------+
9 rows in set (0.00 sec)

-- 9. 查询 "95031" 班的学生人数 (Count-计数)
SELECT COUNT(*) FROM student WHERE class = '95031';

mysql> SELECT COUNT(*) FROM student WHERE class = '95031';
+----------+
| COUNT(*) |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)

-- 10. 查询 score 表中的最高分的学生学号和课程编号。【子查询或排序查询，degree = (SELECT MAX(degree) FROM score)-子查询，算出最高分】
-- 则先执行 SELECT MAX(degree) FROM score 找到最高分，再执行整个语句
SELECT sno, cno FROM score WHERE degree = (SELECT MAX(degree) FROM score);

mysql> SELECT sno, cno FROM score WHERE degree = (SELECT MAX(degree) FROM score);
+-----+-------+
| sno | cno   |
+-----+-------+
| 103 | 3-105 |
+-----+-------+
1 row in set (0.00 sec)

-- 排序方法查询-LIMIT r, n: 表示从第r行开始，查询n条数据 (存在bug，e.g.两个同为最高)

SELECT sno, cno, degree FROM score ORDER BY degree DESC LIMIT 0, 1; # 降序再取第一个即可


mysql> SELECT sno, cno, degree FROM score ORDER BY degree DESC LIMIT 0, 1;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
+-----+-------+--------+
1 row in set (0.00 sec)
```




### 分组计算平均成绩

```mysql
-- AVG: 平均值
SELECT AVG(degree) FROM score WHERE c_no = '3-105';
SELECT AVG(degree) FROM score WHERE c_no = '3-245';
SELECT AVG(degree) FROM score WHERE c_no = '6-166';

-- GROUP BY: 分组查询
SELECT c_no, AVG(degree) FROM score GROUP BY c_no;



-- 11.查询每门课的平均成绩
select * from course;

-- avg()

select avg(degree) from score where cno = '3-105';

mysql> select avg(degree) from score where cno = '3-105';
+-------------+
| avg(degree) |
+-------------+
|     85.3333 |
+-------------+
1 row in set (0.00 sec)

-- 把所有平均值写在一个
-- group by 分组
SELECT cno,avg(degree) FROM score GROUP BY cno;

mysql> SELECT cno,avg(degree) FROM score GROUP BY cno;
+-------+-------------+
| cno   | avg(degree) |
+-------+-------------+
| 3-105 |     85.3333 |
| 3-245 |     76.3333 |
| 6-166 |     81.6667 |
+-------+-------------+
3 rows in set (0.00 sec)
```

### 分组条件与模糊查询


```mysql
-- 12,查询score表中至少有2名学生选修的,并且以3开头的课程的平均分

SELECT cno FROM score GROUP BY cno
having Count(cno)>=2;

mysql> SELECT cno FROM score GROUP BY cno
    -> having Count(cno)>=2;
+-------+
| cno   |
+-------+
| 3-105 |
| 3-245 |
| 6-166 |
+-------+
3 rows in set (0.05 sec)

SELECT cno FROM score GROUP BY cno
having Count(cno)>=4;

mysql> SELECT cno FROM score GROUP BY cno
    -> having Count(cno)>=4;
Empty set (0.00 sec)

SELECT cno FROM score GROUP BY cno
having Count(cno)>=2 AND cno like '3%';

mysql> SELECT cno FROM score GROUP BY cno
    -> having Count(cno)>=2 AND cno like '3%';
+-------+
| cno   |
+-------+
| 3-105 |
| 3-245 |
+-------+
2 rows in set (0.05 sec)

SELECT cno,avg(degree),Count(*) FROM score GROUP BY cno
having Count(cno)>=2 AND cno like '3%';

mysql> SELECT cno,avg(degree),Count(*) FROM score GROUP BY cno
    -> having Count(cno)>=2 AND cno like '3%';
+-------+-------------+----------+
| cno   | avg(degree) | Count(*) |
+-------+-------------+----------+
| 3-105 |     85.3333 |        3 |
| 3-245 |     76.3333 |        3 |
+-------+-------------+----------+
2 rows in set (0.05 sec)

```

### 查询范围的两种方式

```mysql
-- 13.查询分数大于70但是小于90的s_no列:

SELECT sno,degree from score
WHERE degree>70 and degree<90;

mysql> SELECT sno,degree from score
    -> WHERE degree>70 and degree<90;
+-----+--------+
| sno | degree |
+-----+--------+
| 103 |     86 |
| 103 |     85 |
| 105 |     88 |
| 105 |     75 |
| 105 |     79 |
| 109 |     76 |
| 109 |     81 |
+-----+--------+
7 rows in set (0.05 sec)
```
或者
```mysql
SELECT sno,degree from score
WHERE degree BETWEEN 70 and 90;

mysql> SELECT sno,degree from score
    -> WHERE degree BETWEEN 70 and 90;
+-----+--------+
| sno | degree |
+-----+--------+
| 103 |     86 |
| 103 |     85 |
| 105 |     88 |
| 105 |     75 |
| 105 |     79 |
| 109 |     76 |
| 109 |     81 |
+-----+--------+
7 rows in set (0.00 sec)
```
### 多表查询
```mysql
-- 14.查询所有的学生 s_name , c_no, sc_degree列
SELECT sno,sname from student;

mysql> SELECT sno,sname from student;
+-----+--------+
| sno | sname  |
+-----+--------+
| 101 | 曾华   |
| 102 | 匡明   |
| 103 | 王丽   |
| 104 | 李军   |
| 105 | 王芳   |
| 106 | 陆军   |
| 107 | 王尼玛 |
| 108 | 张全蛋 |
| 109 | 赵铁柱 |
+-----+--------+
9 rows in set (0.00 sec)

SELECT sno,cno,degree FROM score;

mysql> SELECT sno,cno,degree FROM score;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
| 103 | 3-245 |     86 |
| 103 | 6-166 |     85 |
| 105 | 3-105 |     88 |
| 105 | 3-245 |     75 |
| 105 | 6-166 |     79 |
| 109 | 3-105 |     76 |
| 109 | 3-245 |     68 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
9 rows in set (0.00 sec)

SELECT sname,cno,degree FROM student,score
WHERE student.sno = score.sno;

mysql> SELECT sname,cno,degree FROM student,score
    -> WHERE student.sno = score.sno;
+--------+-------+--------+
| sname  | cno   | degree |
+--------+-------+--------+
| 王丽   | 3-105 |     92 |
| 王丽   | 3-245 |     86 |
| 王丽   | 6-166 |     85 |
| 王芳   | 3-105 |     88 |
| 王芳   | 3-245 |     75 |
| 王芳   | 6-166 |     79 |
| 赵铁柱 | 3-105 |     76 |
| 赵铁柱 | 3-245 |     68 |
| 赵铁柱 | 6-166 |     81 |
+--------+-------+--------+
9 rows in set (0.00 sec)





--  15.查询所有学生的sno, cname, degree列
--  分析：cname在course中，sno,degree在score中，注意到两个表有共同字段cno

SELECT cno,cname FROM course;

mysql> SELECT cno,cname FROM course;
+-------+------------+
| cno   | cname      |
+-------+------------+
| 3-105 | 计算机导论 |
| 3-245 | 操作系统   |
| 6-166 | 数字电路   |
| 9-888 | 高等数学   |
+-------+------------+
4 rows in set (0.07 sec)

SELECT cno,sno,degree FROM score;

mysql> SELECT cno,sno,degree FROM score;
+-------+-----+--------+
| cno   | sno | degree |
+-------+-----+--------+
| 3-105 | 103 |     92 |
| 3-245 | 103 |     86 |
| 6-166 | 103 |     85 |
| 3-105 | 105 |     88 |
| 3-245 | 105 |     75 |
| 6-166 | 105 |     79 |
| 3-105 | 109 |     76 |
| 3-245 | 109 |     68 |
| 6-166 | 109 |     81 |
+-------+-----+--------+
9 rows in set (0.00 sec)

SELECT sno,cname,degree FROM course,score
WHERE course.cno = score.cno;

mysql> SELECT sno,cname,degree FROM course,score
    -> WHERE course.cno = score.cno;
+-----+------------+--------+
| sno | cname      | degree |
+-----+------------+--------+
| 103 | 计算机导论 |     92 |
| 105 | 计算机导论 |     88 |
| 109 | 计算机导论 |     76 |
| 103 | 操作系统   |     86 |
| 105 | 操作系统   |     75 |
| 109 | 操作系统   |     68 |
| 103 | 数字电路   |     85 |
| 105 | 数字电路   |     79 |
| 109 | 数字电路   |     81 |
+-----+------------+--------+
9 rows in set (0.00 sec)
```

### 三表关联查询

```mysql
-- 16.查询所有的学生 sname , cname, degree列
-- sname -> student
-- cname -> course
-- degree -> score

SELECT sname,cname,degree FROM student,course,score
WHERE student.sno = score.sno
and course.cno = score.cno;

mysql> SELECT sname,cname,degree FROM student,course,score
    -> WHERE student.sno = score.sno
    -> and course.cno = score.cno;
+--------+------------+--------+
| sname  | cname      | degree |
+--------+------------+--------+
| 王丽   | 计算机导论 |     92 |
| 王丽   | 操作系统   |     86 |
| 王丽   | 数字电路   |     85 |
| 王芳   | 计算机导论 |     88 |
| 王芳   | 操作系统   |     75 |
| 王芳   | 数字电路   |     79 |
| 赵铁柱 | 计算机导论 |     76 |
| 赵铁柱 | 操作系统   |     68 |
| 赵铁柱 | 数字电路   |     81 |
+--------+------------+--------+
9 rows in set (0.00 sec)

-- 为了看得清楚一些，我们可以:
SELECT sname,cname,degree,student.sno as stu_sno,score.sno,course.cno as cou_cno,score.cno FROM student,course,score
WHERE student.sno = score.sno
and course.cno = score.cno;


mysql> SELECT sname,cname,degree,student.sno as stu_sno,score.sno,course.cno as cou_cno,score.cno FROM student,course,score
    -> WHERE student.sno = score.sno
    -> and course.cno = score.cno;
+--------+------------+--------+---------+-----+---------+-------+
| sname  | cname      | degree | stu_sno | sno | cou_cno | cno   |
+--------+------------+--------+---------+-----+---------+-------+
| 王丽   | 计算机导论 |     92 | 103     | 103 | 3-105   | 3-105 |
| 王丽   | 操作系统   |     86 | 103     | 103 | 3-245   | 3-245 |
| 王丽   | 数字电路   |     85 | 103     | 103 | 6-166   | 6-166 |
| 王芳   | 计算机导论 |     88 | 105     | 105 | 3-105   | 3-105 |
| 王芳   | 操作系统   |     75 | 105     | 105 | 3-245   | 3-245 |
| 王芳   | 数字电路   |     79 | 105     | 105 | 6-166   | 6-166 |
| 赵铁柱 | 计算机导论 |     76 | 109     | 109 | 3-105   | 3-105 |
| 赵铁柱 | 操作系统   |     68 | 109     | 109 | 3-245   | 3-245 |
| 赵铁柱 | 数字电路   |     81 | 109     | 109 | 6-166   | 6-166 |
+--------+------------+--------+---------+-----+---------+-------+
9 rows in set (0.00 sec)
```
Note: 就是以两个表的值相等作为对应联系。

### 子查询加分组求平均

```mysql
-- 17.查询班级是'95031'班学生每门课的平均分
SELECT * FROM student WHERE class='95031';

mysql> SELECT * FROM student WHERE class='95031';
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
5 rows in set (0.00 sec)

SELECT sno FROM student WHERE class='95031';

mysql> SELECT sno FROM student WHERE class='95031';
+-----+
| sno |
+-----+
| 102 |
| 105 |
| 106 |
| 108 |
| 109 |
+-----+
5 rows in set (0.00 sec)

SELECT * FROM score WHERE sno in (SELECT sno FROM student WHERE class='95031');

mysql> SELECT * FROM score WHERE sno in (SELECT sno FROM student WHERE class='95031');
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 105 | 3-105 |     88 |
| 105 | 3-245 |     75 |
| 105 | 6-166 |     79 |
| 109 | 3-105 |     76 |
| 109 | 3-245 |     68 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
6 rows in set (0.04 sec)

SELECT cno,avg(degree)
FROM score
WHERE sno in (SELECT sno FROM student WHERE class='95031')
GROUP BY cno;

mysql> SELECT cno,avg(degree)
    -> FROM score
    -> WHERE sno in (SELECT sno FROM student WHERE class='95031')
    -> GROUP BY cno;
+-------+-------------+
| cno   | avg(degree) |
+-------+-------------+
| 3-105 |     82.0000 |
| 3-245 |     71.5000 |
| 6-166 |     80.0000 |
+-------+-------------+
3 rows in set (0.05 sec)
```

### 子查询

```mysql
-- 18.查询选修"3-105"课程的成绩高于'109'号同学'3-105'成绩 的所有同学的记录
SELECT degree FROM score WHERE sno='109' and cno='3-105';
mysql> SELECT degree FROM score WHERE sno='109' and cno='3-105';
+--------+
| degree |
+--------+
|     76 |
+--------+
1 row in set (0.00 sec)

SELECT * FROM score WHERE cno='3-105' and degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105');

mysql> SELECT * FROM score WHERE cno='3-105' and degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105');
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
| 105 | 3-105 |     88 |
+-----+-------+--------+
2 rows in set (0.00 sec)



-- 19.查询成绩高于学号为'109',课程号为'3-105'的成绩的所有记录
SELECT * FROM score WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105');

mysql> SELECT * FROM score WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105');
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
| 103 | 3-245 |     86 |
| 103 | 6-166 |     85 |
| 105 | 3-105 |     88 |
| 105 | 6-166 |     79 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
6 rows in set (0.00 sec)

-- 如果我们想在表中添加学生的信息从而制作成一个完整的表
-- 1. 先找到所有需要的信息
SELECT * FROM score,student WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105') AND student.sno = score.sno;

mysql> SELECT * FROM score,student WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105') AND student.sno = score.sno;
+-----+-------+--------+-----+--------+------+---------------------+-------+
| sno | cno   | degree | sno | sname  | ssex | sbirthday           | class |
+-----+-------+--------+-----+--------+------+---------------------+-------+
| 103 | 3-105 |     92 | 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 103 | 3-245 |     86 | 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 103 | 6-166 |     85 | 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 105 | 3-105 |     88 | 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 105 | 6-166 |     79 | 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 109 | 6-166 |     81 | 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+-------+--------+-----+--------+------+---------------------+-------+
6 rows in set (0.00 sec)

-- 2. 再选取我们需要的信息以及修改顶部标题即可
SELECT student.sno AS '学号', sname as '姓名', ssex AS '性别', cno AS '课程号', class AS '班级编号', degree AS '成绩'
FROM score,student WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105') AND student.sno = score.sno;

mysql> SELECT student.sno AS '学号', sname as '姓名', ssex AS '性别', cno AS '课程号', class AS '班级编号', degree AS ' 成绩'
    -> FROM score,student WHERE degree>(SELECT degree FROM score WHERE sno='109' and cno='3-105') AND student.sno = score.sno;
+------+--------+------+--------+----------+------+
| 学号 | 姓名   | 性别 | 课程号 | 班级编号 | 成绩 |
+------+--------+------+--------+----------+------+
| 103  | 王丽   | 女   | 3-105  | 95033    |   92 |
| 103  | 王丽   | 女   | 3-245  | 95033    |   86 |
| 103  | 王丽   | 女   | 6-166  | 95033    |   85 |
| 105  | 王芳   | 女   | 3-105  | 95031    |   88 |
| 105  | 王芳   | 女   | 6-166  | 95031    |   79 |
| 109  | 赵铁柱 | 男   | 6-166  | 95031    |   81 |
+------+--------+------+--------+----------+------+
6 rows in set (0.00 sec)

```

### year函数与带in关键字的子查询

```mysql
--  20.查询所有学号为108.101的同学同年出生的所有学生的sno,sname和sbirthday
SELECT * FROM student WHERE sno in (108,101);

mysql> SELECT * FROM student WHERE sno in (108,101);
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
2 rows in set (0.05 sec)

SELECT year(sbirthday) FROM student WHERE sno in (108,101);

mysql> SELECT year(sbirthday) FROM student WHERE sno in (108,101);
+-----------------+
| year(sbirthday) |
+-----------------+
|            1977 |
|            1975 |
+-----------------+
2 rows in set (0.00 sec)



SELECT * from student 
WHERE year(sbirthday) in (SELECT year(sbirthday) FROM student WHERE sno in (108,101));

mysql> SELECT * from student
    -> WHERE year(sbirthday) in (SELECT year(sbirthday) FROM student WHERE sno in (108,101));
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
4 rows in set (0.00 sec)
```


### 多层嵌套的子查询

```mysql
-- 21.查询 张旭 教师任课的学生的成绩
SELECT * from teacher WHERE tname = '张旭';
+-----+-------+------+---------------------+------+------------+
| tno | tname | tsex | tbirthday           | prof | depart     |
+-----+-------+------+---------------------+------+------------+
| 856 | 张旭  | 男   | 1969-03-12 00:00:00 | 讲师 | 电子工程系 |
+-----+-------+------+---------------------+------+------------+
1 row in set (0.07 sec)

SELECT * FROM course WHERE tno = (SELECT tno from teacher WHERE tname = '张旭');
+-------+----------+-----+
| cno   | cname    | tno |
+-------+----------+-----+
| 6-166 | 数字电路 | 856 |
+-------+----------+-----+
1 row in set (0.05 sec)

SELECT * FROM score WHERE cno = (SELECT cno FROM course WHERE tno = (SELECT tno from teacher WHERE tname = '张旭'));

mysql> SELECT * FROM score WHERE cno = (SELECT cno FROM course WHERE tno = (SELECT tno from teacher WHERE tname = '张旭'));
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 6-166 |     85 |
| 105 | 6-166 |     79 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
3 rows in set (0.00 sec)

-- 嵌套了两次

-- 想显示出学生的名字和性别从而完善表
SELECT student.sno, sname, ssex, cno, degree FROM score,student 
WHERE cno = (SELECT cno FROM course WHERE tno = (SELECT tno from teacher WHERE tname = '张旭')) 
and student.sno = score.sno;
+-----+--------+------+-------+--------+
| sno | sname  | ssex | cno   | degree |
+-----+--------+------+-------+--------+
| 103 | 王丽   | 女   | 6-166 |     85 |
| 105 | 王芳   | 女   | 6-166 |     79 |
| 109 | 赵铁柱 | 男   | 6-166 |     81 |
+-----+--------+------+-------+--------+
3 rows in set (0.00 sec)

```



### 多表查询

```mysql
-- 22.查询选修课程的同学人数多余 5 人的教师姓名
SELECT * FROM score;
-- 发现数据不够，此时再添加数据
INSERT INTO score VALUES('101','3-105','90');
INSERT INTO score VALUES('102','3-105','91');
INSERT INTO score VALUES('104','3-105','89');

mysql> SELECT * FROM score;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 103 | 3-105 |     92 |
| 103 | 3-245 |     86 |
| 103 | 6-166 |     85 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
| 105 | 3-245 |     75 |
| 105 | 6-166 |     79 |
| 109 | 3-105 |     76 |
| 109 | 3-245 |     68 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
12 rows in set (0.00 sec)
-- 此时的数据数量才够。

SELECT cno FROM score group BY cno having Count(*)>5;
+-------+
| cno   |
+-------+
| 3-105 |
+-------+
1 row in set (0.00 sec)

SELECT tno from course WHERE cno in (SELECT cno FROM score group BY cno having Count(*)>5);
+-----+
| tno |
+-----+
| 825 |
+-----+
1 row in set (0.00 sec)

-- 分层查询即可，别嫌麻烦。
SELECT tname FROM teacher WHERE tno in (SELECT tno from course WHERE cno in (SELECT cno FROM score group BY cno having Count(*)>5));

mysql> SELECT tname FROM teacher WHERE tno in (SELECT tno from course WHERE cno in (SELECT cno FROM score group BY cno having Count(*)>5));
+-------+
| tname |
+-------+
| 王萍  |
+-------+
1 row in set (0.05 sec)
```

Note:视频中用 '=' 是不严谨的,实际中你根本不知道有多少条件是符合的,要用IN



### in表示或者关系

```mysql
-- 23.查询95033班和95031班全体学生的记录

-- 由于视频中就只有这两个班,所以要插入数据:
INSERT INTO student VALUES('110','张飞','男','1974-06-03','95038');
mysql> select * from student;
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
| 110 | 张飞   | 男   | 1974-06-03 00:00:00 | 95038 |
+-----+--------+------+---------------------+-------+
10 rows in set (0.00 sec)

-- 再查询
select * FROM student where class in ('95033','95031');
mysql> select * FROM student where class in ('95033','95031');
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
9 rows in set (0.00 sec)
```

### where条件查询

```mysql
-- 24.查询存在85分以上成绩的课程cno

SELECT * FROM score where degree>85;
mysql> SELECT * FROM score where degree>85;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 103 | 3-105 |     92 |
| 103 | 3-245 |     86 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
+-----+-------+--------+
6 rows in set (0.00 sec)

-- 补充名字等信息
SELECT student.sno, sname, ssex, course.cno, cname, degree FROM score,student,course
WHERE degree>85 AND student.sno = score.sno AND course.cno = score.cno;

mysql> SELECT student.sno, sname, ssex, course.cno, cname, degree FROM score,student,course
    -> WHERE degree>85 AND student.sno = score.sno AND course.cno = score.cno;
+-----+-------+------+-------+------------+--------+
| sno | sname | ssex | cno   | cname      | degree |
+-----+-------+------+-------+------------+--------+
| 101 | 曾华  | 男   | 3-105 | 计算机导论 |     90 |
| 102 | 匡明  | 男   | 3-105 | 计算机导论 |     91 |
| 103 | 王丽  | 女   | 3-105 | 计算机导论 |     92 |
| 103 | 王丽  | 女   | 3-245 | 操作系统   |     86 |
| 104 | 李军  | 男   | 3-105 | 计算机导论 |     89 |
| 105 | 王芳  | 女   | 3-105 | 计算机导论 |     88 |
+-----+-------+------+-------+------------+--------+
6 rows in set (0.00 sec)



-- 25.查出所有'计算机系' 教师所教课程的成绩表
SELECT * FROM score WHERE cno IN (SELECT cno FROM course WHERE tno IN (SELECT tno FROM teacher WHERE depart = '计算机系'));

mysql> SELECT * FROM score WHERE cno IN (SELECT cno FROM course WHERE tno IN (SELECT tno FROM teacher WHERE depart = '计算机系'));
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-245 |     86 |
| 105 | 3-245 |     75 |
| 109 | 3-245 |     68 |
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 103 | 3-105 |     92 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
| 109 | 3-105 |     76 |
+-----+-------+--------+
9 rows in set (0.00 sec)
```



### union和notin

```mysql
-- 26.查询'计算机系'与'电子工程系' 不同职称的教师的name和prof
SELECT * FROM teacher WHERE depart = '计算机系' AND prof NOT IN (SELECT prof FROM teacher WHERE depart = '电子工程系')
UNION
SELECT * FROM teacher WHERE depart = '电子工程系' AND prof NOT IN (SELECT prof FROM teacher WHERE depart = '计算机系');

mysql> SELECT * FROM teacher WHERE depart = '计算机系' AND prof NOT IN (SELECT prof FROM teacher WHERE depart = '电子工 程系')
    -> UNION
    -> SELECT * FROM teacher WHERE depart = '电子工程系' AND prof NOT IN (SELECT prof FROM teacher WHERE depart = '计算 机系');
+-----+-------+------+---------------------+--------+------------+
| tno | tname | tsex | tbirthday           | prof   | depart     |
+-----+-------+------+---------------------+--------+------------+
| 804 | 李诚  | 男   | 1958-12-02 00:00:00 | 副教授 | 计算机系   |
| 856 | 张旭  | 男   | 1969-03-12 00:00:00 | 讲师   | 电子工程系 |
+-----+-------+------+---------------------+--------+------------+
2 rows in set (0.05 sec)
```




### ANY 和 ALL

```mysql
-- 27, 查询选修编号为"3-105"课程且成绩至少高于选修编号为'3-245'同学的cno,sno和degree,并且按照degree从高到地次序排序
SELECT * FROM score WHERE cno='3-105';
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 103 | 3-105 |     92 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
| 109 | 3-105 |     76 |
+-----+-------+--------+
6 rows in set (0.00 sec)

SELECT * FROM score WHERE cno='3-245';
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-245 |     86 |
| 105 | 3-245 |     75 |
| 109 | 3-245 |     68 |
+-----+-------+--------+
3 rows in set (0.00 sec)

-- 至少则大于任意一个即可
SELECT * FROM score WHERE cno='3-105'
AND degree>any(SELECT degree FROM score WHERE cno='3-245')
ORDER BY degree DESC;

mysql> SELECT * FROM score WHERE cno='3-105'
    -> AND degree>any(SELECT degree FROM score WHERE cno='3-245')
    -> ORDER BY degree DESC;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
| 102 | 3-105 |     91 |
| 101 | 3-105 |     90 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
| 109 | 3-105 |     76 |
+-----+-------+--------+
6 rows in set (0.00 sec)



-- 28.查询选修编号为"3-105"且成绩高于选修编号为"3-245"课程的同学cno.sno和scdegree
SELECT * FROM score WHERE degree > ALL (select degree from score WHERE cno = '3-245') AND cno = '3-105';
mysql> SELECT * FROM score WHERE degree > ALL (select degree from score WHERE cno = '3-245') AND cno = '3-105';
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 103 | 3-105 |     92 |
| 104 | 3-105 |     89 |
| 105 | 3-105 |     88 |
+-----+-------+--------+
5 rows in set (0.00 sec)
```




### 更改标题与合并

```mysql
-- 29. 查询所有教师和同学的 name ,sex, birthday
SELECT sname AS name, ssex AS sex, sbirthday AS birthday  FROM student
UNION
SELECT tname AS name, tsex AS sex, tbirthday AS birthday FROM teacher;

mysql> SELECT sname AS name, ssex AS sex, sbirthday AS birthday  FROM student
    -> UNION
    -> SELECT tname AS name, tsex AS sex, tbirthday AS birthday FROM teacher;
+--------+-----+---------------------+
| name   | sex | birthday            |
+--------+-----+---------------------+
| 曾华   | 男  | 1977-09-01 00:00:00 |
| 匡明   | 男  | 1975-10-02 00:00:00 |
| 王丽   | 女  | 1976-01-23 00:00:00 |
| 李军   | 男  | 1976-02-20 00:00:00 |
| 王芳   | 女  | 1975-02-10 00:00:00 |
| 陆军   | 男  | 1974-06-03 00:00:00 |
| 王尼玛 | 男  | 1976-02-20 00:00:00 |
| 张全蛋 | 男  | 1975-02-10 00:00:00 |
| 赵铁柱 | 男  | 1974-06-03 00:00:00 |
| 张飞   | 男  | 1974-06-03 00:00:00 |
| 李诚   | 男  | 1958-12-02 00:00:00 |
| 王萍   | 女  | 1972-05-05 00:00:00 |
| 刘冰   | 女  | 1977-08-14 00:00:00 |
| 张旭   | 男  | 1969-03-12 00:00:00 |
+--------+-----+---------------------+
14 rows in set (0.00 sec)

-- 第二排不需要取别名，会按照顺序合并
SELECT sname AS name, ssex AS sex, sbirthday AS birthday  FROM student
UNION
SELECT tname, tsex, tbirthday FROM teacher;
+--------+-----+---------------------+
| name   | sex | birthday            |
+--------+-----+---------------------+
| 曾华   | 男  | 1977-09-01 00:00:00 |
| 匡明   | 男  | 1975-10-02 00:00:00 |
| 王丽   | 女  | 1976-01-23 00:00:00 |
| 李军   | 男  | 1976-02-20 00:00:00 |
| 王芳   | 女  | 1975-02-10 00:00:00 |
| 陆军   | 男  | 1974-06-03 00:00:00 |
| 王尼玛 | 男  | 1976-02-20 00:00:00 |
| 张全蛋 | 男  | 1975-02-10 00:00:00 |
| 赵铁柱 | 男  | 1974-06-03 00:00:00 |
| 张飞   | 男  | 1974-06-03 00:00:00 |
| 李诚   | 男  | 1958-12-02 00:00:00 |
| 王萍   | 女  | 1972-05-05 00:00:00 |
| 刘冰   | 女  | 1977-08-14 00:00:00 |
| 张旭   | 男  | 1969-03-12 00:00:00 |
+--------+-----+---------------------+
14 rows in set (0.00 sec)


-- 30.查询所有'女'教师和'女'学生的name,sex,birthday
SELECT sname AS name, ssex AS sex, sbirthday AS birthday  FROM student WHERE ssex = '女'
UNION
SELECT tname AS name, tsex AS sex, tbirthday AS birthday FROM teacher WHERE tsex = '女';

mysql> SELECT sname AS name, ssex AS sex, sbirthday AS birthday  FROM student WHERE ssex = '女'
    -> UNION
    -> SELECT tname AS name, tsex AS sex, tbirthday AS birthday FROM teacher WHERE tsex = '女';
+------+-----+---------------------+
| name | sex | birthday            |
+------+-----+---------------------+
| 王丽 | 女  | 1976-01-23 00:00:00 |
| 王芳 | 女  | 1975-02-10 00:00:00 |
| 王萍 | 女  | 1972-05-05 00:00:00 |
| 刘冰 | 女  | 1977-08-14 00:00:00 |
+------+-----+---------------------+
4 rows in set (0.00 sec)
```



### 复制表数据做条件查询

```mysql
-- 31.查询成绩比该课程平均成绩低的同学的成绩表
SELECT cno,avg(degree) FROM score GROUP BY cno;
+-------+-------------+
| cno   | avg(degree) |
+-------+-------------+
| 3-105 |     87.6667 |
| 3-245 |     76.3333 |
| 6-166 |     81.6667 |
+-------+-------------+
3 rows in set (0.00 sec)

SELECT * FROM score;
+-----+-------+--------+		
| sno | cno   | degree |		
+-----+-------+--------+		
| 101 | 3-105 |     90 |		
| 102 | 3-105 |     91 |		
| 103 | 3-105 |     92 |		
| 103 | 3-245 |     86 |		
| 103 | 6-166 |     85 |		
| 104 | 3-105 |     89 |		
| 105 | 3-105 |     88 |		
| 105 | 3-245 |     75 |		
| 105 | 6-166 |     79 |		
| 109 | 3-105 |     76 |		
| 109 | 3-245 |     68 |		
| 109 | 6-166 |     81 |		
+-----+-------+--------+		
12 rows in set (0.00 sec)		

SELECT * FROM score a WHERE degree < (SELECT avg(degree) from score b where a.cno = b.cno);

mysql> SELECT * FROM score a WHERE degree < (SELECT avg(degree) from score b where a.cno = b.cno);
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 105 | 3-245 |     75 |
| 105 | 6-166 |     79 |
| 109 | 3-105 |     76 |
| 109 | 3-245 |     68 |
| 109 | 6-166 |     81 |
+-----+-------+--------+
5 rows in set (0.00 sec)
```
Note: 把表分成了两个一样的 为 表a 和表b




### where in

```mysql
-- 32.查询所有任课教师的tname 和 depart(要在分数表中可以查得到)
SELECT * FROM teacher WHERE tno IN(SELECT tno FROM course);

mysql> SELECT * FROM teacher WHERE tno IN(SELECT tno FROM course);
+-----+-------+------+---------------------+--------+------------+
| tno | tname | tsex | tbirthday           | prof   | depart     |
+-----+-------+------+---------------------+--------+------------+
| 804 | 李诚  | 男   | 1958-12-02 00:00:00 | 副教授 | 计算机系   |
| 825 | 王萍  | 女   | 1972-05-05 00:00:00 | 助教   | 计算机系   |
| 831 | 刘冰  | 女   | 1977-08-14 00:00:00 | 助教   | 电子工程系 |
| 856 | 张旭  | 男   | 1969-03-12 00:00:00 | 讲师   | 电子工程系 |
+-----+-------+------+---------------------+--------+------------+
4 rows in set (0.00 sec)
```




### 条件分组筛选
 
 ```mysql
-- 33.查出至少有2名男生的班号
SELECT class FROM student WHERE ssex = '男' GROUP BY class HAVING COUNT(sno) > 1; 

mysql> SELECT class FROM student WHERE ssex = '男' GROUP BY class HAVING COUNT(sno) > 1;
+-------+
| class |
+-------+
| 95033 |
| 95031 |
+-------+
2 rows in set (0.00 sec)
```






### notlike 模糊查询取反

```mysql
-- 34.查询student 表中 不姓"王"的同学的记录
SELECT * FROM student WHERE sname NOT LIKE '王%'; 

mysql> SELECT * FROM student WHERE sname NOT LIKE '王%';
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
| 110 | 张飞   | 男   | 1974-06-03 00:00:00 | 95038 |
+-----+--------+------+---------------------+-------+
7 rows in set (0.00 sec)
```



### year函数和 now函数

```mysql
-- 35. 查询student 中每个学生的姓名和年龄(当前时间 - 出生年份)
SELECT YEAR(NOW());
+-------------+
| YEAR(NOW()) |
+-------------+
|        2019 |
+-------------+
1 row in set (0.03 sec)

SELECT sname, YEAR(NOW()) - YEAR(sbirthday) AS age FROM student;

mysql> SELECT sname, YEAR(NOW()) - YEAR(sbirthday) AS age FROM student;
+--------+------+
| sname  | age  |
+--------+------+
| 曾华   |   42 |
| 匡明   |   44 |
| 王丽   |   43 |
| 李军   |   43 |
| 王芳   |   44 |
| 陆军   |   45 |
| 王尼玛 |   43 |
| 张全蛋 |   44 |
| 赵铁柱 |   45 |
| 张飞   |   45 |
+--------+------+
10 rows in set (0.00 sec)
```





### max 和 min

```mysql
-- 36. 查询student中最大和最小的 sbirthday的值
SELECT MAX(sbirthday) as '最大', MIN(sbirthday) as '最小' FROM student;

mysql> SELECT MAX(sbirthday) as '最大', MIN(sbirthday) as '最小' FROM student;
+---------------------+---------------------+
| 最大                | 最小                |
+---------------------+---------------------+
| 1977-09-01 00:00:00 | 1974-06-03 00:00:00 |
+---------------------+---------------------+
1 row in set (0.00 sec)
```
note： 最大最小仅指数值上的大小。


### 多字段查询

```mysql
-- 37.以班级号和年龄从大到小的顺序查询student表中的全部记录
SELECt * FROM student ORDER BY class DESC, sbirthday; 
mysql> SELECt * FROM student ORDER BY class DESC, sbirthday;
+-----+--------+------+---------------------+-------+
| sno | sname  | ssex | sbirthday           | class |
+-----+--------+------+---------------------+-------+
| 110 | 张飞   | 男   | 1974-06-03 00:00:00 | 95038 |
| 103 | 王丽   | 女   | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男   | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男   | 1976-02-20 00:00:00 | 95033 |
| 101 | 曾华   | 男   | 1977-09-01 00:00:00 | 95033 |
| 106 | 陆军   | 男   | 1974-06-03 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男   | 1974-06-03 00:00:00 | 95031 |
| 105 | 王芳   | 女   | 1975-02-10 00:00:00 | 95031 |
| 108 | 张全蛋 | 男   | 1975-02-10 00:00:00 | 95031 |
| 102 | 匡明   | 男   | 1975-10-02 00:00:00 | 95031 |
+-----+--------+------+---------------------+-------+
10 rows in set (0.00 sec)




-- 38.查询"男"教师 及其所上的课
SELECT * FROM course WHERE tno IN (SELECT tno FROM teacher WHERE tsex = '男');
+-------+----------+-----+
| cno   | cname    | tno |
+-------+----------+-----+
| 3-245 | 操作系统 | 804 |
| 6-166 | 数字电路 | 856 |
+-------+----------+-----+
2 rows in set (0.00 sec)




-- 39.查询最高分同学的sno cno 和 degree;
SELECT * FROM score WHERE degree = (select MAX(degree) AS degree FROM score);
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-105 |     92 |
+-----+-------+--------+
1 row in set (0.00 sec)



-- 40. 查询和"李军"同性别的所有同学的sname
SELECT sname, ssex FROM student WHERE ssex = (SELECT ssex FROM student WHERE sname = '李军');
+--------+------+
| sname  | ssex |
+--------+------+
| 曾华   | 男   |
| 匡明   | 男   |
| 李军   | 男   |
| 陆军   | 男   |
| 王尼玛 | 男   |
| 张全蛋 | 男   |
| 赵铁柱 | 男   |
| 张飞   | 男   |
+--------+------+
8 rows in set (0.00 sec)





-- 41.查询和"李军"同性别并且同班的所有同学的sname
SELECT sname, ssex FROM student WHERE ssex = (SELECT ssex FROM student WHERE sname = '李军') 
AND class = (SELECT class FROM student WHERE sname = '李军');
+--------+------+
| sname  | ssex |
+--------+------+
| 曾华   | 男   |
| 李军   | 男   |
| 王尼玛 | 男   |
+--------+------+
3 rows in set (0.00 sec)

SELECT s_name, s_sex FROM student s1 WHERE s_sex = (SELECT s_sex FROM student s2 WHERE s_name = '李军' AND s1.s_class = s2.s_class);
+--------+-------+
| s_name | s_sex |
+--------+-------+
| 曾华   | 男    |
| 李军   | 男    |
| 王尼玛 | 男    |
+--------+-------+





-- 42. 查询所有选修'计算机导论'课程的'男'同学的成绩表
SELECT * FROM score WHERE cno = (SELECT cno FROM course WHERE cname = '计算机导论' ) 
AND sno IN (SELECT sno FROM student WHERE ssex = '男');
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     90 |
| 102 | 3-105 |     91 |
| 104 | 3-105 |     89 |
| 109 | 3-105 |     76 |
+-----+-------+--------+
4 rows in set (0.00 sec)
```








### 按等级查询

```mysql
-- 43. 假设使用了以下命令建立了一个grade表
CREATE TABLE grade(
    low INT(3),
    upp INT(3),
    grade CHAR(1)
);
INSERT INTO grade VALUES(90,100,'A');
INSERT INTO grade VALUES(80,89,'B');
INSERT INTO grade VALUES(70,79,'c');
INSERT INTO grade VALUES(60,69,'D');
INSERT INTO grade VALUES(0,59,'E');


-- 查询所有同学的sno , cno 和grade列
SELECT sno,cno,grade from score,grade WHERE degree BETWEEN low AND upp;

mysql> SELECT sno,cno,grade from score,grade WHERE degree BETWEEN low AND upp;
+-----+-------+-------+
| sno | cno   | grade |
+-----+-------+-------+
| 101 | 3-105 | A     |
| 102 | 3-105 | A     |
| 103 | 3-105 | A     |
| 103 | 3-245 | B     |
| 103 | 6-166 | B     |
| 104 | 3-105 | B     |
| 105 | 3-105 | B     |
| 105 | 3-245 | c     |
| 105 | 6-166 | c     |
| 109 | 3-105 | c     |
| 109 | 3-245 | D     |
| 109 | 6-166 | B     |
+-----+-------+-------+
12 rows in set (0.00 sec)


-- 显示学生姓名等

SELECT student.sno as '学号',Student.sname as '学生姓名', ssex as '性别', course.cno as '课程号', cname as '课程名称', teacher.tno as '教师编号', tname as '教师姓名', degree as '成绩', grade as '等级'
FROM student,course,score,grade,teacher WHERE degree BETWEEN low and upp
and student.sno = score.sno 
and teacher.tno = course.tno
and score.cno = course.cno;
+------+----------+------+--------+------------+----------+----------+------+------+
| 学号 | 学生姓名 | 性别 | 课程号 | 课程名称   | 教师编号 | 教师姓名 | 成绩 | 等级 |
+------+----------+------+--------+------------+----------+----------+------+------+
| 101  | 曾华     | 男   | 3-105  | 计算机导论 | 825      | 王萍     |   90 | A    |
| 102  | 匡明     | 男   | 3-105  | 计算机导论 | 825      | 王萍     |   91 | A    |
| 103  | 王丽     | 女   | 3-105  | 计算机导论 | 825      | 王萍     |   92 | A    |
| 103  | 王丽     | 女   | 3-245  | 操作系统   | 804      | 李诚     |   86 | B    |
| 103  | 王丽     | 女   | 6-166  | 数字电路   | 856      | 张旭     |   85 | B    |
| 104  | 李军     | 男   | 3-105  | 计算机导论 | 825      | 王萍     |   89 | B    |
| 105  | 王芳     | 女   | 3-105  | 计算机导论 | 825      | 王萍     |   88 | B    |
| 105  | 王芳     | 女   | 3-245  | 操作系统   | 804      | 李诚     |   75 | c    |
| 105  | 王芳     | 女   | 6-166  | 数字电路   | 856      | 张旭     |   79 | c    |
| 109  | 赵铁柱   | 男   | 3-105  | 计算机导论 | 825      | 王萍     |   76 | c    |
| 109  | 赵铁柱   | 男   | 3-245  | 操作系统   | 804      | 李诚     |   68 | D    |
| 109  | 赵铁柱   | 男   | 6-166  | 数字电路   | 856      | 张旭     |   81 | B    |
+------+----------+------+--------+------------+----------+----------+------+------+
12 rows in set (0.00 sec)


``` 





















