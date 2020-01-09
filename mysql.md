## DDL  相关操作

### 1.数据库操作

1.1创建数据库

```sql
CREATE DATABASE 数据库名；
--  判断数据库是否已经存在，不存在则创建数据库
CREATE DATABASE IF NOT EXISTS  数据库名;
--  创建数据库并指定字符集
CREATE DATABASE  数据库名 CHARACTER SET  字符集;
```

1.2查看数据库

```mysql
-- 查看当前所有数据库
show databases ;
-- 查看某个数据库的定义信息
show create database 数据库名称;
-- 查看正在使用的数据库
select database();
```

1.3删除数据库

```mysql
DROP DATABASE IF EXISTS 数据库名称；
```

1.4使用数据库

```mysql
USE 数据库名称；
```

### 2.数据表操作

2.1创建数据库表

```mysql
CREATE TABLE  表名 (
字段名 1  字段类型 1,
字段名 2  字段类型 2
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 快速创建一个已有的数据库表结构
CREATE TABLE  新表名 LIKE  旧表名;
```

2.2 查看表

```mysql
-- 查看某个数据库所有的表
SHOW TABLES;
-- 查看表结构
DESC 表的名字;
-- 查看表的创建sql
show create table 表的名称;
```

2.3 删除表

```mysql
DROP TABLE IF EXISTS 表名字;
```

2.4 修改表

```mysql
-- 修改表名
RENAME TABLE  表名 TO  新表名;
-- 修改表的字符集
ALTER TABLE  表名 character set  字符集;
-- 修改表的存储引擎
alter table 表名 ENGINE = 存储引擎名;
-- 为表新增字段
ALTER TABLE  表名 ADD  列名  类型;
-- 修改表中某一字段的类型
ALTER TABLE  表名 MODIFY  列名  新的类型;
-- 修改列名以及类型
ALTER TABLE  表名 CHANGE  旧列名  新列名  类型;


```

## DML相关操作

### 1.表记录操作

1.1 新增记录

```mysql
-- 新增一条记录
INSERT INTO 表名 (字段1,字段2,字段3,....,字段n) values (值1,值2,值3,....,值n);
-- 新增所有字段
INSERT INTO  表名 VALUES (值 值 1, 值 值 2, 值 值 3…); 
```

1.2 修改记录

```mysql
-- 不带条件修改数据，将所有的性别改成女
update student set sex = '女';
-- 带条件修改数据，将 id 号为 2 的学生性别改成男
update student set sex='男' where id=2;
```

1.3 删除记录

```mysql
-- 带条件删除数据，删除 id 为 1 的记录
delete from student where id=1;
-- 不带条件删除数据,删除表中的所有数据
delete from student

-- DELETE 删除某条记录可以回滚 ， truncate 删除记录以及索引，触发器等不能回滚 ， drop 删除整个表释放表的所有空间

```

## DQL相关操作

1.1.单表查询指定列

```mysql
SELECT  字段名 1,  字段名 2,  字段名 3, ... FROM  表名;
```

1.2.起别名查询

```mysql
SELECT  字段名 1 AS  别名,  字段名 2 AS  别名... FROM  表名;
```

1.3.去重查询

```mysql
SELECT DISTINCT  字段名 FROM  表名;
```

1.4.排序

```mysql
SELECT  字段名 FROM  表名 WHERE  字段= 值 ORDER BY  字段名 [ASC|DESC];
-- 查询所有数据,在年龄降序排序的基础上，如果年龄相同再以数学成绩升序排序
select * from student order by age desc, math asc;
```

1.5.聚合函数纵向查询常见函数如下

```mysql
-- 查询年龄大于 20 的总数
select count(*) from student where age>20;
-- 查询数学成绩总分
select sum(math) 总分 from student;
-- 查询数学成绩平均分
select avg(math) 平均分 from student;
-- 查询数学成绩最高分
select max(math) 最高分 from student;
-- 查询数学成绩最低分
select min(math) 最低分 from student;
```

1.6.分组

```mysql
-- 分组查询是指使用 GROUP BY  语句对查询信息进行分组，相同数据作为一组
SELECT  字段 1, 字段 2... FROM  表名 GROUP BY  分组字段 [HAVING  条件]；
```

1.7.分页查询

```mysql
-- 最后如果不够 5 条，有多少显示多少
select * from student3 limit 10,5;
```



1.8  约束

1.8.1 主键操作

```mysql
-- 创建表学生表 st5, 包含字段(id, name, age)将 id 做为主键
create table test (
id int primary key, -- id 为主键
name varchar(20),
age int
)

-- 添加主键
alter table 表名 add primary key(字段);

-- 删除  表的主键
alter table 表名 drop primary key;

-- 删除 st5 表的主键
alter table 表名 drop primary key;

```

1.8.2 自增  --一般含有索引值才能自增

```mysql
--ALTER TABLE  表名 AUTO_INCREMENT= 起始值;
alter table 表名 auto_increment = 2000;
```

1.8.2 唯一、非空、默认

```mysql
字段名  字段类型 UNIQUE
字段名  字段类型 NOT NULL
字段名  字段类型 DEFAULT 
```

1.9 外键（级联：在修改和删除主表的主键时，同时更新或删除副表的外键值，称为级联操作）相关操作

```mysql
-- 新建表时添加
[CONSTRAINT] [ 外键约束名称] FOREIGN KEY( 外键字段名) REFERENCES  主表名( 主键字段名)
-- 已有表时创建
ALTER TABLE  从表 ADD [CONSTRAINT] [ 外键约束名称] FOREIGN KEY ( 外键字段名) REFERENCES  主表( 主
键字段名);
-- 删除外键
ALTER TABLE  从表 drop foreign key 
```

2多表查询

2.1笛卡尔积

```mysql
SELECT 字段1，SELECT 字段2，SELECT 字段3，..... FROM 表1，表2.....
```

2.2 内连接



```mysql
-- 显示内连接
-- 用左边表的记录去匹配右边表的记录，如果符合条件的则显示。如：从表.外键=主表.主键
SELECT  字段名 FROM  左表 [INNER] JOIN  右表 ON 
-- 隐示内连接
SELECT  字段名 FROM  左表 ,  右表 WHERE ...
```

2.3 外链接

在内连接的基础上保证左表的数据全部显示

```mysql
SELECT  字段名 FROM  左表 RIGHT [OUTER ]JOIN  右表 ON  条件
```



## DCL相关操作

```mysql
-- 备份
mysqldump -u  用户名 -p  密码  数据库 > path //格式
mysqldump -uroot -ppt123 db-test > d:/test.sql

-- 还原
use db-test;
source sql脚本路径;
```

## sql的执行顺序



```mysql
-- 手写sql:
select <select_list>
from <table_name>
<join_type> join <join_table> on <join_condition>
where <where_condition>
group by <group_by_list>
having <having_condition>
order by <order_by_condition>
limit <limt_number>
-- 二、MySql执行顺序
from <left table>
on <on_condition>
<join_type> join <join_table>
where <where_condition>
group by <group_by_list>
<sum()avg()等聚合函数>
having <having_condition>
select <select_list>
distinct
order by <order_by_condition>
limit <limit_number>
-- 三、MySql执行顺序理解
第一步：加载from子句的前两个表计算笛卡尔积，生成虚拟表vt1；

第二步：筛选关联表符合on表达式的数据，保留主表，生成虚拟表vt2；

第三步：如果使用的是外连接，执行on的时候，会将主表中不符合on条件的数据也加载进来，做为外部行

第四步：如果from子句中的表数量大于2，则重复第一步到第三步，直至所有的表都加载完毕，更新vt3；

第五步：执行where表达式，筛选掉不符合条件的数据生成vt4；

第六步：执行group by子句。group by 子句执行过后，会对子句组合成唯一值并且对每个唯一值只包含一行，生成vt5,。一旦执行group by，后面的所有步骤只能得到vt5中的列（group by的子句包含的列）和聚合函数。

第七步：执行聚合函数，生成vt6；

第八步：执行having表达式，筛选vt6中的数据。having是唯一一个在分组后的条件筛选，生成vt7;

第九步：从vt7中筛选列，生成vt8；

第十步：执行distinct，对vt8去重，生成vt9。其实执行过group by后就没必要再去执行distinct，因为分组后，每组只会有一条数据，并且每条数据都不相同。

第十一步：对vt9进行排序，此处返回的不是一个虚拟表，而是一个游标，记录了数据的排序顺序，此处可以使用别名；

第十二步：执行limit语句，将结果返回给客户端

```

## 事务

```mysql
-- 执行成功的情况： 开启事务  执行多条 SQL 语句  成功提交事务
 start transaction;
 update account set money=500 where name='张三';
 update account set money=1500 where name='李四';
 commit;
-- 设置隔离级别 
 set global transaction isolation level  级别字符串;
 
-- MySQL提供了3个等级的隔离级别配置，下面分别列出配置方法：

-- 全局
-- 直接改配置文件
set global transaction isolation level repeatable read;

-- 当前session
set tx_isolation = 'repeatable-read';
set session transaction isolation level repeatable read;

-- 下一个事务
set transaction isolation level repeatable read;

```

