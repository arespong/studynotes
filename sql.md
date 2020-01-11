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

1.单表查询指定列

```mysql
SELECT  字段名 1,  字段名 2,  字段名 3, ... FROM  表名;
```

2.起别名查询

```mysql
SELECT  字段名 1 AS  别名,  字段名 2 AS  别名... FROM  表名;
```

3.去重查询

```mysql
SELECT DISTINCT  字段名 FROM  表名;
```

4.排序

```mysql
SELECT  字段名 FROM  表名 WHERE  字段= 值 ORDER BY  字段名 [ASC|DESC];
```

5.聚合函数纵向查询常见函数如下

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

6.分组

```mysql
-- 分组查询是指使用 GROUP BY  语句对查询信息进行分组，相同数据作为一组
SELECT  字段 1, 字段 2... FROM  表名 GROUP BY  分组字段 [HAVING  条件]；
```



