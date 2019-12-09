# mysql ѧϰ�ʼ�

## ���ؼ���װ
���û���
��¼
�˳�

## �����﷨
'''
-- ��ʾ�������ݿ�
show databases; (mysql��һ��Ҫ��ֺţ�����python)

-- �������ݿ�
CREATE DATABASE test;

-- �л�
ust test;

-- ��ʾ���ݿ�databases�����е����ݱ�tables
show tables;

-- �������ݱ�
mysql> CREATE TABLE pet (
    -> name VARCHAR(20),
    -> owner VARCHAR(20),
    -> species VARCHAR(20),
    -> sex CHAR(1),
    -> birth DATE,
    -> death DATE
    -> );
Query OK, 0 rows affected (0.10 sec)

-- �鿴���ݱ�ṹ
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

����

mysql> desc pet;

-- ��ѯ��
mysql> SELECT *from pet;
Empty set (0.02 sec)

-- �����
INSERT INTO pet VALUES ('puffball', 'Diane', 'hamster', 'f', '1993-03-30', NULL);

mysql> SELECT *from pet;
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| puffball | Diane | hamster | f    | 1993-03-30 | NULL  |
+----------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)

-- �޸�����
UPDATE pet SET name = 'squirrel' where owner = 'Diane';

mysql> SELECT *from pet;
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| squirrel | Diane | hamster | f    | 1993-03-30 | NULL  |
+----------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)

-- ɾ������
DELETE FROM pet where name = 'squirrel';

mysql> SELECT *from pet;
Empty set (0.00 sec)

-- ɾ����
DROP TABLE myorder;
```


## mysql����Լ��
``` shell
### ����Լ��
�ܹ�Ψһȷ��һ�ű��е�һ����¼��Ҳ����ͨ����ĳ���ֶ����Լ�����Ϳ����ǵĸ��ֶβ��ظ����Ҳ�Ϊ�ա�

����һ������Լ����
create table user(
id int primary key,
name varchar(20)
);
����1������
insert into user value(1,'����');

mysql> insert into user value(1,'����');
Query OK, 1 row affected (0.08 sec)

mysql> insert into user value(1,'����');
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'


insert into user value(2,'����')

mysql> select * from user;
+----+------+
| id | name |
+----+------+
|  1 | ���� |
|  2 | ���� |
+----+------+

insert into user value(NULL,'����');
mysql> insert into user value(NULL,'����');
ERROR 1048 (23000): Column 'id' cannot be null

## ��������
-- ֻҪ��������ֵ���������ظ��Ϳ���
create table user2(
id int,
name varchar(20),
password varchar(20),
primary key (id,name)
);


insert into user2 values(1,'����','123');
insert into user2 values(2,'����','123');

mysql> insert into user2 values(1,'����','123');
Query OK, 1 row affected (0.06 sec)

mysql> insert into user2 values(1,'����','123');
ERROR 1062 (23000): Duplicate entry '1-����' for key 'PRIMARY'

mysql> insert into user2 values(2,'����','123');
Query OK, 1 row affected (0.05 sec)

mysql> select * from user2
    -> ;
+----+------+----------+
| id | name | password |
+----+------+----------+
|  1 | ���� | 123      |
|  2 | ���� | 123      |
+----+------+----------+
2 rows in set (0.00 sec)


insert into user2 values(NULL,'����','123');

mysql> insert into user2 values(NULL,'����','123');
ERROR 1048 (23000): Column 'id' cannot be null


### ����Լ��
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

-- ���Ǵ�������Լ����ô��
create table user4(
id int,
name varchar(20)
);

-- �޸ı�ṹ���������
alter table user4 add primary key(id);
mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.05 sec)

-- ɾ����
alter table user4 drop primary key;

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.05 sec)

-- ʹ��modify�޸��ֶΣ����Լ��
alter table user4 modify id int primary key;




### ΨһԼ��

-- Լ�����ε��ֶε�ֵ�������ظ�

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


 







### �ǿ�Լ��
���ε��ֶβ���Ϊ�� NULL

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



### Ĭ��Լ��
�����ֶ�ֵ��ʱ�����û�д�ֵ���ͻ�ʹ��Ĭ��ֵ

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


### ���Լ��
�漰��������������ӱ�������͸���

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


-- 1. ����classes��û�е�����ֵ���ڸ������ǲ�����ʹ�õģ�
-- 2. �����еļ�¼���������ã��ǲ�����ɾ���ġ�

delete from classes where id=4;

mysql> delete from classes where id=4;
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`test`.`students`, CONSTRAINT `students_ibfk_1` FOREIGN KEY (`class_id`) REFERENCES `classes` (`id`))

```
-----------------------------------







## ���ݿ��������Ʒ�ʽ

### ��һ��ʽ 1NF

���ݱ��е������ֶζ��ǲ��ɷָ��ԭ��ֵ
```shell
create table student2(
id int primary key,
name varchar(20),
address varchar(20)
);

insert into student2 values(1,'Zhangsan','�й��Ĵ�ʡ�ɶ��������һ��·�϶�24��');
insert into student2 values(2,'Lisi','�й��Ĵ�ʡ�ɶ��������һ��·�϶�24��');
insert into student2 values(3,'Wangwu','�й��Ĵ�ʡ�ɶ��и������츮����2��');

mysql> select * from student2;
+----+----------+--------------------------------------+
| id | name     | address                              |
+----+----------+--------------------------------------+
|  1 | Zhangsan | �й��Ĵ�ʡ�ɶ��������һ��·�϶�24�� |
|  2 | Lisi     | �й��Ĵ�ʡ�ɶ��������һ��·�϶�24�� |
|  3 | Wangwu   | �й��Ĵ�ʡ�ɶ��и������츮����2��    |
+----+----------+--------------------------------------+
3 rows in set (0.01 sec)

-- �����Լ�����֣��������һ��ʽ��


create table student3(
id int primary key,
name varchar(20),
country varchar(10),
province varchar(10),
city varchar(10),
details varchar(30)
);

insert into student3 values(1,'Zhangsan','�й�','�Ĵ�ʡ','�ɶ���','�����һ��·�϶�24��');
insert into student3 values(2,'Lisi','�й�','�Ĵ�ʡ','�ɶ���','�����һ��·�϶�24��');
insert into student3 values(3,'Wangwu','�й�','�Ĵ�ʡ','�ɶ���','�������츮����2��');

mysql> select * from student3;
+----+----------+---------+----------+--------+----------------------+
| id | name     | country | province | city   | details              |
+----+----------+---------+----------+--------+----------------------+
|  1 | Zhangsan | �й�    | �Ĵ�ʡ   | �ɶ��� | �����һ��·�϶�24�� |
|  2 | Lisi     | �й�    | �Ĵ�ʡ   | �ɶ��� | �����һ��·�϶�24�� |
|  3 | Wangwu   | �й�    | �Ĵ�ʡ   | �ɶ��� | �������츮����2��    |
+----+----------+---------+----------+--------+----------------------+
3 rows in set (0.00 sec)

-- ��ʽ����Ƶ�Խ��ϸ������ĳЩʵ�ʲ������ܸ��ã�����һ�����Ǻô���
```


### �ڶ���ʽ 2NF

���������һ��ʽ��ǰ���£��ڶ���ʽҪ�󣬳��������ÿһ�ж�������ȫ������������
���Ҫ���ֲ���ȫ������ֻ���ܷ�������������������¡�

```shell
-- ������

create table myorder(
product_id int,
customer_id int,
product_name varchar(20),
customer_name varchar(20),
primary key(product_id, customer_id)
);

-- ���⣿
-- ��������������У�ֻ�����������Ĳ����ֶ�
-- ���

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

-- ��ֳ�������������ڶ���ʽ����ƣ�

```

### ������ʽ 3NF

����������ڶ���ʽ�����������е�������֮�䲻���д���������ϵ��

```shell

create table myorder(
order_id int primary key,
product_id int,
customer_id int,
customer_phone varchar(15)
);

-- �����������ʽ
-- ���

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


-- ���������ʽ��

```

