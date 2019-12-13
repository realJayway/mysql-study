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



