### DDL（Data Definition Language）
- 对象：数据库和表
- 关键词：==create alter drop truncate==(删除当前表再新建一个一模一样的表结构)
- eg：

```
-- 创建数据库
create database school;

-- 删除数据库
drop database school;

-- 切换数据库
use school;
　　　　
-- 查看数据库里存在的表
show tables;

-- 创建表
create table student(
　　id int(4) primary key auto_increment,

　　name varchar(20),

　　score int(3)
);
　　　　
-- 修改表
alter table student rename (to) teacher;

alter table student add password varchar(20);

alter table student change password pwd varchar(20);

alter table student modify pwd int;

alter table student drop pwd;

-- 删除表
drop table student;

-- 删除表再创建表
truncate table student;

-- 查看生成表的sql语句
show create table student;

-- 查看表结构
desc student;
```

### DML（Data Manipulation Language）
- 对象：纪录(行)
- 关键词：insert update delete
- eg：

```
-- 插入
insert into student values(01,'tonbby',99); (插入所有的字段)

insert into student(id,name) values(01,'tonbby'); (插入指定的字段)
　　　
-- 更新
update student set name = 'tonbby',score = '99' where id = 01;

-- 删除
delete from tonbby where id = 01;
```

### DQL（Data Query Language）
- 对象：纪录(行)
- 关键词：select
- eg：
```
select ... from student where 条件 group by 分组字段 having 条件 order by 排序字段

执行顺序：from->where->group by->having->order by->select

注意：group by 通常和聚合函数(avg(),count()...)一起使用 ,经常先使用group by关键字进行分组，然后再进行集合运算。

　　　group by与having 一起使用，可以限制输出的结果，只有满足条件表达式的结果才会显示。

having和where的区别：
　　两者起作用的地方不一样，where作用于表或视图，是表和视图的查询条件。having作用于分组后的记录，用于选择满足条件的组。
```

### DCL（Data Control Language）
- 对象：数据库用户
- 关键词：create grant revoke show drop
- eg：

```
-- 创建用户（用户只能在指定的IP地址上登录）
create user 用户名@IP地址 identified by '密码';

-- 创建用户（用户只能在指定的IP地址上登录）
create user 用户名@'%' identified by '密码';

--  给用户授权（给用户分派在指定的数据库的指定的权限）
grant 权限1，权限2，... on 数据库.* to 用户名@IP地址;
--  给用户授权（给用户分派在指定数据库的所有的权限）
grant all on 数据库.* to 用户名@;

-- 撤销权限（撤销指定用户在指定数据上的指定权限）
revoke 权限1，...，权限n on 数据库.* from 用户名@IP地址;

-- 查看权限
show grants for 用户名@IP地址;

-- 删除用户
drop user 用户名@IP地址;

-- 权限
select：查询权限

insert：插入权限

update：修改权限

delete：删除权限

all  privileges：所有的权限

...
```

### 其它

```sql
-- 可以查看是否使用索引、使用的哪个索引等
explain + SQL语句

-- 查看服务器的信息（InnoDB引擎）
show engine innodb status
```
