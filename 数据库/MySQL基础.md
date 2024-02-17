

# SQL基础

**SQL(Structured Query Language)**结构化查询语言是关系型数据库语言的标准

+ **DDL数据库定义语言Data Definition Language**

  用于创建数据库和数据库对象，为数据库操作提供对象（数据库、表、字段）

+ **DML数据库操作语言Data Manipulation Language**

  主要用于操作数据库中的数据

+ **DQL数据查询语言Data Query Language**

  用来查询数据库中表的记录

+ **DCL数据库控制语言Data Control Language**

  主要实现创建数据库用户、控制数据库的控制权限

1. <span style="color: orange;">[ ]内是可选参数</span>
2. SQL对大小写不敏感
3. 缩进和大小写是为了可读性和美观

## DDL

**Database operation**

```SQL
-- 查询所有数据库：
SHOW DATABASES;
-- 查询当前数据库：
SELECT DATABASE();

-- 创建数据库
CREATE DATABASE [ IF NOT EXISTS ] 数据库名
[ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];

-- 删除数据库
DROP DATABASE [ IF EXISTS ] 数据库名;
-- 使用数据库
USE 数据库名;
```

<span style="color: orange;"></span>

+ UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

**Table operation**

```SQL
-- 查询当前数据库所有表
SHOW TABLES;
-- 查询表结构
DESC 表名;
-- 查询指定表的建表语句
SHOW CREATE TABLE 表名;

-- 创建表
CREATE TABLE 表名(
    字段1 字段1类型 [COMMENT 字段1注释],
    字段2 字段2类型 [COMMENT 字段2注释],
    字段3 字段3类型 [COMMENT 字段3注释],
    ...
    字段n 字段n类型 [COMMENT 字段n注释]
    -- 最后一个字段后无逗号
) [ COMMENT 表注释 ];

-- 添加字段
ALTER TABLE 表名 
	ADD 字段名 类型(长度) [COMMENT 注释] [约束];
example：ALTER TABLE emp
	ADD nickname varchar(20) COMMENT '昵称';

-- 修改数据类型
ALTER TABLE 表名 
	MODIFY 字段名 新数据类型(长度);
-- 修改字段名和字段类型
ALTER TABLE 表名
	CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
-- example：将emp表的nickname字段修改为username，类型为varchar(30)
ALTER TABLE emp
	CHANGE nickname username varchar(30) COMMENT '昵称';

-- 删除字段
ALTER TABLE 表名
		DROP 字段名;

-- 修改表名
ALTER TABLE 表名
		RENAME TO 新表名

-- 删除表
DROP TABLE [IF EXISTS] 表名;
-- 删除表，并重新创建该表
TRUNCATE TABLE 表名;
```

## DML

**添加数据**

```SQL
-- 指定字段
INSERT INTO 表名 
	(字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
-- 全部字段
INSERT INTO 表名 
	VALUES (值1, 值2, ...);

-- 批量添加数据
INSERT INTO 表名 
	(字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
	
INSERT INTO 表名 
	VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
```

+ 字符串和日期类型数据应该包含在引号中
+ 插入的数据大小应该在字段的规定范围内

**更新和删除数据**

```SQL
-- 修改数据
UPDATE 表名 
	SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];
example：
UPDATE emp 
	SET name = 'Jack' WHERE id = 1;

-- 删除数据
DELETE FROM 表名 [ WHERE 条件 ];
```

## DQL

+ **DQL语句书写顺序**

```SQL
SELECT
    字段列表
FROM
    表名字段
WHERE
    条件列表
GROUP BY
    分组字段列表
HAVING
    分组后的条件列表
ORDER BY
    排序字段列表
LIMIT
    分页参数
```

+ **DQL语句执行顺序**

```SQL
FROM 
  	表名字段
WHERE
    条件列表
GROUP BY
    分组字段列表
HAVING
    分组后的条件列表
SELECT
 	字段列表	
ORDER BY
    排序字段列表
LIMIT
    分页参数
```

+ **基础查询**

```SQL
-- 查询多个字段
SELECT 字段1, 字段2, 字段3, ... FROM 表名;
-- *表示所有字段
SELECT * FROM 表名;

-- 设置别名 
SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... FROM 表名;
-- AS可省略
SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... FROM 表名;

-- 去除重复记录
SELECT DISTINCT 字段列表 FROM 表名;
```

***

###  **条件查询**

`WHERE`后就是条件语句，`WHERE`只有一个，多个条件使用`AND`或`OR`连接

| 逻辑运算符 |             功能             |
| :--------: | :--------------------------: |
| AND 或 &&  |   并且（多个条件同时成立）   |
| OR 或 \|\| | 或者（多个条件任意一个成立） |
|  NOT 或 !  |           非，不是           |


|             比较运算符             |                    功能                    |
| :--------------------------------: | :----------------------------------------: |
|                 >                  |                    大于                    |
|                 >=                 |                  大于等于                  |
|                 <                  |                    小于                    |
|                 <=                 |                  小于等于                  |
|                 =                  |                    等于                    |
|              <> 或 !=              |                   不等于                   |
| BETWEEN … AND … 之间。。。和。。。 |       在某个范围内（含最小、最大值）       |
|          IN(…) 在（...）           |        在in之后的列表中的值，多选一        |
|            LIKE 占位符             | 模糊匹配（_匹配单个字符，%匹配任意个字符） |
|          IS NULL 为 NULL           |                   是NULL                   |

```SQL
-- 转义
SELECT * FROM 表名 
	WHERE name LIKE '/_张三' ESCAPE '/'
	-- 还有 NOT LIKE
-- / 之后的_不作为通配符，因为使用了转义字符
```

默认的转义字符为`\`，使用`ESCAPE`可指定转义字符

`MySQL`支持正则表达式的匹配，在`MySQL`使用`REGEXP`运算符进行正则表达式匹配

| `WHERE 字段名 REGEXP 模式串`                                 |
| :----------------------------------------------------------- |
| `^`：匹配字符串的开头                                        |
| `$`：匹配字符串的结尾                                        |
| `.`：匹配任意单个字符（除了换行符）                          |
| `*`：匹配前面的子表达式零次或多次                            |
| `+`：匹配前面的子表达式一次或多次                            |
| `?`：匹配前面的子表达式零次或一次                            |
| `{n}`：匹配前面的子表达式恰好n次                             |
| `{n,}`：匹配前面的子表达式至少n次                            |
| `{n,m}`：匹配前面的子表达式至少n次，但不超过m次              |
| `[ ]`：字符集，用于指定要匹配的字符集合，例如`[abc]`表示匹配a、b或c中的任何一个字符 |
| `|`：或运算符，用于指定多个可能的匹配项之一，例如`ab|cd`表示匹配a或b，或者c或d |
| `()`：分组运算符，用于将多个子表达式组合成一个整体，以便可以对它们进行分组操作 |
| `\`：转义字符，用于将特殊字符（如`.`、`*`等）视为普通字符    |

***example：***

```SQL
-- 年龄等于30
select * from employee where age = 30;
-- 年龄小于30
select * from employee where age < 30;
-- 小于等于
select * from employee where age <= 30;
-- 不等于
select * from employee where age != 30;

-- 年龄在20到30之间
select * from employee where age between 20 and 30;
select * from employee where age >= 20 and age <= 30;
-- 语句不报错，但语句无效
select * from employee where age between 30 and 20;

-- 没有身份证
select * from employee where idcard is null or idcard = '';
-- 有身份证
select * from employee where idcard;
select * from employee where idcard is not null;

-- 性别为女且年龄小于30
select * from employee where age < 30 and gender = '女';
-- 年龄等于25或30或35
select * from employee where age = 25 or age = 30 or age = 35;
select * from employee where age in (25, 30, 35);

-- 姓名为两个字
select * from employee where name like '__';
-- 身份证最后为X
select * from employee where idcard like '%X';
```

***

### 聚合函数

常见聚合函数：

|  函数 | 功能     |
| ----: | :------- |
| count | 统计数量 |
|   max | 最大值   |
|   min | 最小值   |
|   avg | 平均值   |
|   sum | 求和     |

```SQL
-- 语法：
SELECT 聚合函数(字段列表) FROM 表名;
example：
SELECT count(id) from employee where workaddress = "广东省";
```



***

### 分组查询

```SQL
-- 语法：
SELECT 字段列表 FROM 表名 
	[ WHERE 条件 ] 
		GROUP BY 分组字段名
    		[ HAVING 分组后的过滤条件 ];
```

where 和 having 的区别：

- 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以

***example：***

```SQL
-- 根据性别分组，统计男性和女性数量
select gender, count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性的平均年龄
select gender, avg(age) from employee group by gender;
-- 年龄小于45，并根据工作地址分组
select workaddress, count(*) from employee
	where age < 45 group by workaddress;
-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from employee
	where age < 45 group by workaddress having address_count >= 3;
```

**注意事项**

- 执行顺序：where > 聚合函数 > having
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义

### 排序查询

```SQL
-- 语法：
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;

example：
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```

**注意事项**

- `ASC`: 升序（默认）
- `DESC`: 降序
- 如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

***

### 分页查询

```SQL
-- 语法：
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;

example：
-- 查询第一页数据，展示10条
SELECT * FROM employee LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;
```

**注意事项**

- 起始索引从0开始，起始索引 = (查询页码 - 1)X每页显示记录数
- 分页查询是数据库的方言，不同数据库有不同实现，`MySQL`是`LIMIT`
- 如果查询的是第一页数据，起始索引可以省略，直接简写 `LIMIT 10`

## DCL

**管理用户**

```SQL
-- 查询用户
USE mysql;
SELECT * FROM user;
-- 创建用户
CREATE USER '用户名'@'主机名'
	IDENTIFIED BY '密码';
	-- 若要在任意主机访问则用%通配符表示任意主机

-- 修改用户密码
ALTER USER '用户名'@'主机名'
	IDENTIFIED WITH mysql_native_password BY '新密码';

-- 删除用户
DROP USER '用户名'@'主机名';

example：
-- 创建用户test，只能在当前主机localhost访问
create user 'test'@'localhost' identified by '123456';
-- 创建用户test，能在任意主机访问
create user 'test'@'%' identified by '123456';
create user 'test' identified by '123456';
-- 修改密码
alter user 'test'@'localhost' identified with mysql_native_password by '1234';
-- 删除用户
drop user 'test'@'localhost';
```

- 主机名可以使用通配

**权限控制**

常用权限：

|         权限          |        说明        |
| :-------------------: | :----------------: |
| `ALL, ALL PRIVILEGES` |      所有权限      |
|       `SELECT`        |      查询数据      |
|       `INSERT`        |      插入数据      |
|       `UPDATE`        |      修改数据      |
|       `DELETE`        |      删除数据      |
|        `ALTER`        |       修改表       |
|        `DROP`         | 删除数据库/表/视图 |
|       `CREATE`        |   创建数据库/表    |

```SQL
-- 查询权限
SHOW GRANTS FOR '用户名'@'主机名';

-- 授予权限
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';

-- 撤销权限
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

**注意事项**

- 多个权限用逗号分隔
- 授权时，数据库名和表名可以用 * 进行通配，代表所有

# 函数

+ MySQL中的聚合函数不能直接嵌套使用，但可以通过子查询的方式进行嵌套

## 字符串函数



|                             函数 | 功能                                                         |
| -------------------------------: | :----------------------------------------------------------- |
|          `CONCAT(s1, s2, …, sn)` | 字符串拼接，将s1, s2, …, sn拼接成一个字符串                  |
|                     `LOWER(str)` | 将字符串全部转为小写                                         |
|                     `UPPER(str)` | 将字符串全部转为大写                                         |
|              `LPAD(str, n, pad)` | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度    |
|              `RPAD(str, n, pad)` | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度    |
|                      `TRIM(str)` | 去掉字符串头部和尾部的空格                                   |
|     `SUBSTRING(str, start, len)` | 返回从字符串str从start位置起的len个长度的字符串              |
| `REPLACE(str, from_str, to_str)` | str原始字符串，from_str需要被替换的子串，to_str用于替换的新子串 |

***example：***

```SQL
-- 拼接
SELECT CONCAT('Hello', 'World');
result：HelloWorld
-- 小写
SELECT LOWER('Hello');
result：hello
-- 大写
SELECT UPPER('Hello');
result：HELLO
-- 左填充
SELECT LPAD('01', 5, '-');
result：---01
-- 右填充
SELECT RPAD('01', 5, '-');
result：01---
-- 去除首尾空格，不是中间空格
SELECT TRIM('   Hello World ');
result：Hello World
-- 切片（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);
result：Hello
-- 替换
SELECT REPLACE('Hello World', 'World', 'MySQL');
result：Hello MySQL
```

## 数值函数



|          函数 | 功能                             |
| ------------: | :------------------------------- |
|     `CEIL(x)` | 向上取整                         |
|    `FLOOR(x)` | 向下取整                         |
|   `MOD(x, y)` | 返回x/y的模                      |
|      `RAND()` | 返回0 ~ 1内的随机数              |
| `ROUND(x, y)` | 求参数x的四舍五入值，保留y位小数 |
|    `MOD(x,y)` | 返回x%y的余数                    |
|  `POWER(x,y)` | 返回x的y次方                     |
|     `SORT(x)` | 返回x的平方根                    |
|        `PI()` | 返回圆周率的值                   |
|      `LOG(x)` | 返回x的自然对数(以e为底)         |
|      `EXP(x)` | 返回e(~=2.71828)的x次方          |

##日期函数

|                            CURDATE() | 返回当前日期                                      |
| -----------------------------------: | ------------------------------------------------- |
|                          `CURTIME()` | 返回当前时间                                      |
|                              `NOW()` | 返回当前日期和时间                                |
|                         `YEAR(date)` | 获取指定date的年份                                |
|                        `MONTH(date)` | 获取指定date的月份                                |
|                          `DAY(date)` | 获取指定date的日期                                |
| `DATE_ADD(date, INTERVAL expr type)` | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
|             `DATEDIFF(date1, date2)` | 返回起始时间date1和结束时间date2之间的天数        |

## 流程函数

| 函数                                                         | 功能                                                    |
| :----------------------------------------------------------- | :------------------------------------------------------ |
| `IF(value, t, f)`                                            | 如果value为true，则返回t，否则返回f                     |
| `IFNULL(value1, value2)`                                     | 如果value1不为空，返回value1，否则返回value2            |
| `CASE WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END`    | 如果val1为true，返回res1，… 否则返回default默认值       |
| `CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END` | 如果expr的值等于val1，返回res1，… 否则返回default默认值 |

***example：***

```SQL
select
    name,
    (case when age > 30 then '中年' else '青年' end)
from employee;
select
    name,
    (case workaddress when '北京市' then '一线城市'
     				 when '上海市' then '一线城市'
                       else '二线城市' end) as '工作地址'
from employee;
```

+ `CASE`表达式不会修改表的内容，它主要用于查询结果的转换和条件判断



***

# 约束



| 约束     | 描述                                                         | 关键字           |
| :------- | :----------------------------------------------------------- | :--------------- |
| 非空约束 | 限制该字段的数据不能为null                                   | `NOT NULL`       |
| 唯一约束 | 保证该字段的所有数据都是唯一、不重复的                       | `UNIQUE`         |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                     | `PRIMARY KEY`    |
| 默认约束 | 保存数据时，如果未指定该字段的值，则采用默认值               | `DEFAULT`        |
| 检查约束 | 保证字段值满足某一个条件                                     | `CHECK`          |
| 外键约束 | 用来让两张图的数据之间建立连接，保证数据的一致性和完整性     | `FOREIGN KEY`    |
| 自动增长 | 当向表中插入新记录时，如果该字段没有显式指定值，那么该字段的值将自动递增 | `AUTO_INCREMENT` |

***example：***

```SQL
create table user(
    id int primary key auto_increment,
    name varchar(10) not null unique,
    -- 同一字段的多个约束之间空格隔开，没有逗号
    age int check(age > 0 and age < 120),
    status char(1) default '1',
    gender char(1)
);
```

**外键约束**

```SQL
-- 添加外键
CREATE TABLE 表名(
    字段名 字段类型,
    ...
    [CONSTRAINT] [外键名称] 
    	FOREIGN KEY (外键字段名) 
    		REFERENCES 主表(主表字段名)
);
ALTER TABLE 表名
	ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名)
    	REFERENCES 主表(主表字段名);
example：
alter table emp
	add constraint fk_emp_dept_id foreign key(dept_id)
		references dept(id);
-- 删除外键
ALTER TABLE 表名 DROP FOREIGN KEY 外键名;
```

**删除/更新行为**



```SQL
ALTER TABLE 表名 
	ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段)
    	REFERENCES 主表名(主表字段名) [ON UPDATE 行为] [ON DELETE 行为];
```

+ CASCADE：当更新或删除主表中的记录时，自动更新或删除从表中与之关联的记录
+ SET NULL：当更新或删除主表中的记录时，将从表中的一个或多个列设置为NULL
+ NO ACTION：当更新或删除主表中的记录时，不执行任何操作
+ RESTRICT：拒绝主表更新或修改外键的关联字段

***

#  <span style="color: orange;">重点：多表查询</span>

多表关系

- 一对多（多对一）
- 多对多
- 一对一

```SQL
-- 合并查询（笛卡尔积，会展示所有组合结果）
select * from employee, dept;

笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）

-- 消除无效笛卡尔积
select * from employee, dept where employee.dept = dept.id;
```

## 内连接查询

内连接查询的是两张表交集的部分

```SQL
-- 隐式内连接
SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;

-- 显式内连接
SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ...;

显式性能比隐式高

example：
-- 查询员工姓名，及关联的部门的名称
-- 隐式
select e.name, d.name from
	employee as e, dept as d where e.dept = d.id;
-- 显式
select e.name, d.name from 
	employee as e inner join dept as d on e.dept = d.id;
```

## 外连接查询

```SQL
-- 左外连接
查询左表所有数据，以及两张表交集部分数据
SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;
相当于查询表1的所有数据，包含表1和表2交集部分数据

-- 右外连接
查询右表所有数据，以及两张表交集部分数据
SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;
```

## 自连接查询

当前表与自身的连接查询，自连接必须使用表别名，别名不能相同
自连接查询，可以是内连接查询，也可以是外连接查询

```SQL
-- 语法
SELECT 字段列表 FROM 
	表A 别名 JOIN 表A 别名 ON 条件 ...;

example：
-- 查询员工及其所属领导的名字
select a.name, b.name from 
	employee a, inner join employee b on a.manager = b.id;
-- 没有领导的也查询
select a.name, b.name from 
	employee a left join employee b on a.manager = b.id;
```

## 联合查询

把多次查询的结果合并，形成一个新的查询集

```SQL
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```

**注意事项**

- `UNION ALL`会有重复结果，`UNION `不会
- 联合查询比使用`OR`效率高，不会使索引失效

## 子查询

SQL语句中嵌套`SELECT`语句，称为嵌套查询，又称子查询

**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**

+ 用于检查主查询的结果集是否存在满足条件的记录，它返回布尔值，而不返回实际的数据
+ `exists`：如果子查询返回至少一行数据，则`exists`返回true，否则返回false
+ `not exists`：如果子查询没有返回任何行数据，则`not exists`返回true，否则返回false

根据子查询结果可以分为：

- 标量子查询（子查询结果为单个值）
- 列子查询（子查询结果为一列）
- 行子查询（子查询结果为一行）
- 表子查询（子查询结果为多行多列）

根据子查询位置可分为：

- `WHERE`之后
- `FROM`之后
- `SELECT`之后

### 标量子查询

```SQL
子查询返回的结果是单个值（数字、字符串、日期等）

-- 查询销售部所有员工
select id from dept where name = '销售部';
-- 根据销售部部门ID，查询员工信息
select * from employee where dept = 4;
-- 合并（子查询）
select * from employee
	where dept = (select id from dept where name = '销售部');
-- 查询xxx入职之后的员工信息
select * from employee
	where entrydate > (select entrydate from employee where name = 'xxx');
```

### 列子查询

返回的结果是一列或多列

+ `IN`：在指定的集合范围内多选一
+ `NOT IN`：不在指定的集合范围内
+ `ANY`：子查询返回列表中，有任意一个满足即可
+ `SOME`：与ANY等同，使用SOME的地方都可以使用ANY
+ `ALL`：子查询返回列表的所有值都必须满足

```SQL
-- 查询销售部和市场部的所有员工信息
select * from employee 
	where dept in (select id from dept where name = '销售部' or name = '市场部');
-- 查询比财务部所有人工资都高的员工信息
select * from employee
	where salary > all(select salary from employee where dept =
            		  (select id from dept where name = '财务部'));
-- 查询比研发部任意一人工资高的员工信息
select * from employee
	where salary > any (select salary from employee where dept =
                        (select id from dept where name = '研发部'));
```

### 行子查询

返回的结果是一行或多行

```SQL
-- 查询与xxx的薪资及直属领导相同的员工信息
select * from employee where (salary, manager) = (12500, 1);
select * from employee where (salary, manager) = 
						   (select salary, manager from employee where name = 'xxx');
```

### 表子查询

返回结果是多行多列

```SQL
-- 查询与xxx1，xxx2的职位和薪资相同的员工
select * from employee 
	where (job, salary) in
		(select job, salary from employee where name = 'xxx1' or name = 'xxx2');
-- 查询入职日期是2006-01-01之后的员工，及其部门信息
select e.*, d.* from 
	(select * from employee where entrydate > '2006-01-01') as e 
		left join dept as d on e.dept = d.id;
```

***

# 事务

事务是一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求
这些操作要么同时成功，要么同时失败

```SQL
-- 查看事务提交方式
SELECT @@AUTOCOMMIT;
-- 设置事务提交方式，1为自动提交，0为手动提交，该设置只对当前会话有效
SET @@AUTOCOMMIT = 0;

-- 开启事务
START TRANSACTION 或 BEGIN TRANSACTION;
-- 提交事务 使更改永久生效
COMMIT;
-- 回滚事务 取消之前的更改
ROLLBACK;

-- 在事务中设置保存点，以便稍后能够回滚到该点
SAVEPOINT savepoint_name;
-- 回滚到之前设置的保存点
ROLLBACK TO SAVEPOINT savepoint_name;
```

`savepoint` 是在数据库事务处理中实现 子事务（subtransaction），也称为嵌套事务的方法
事务可以回滚到 `savepoint` 而不影响 `savepoint` 创建前的变化, 不需要放弃整个事务

ROLLBACK 回滚的用法可以设置保留点 SAVEPOINT，执行多条操作时，回滚到想要的那条语句之前

**四大特性ACID**

- 原子性(Atomicity)：事务是不可分割的最小操作但愿，要么全部成功，要么全部失败
- 一致性(Consistency)：事务完成时，必须使所有数据都保持一致状态
- 隔离性(Isolation)：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

​													**并发事务**

| 问题       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| 脏读       | 一个事务读到另一个事务还没提交的数据                         |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同             |
| 幻读       | 一个事务按照条件查询数据时，没有对应的数据行，但是再插入数据时，又发现这行数据已经存在 |

**并发事务隔离级别**

| 隔离级别              | 脏读 | 不可重复读 | 幻读 |
| :-------------------- | :--- | :--------- | :--- |
| Read uncommitted      | √    | √          | √    |
| Read committed        | ×    | √          | √    |
| Repeatable Read(默认) | ×    | ×          | √    |
| Serializable          | ×    | ×          | ×    |

- √表示在当前隔离级别下该问题会出现
- Serializable性能最低；Read uncommitted性能最高，数据安全性最差

```SQL
-- 查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;
-- 设置事务隔离级别
SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL
	{READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE };
-- SESSION表示只针对当前会话有效，GLOBAL表示对所有会话有效
```

***

# 存储引擎

存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表而不是基于库的，所以存储引擎也可以被称为表引擎

```SQL
-- 创建表时，指定存储引擎
CREATE TABLE 表名(
	字段1 字段1类型
	...
    字段n 字段n类型
)ENGINE= INNODB;
-- 查询当前数据库支持的存储引擎
SHOW ENGINES;
```

**InnoDB**

InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB是默认的MySQL引擎

特点：

- DML 操作遵循ACID模型，支持**事务**
- **行级锁**，提高并发访问性能
- 支持**外键**约束，保证数据的完整性和正确性

文件：

- xxx.ibd

  xxx代表表名，InnoDB引擎的每张表都会对应这样一个表空间文件存储该表的表结构（frm、sdi）、数据和索引

![InnoDB逻辑存储结构](https://learning-logs-1253130399.cos.ap-guangzhou.myqcloud.com/editor/%E9%80%BB%E8%BE%91%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84_20220316030616590001.png)

**MyISAM**

MyISAM是MySQL早期的默认存储引擎

特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

文件：

- xxx.sdi: 存储表结构信息(json文件，可直接打开)
- xxx.MYD: 存储数据
- xxx.MYI: 存储索引

**Memory**

Memory引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用

特点：

- 存放在内存中，速度快
- hash索引（默认）

文件：

- xxx.sdi: 存储表结构信息

在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合

- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，则InnoDB是比较合适的选择
- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的
- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性

电商中的足迹和评论适合使用MyISAM引擎，缓存适合使用Memory引擎



# 索引



 <span style="color: orange;"></span>

 <span style="color: orange;"></span>

<span style="color: orange;"></span>

# 视图

视图可以将查询包装成一个虚拟表，视图并不在数据库中以存储的数据值集形式存在，其行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成

+ 重用`sql`语句

+ 简化复杂的`sql`操作

  在编写查询后，可以方便地重用它而不必知道它的基本查询细节

+ 使用表的组成部分而不是整个表

+ 保护数据

  可以给用户授予表的特定部分的访问权限而不是整个表的访问权限

+ 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据

<span style="color: orange;">性能问题</span>

因为视图不包含数据，所以每次使用视图时都必须处理查询执行时所需的任一个检索
如果用多个联结和过滤创建了复杂的视图或者嵌套了视图，可能会发现性能下降得很厉害。因此，在部署使用了大量视图的应用前，应该进行测试。

**视图的规则和限制**

+ 与表一样，视图必须唯一命名，不能给视图取与别的视图或表相同的名字

  

+ 对于可以创建的视图数目没有限制

  

+ 为了创建视图，必须具有足够的访问权限。这些限制通常由数据库管理人员授予

  

+ 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造一个视图

  

+ `ORDER BY`可以用在视图中，但如果从该视图检索数据`SELECT`中也含有`ORDER BY`，那么该视图中的`ORDER BY`将被覆盖

  

+ 视图不能索引，也不能有关联的触发器或默认值

  

+ 视图可以和表一起使用。例如，编写一条联结表和视图的`SELECT`语句

## 视图基本操作

**创建视图**

```sql
CREATE [OR REPLACE] VIEW [databasename] viewname[(ColumnNameList)]
AS
SELECT
[WITH [CASCADED|LOCAL] CHECK OPTION]
```

+ `OR REPLACE`：若视图存在则修改定义，否则创建新视图

+ 列名列表：视图自定义的列名

  该列表中的列名必须与视图定义的`SELECT`语句查询结果列一一对应

  若使用与源表或视图中相同的列名则可以省略列名列表、

+ `WITH [CASCADED|LOCAL] CHECK OPTION`：表示要保证在该视图的权限范围内更新视图

  `CASCADED`表示更新视图时要满足所有相关视图和表的条件

  `LOCAL`表示更新视图时要满足该视图本身定义的条件
  

**查看视图**

```sql
SHOW CREATE VIEW viewname
-- 查看视图基本信息

DESCRIBE viewname
-- 查看视图结构信息

-- MySQL的information_schema数据库中的view表存储所有视图的定义
SELECT * FROM information_schema.VIEWS
WHERE TABLE_SCHEMA = databasename;

-- 查看视图的方法与查看表基本相似
SELECT * FROM
viewname
WHERE condition
```

视图可以像基本表查询一样通过视图查询数据

**维护视图**

修改视图可直接在创建视图语句的原有基础上更改
或使用`ALTER VIEW`语句

```SQL
ALTER VIEW [databasename] viewname[(ColumnNameList)]
AS
SELECT
[WITH [CASCADED|LOCAL] CHECK OPTION]
```

**删除视图**

```SQL
DROP VIEW [IF EXISTS] viewname
```

**更新视图**

  <span style="color: orange;">`notice：`</span>
若视图包含以下任一情况，则该视图不可更新：

+ 包含聚合函数
+ 包含`DISTINCT`、`UNION`、`ORDER BY`、`GROUP BY`、`HAVING`等关键字或子句
+ 包含子查询(多表连接)
+ 由不可更新的视图导出的视图
+ 视图对应的基本表中存在没有默认值且不为空的列，而该列未包含在视图中

**要使视图可更新，视图中的行与基本表中的行之间必须存在一对一的关系**

1. 通过视图修改数据

```SQL
UPDATE viewname
SET fieldname1=value1,fieldname2=value2
WHERE condition
```

2. 通过视图向基本表插入数据

```SQL
INSERT [INTO] viewname[(ColumnNameList)]
VALUES (valueList1),(valueList2),(valueList3)
```

3. 通过视图删除基本表中的数据

```SQL
DELETE FROM viewname
WHERE condition
```

# 数据类型

## 整型



| 类型名称        | 取值范围                                   | 大小    |
| :-------------- | :----------------------------------------- | :------ |
| `TINYINT`       | -128 ~ 127                                 | 1个字节 |
| `SMALLINT`      | -32768 ~ 32767                             | 2个宇节 |
| `MEDIUMINT`     | -8388608 ~ 8388607                         | 3个字节 |
| `INT (INTEGHR)` | -2147483648 ~ 2147483647                   | 4个字节 |
| `BIGINT`        | -9223372036854775808 ~ 9223372036854775807 | 8个字节 |

无符号在数据类型后加 `unsigned` 关键字

## 浮点型



| 类型名称                     | 说明                       | 存储需求   |
| :--------------------------- | :------------------------- | :--------- |
| `FLOAT`                      | 单精度浮点数               | 4 个字节   |
| `DOUBLE`                     | 双精度浮点数               | 8 个字节   |
| `DECIMAL (M, D)`或`DEC(M,D)` | M是数据长度D是小数点后位数 | M+2 个字节 |

## 日期和时间

| 类型名称    | 日期格式            | 日期范围                                            | 存储需求 |
| :---------- | :------------------ | :-------------------------------------------------- | :------- |
| `YEAR`      | YYYY                | 1901  ~  2155                                       | 1 个字节 |
| `TIME`      | HH:MM:SS            | -838:59:59  ~  838:59:59                            | 3 个字节 |
| `DATE`      | YYYY-MM-DD          | 1000-01-01  ~  9999-12-3                            | 3 个字节 |
| `DATETIME`  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00  ~  9999-12-31 23:59:59         | 8 个字节 |
| `TIMESTAMP` | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC  ~  2040-01-19 03:14:07 UTC | 4 个字节 |

## 字符串

| 类型名称     | 说明                                         | 存储需求                                                   |
| :----------- | :------------------------------------------- | :--------------------------------------------------------- |
| `CHAR(M)`    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| `VARCHAR(M)` | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| `TINYTEXT`   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8^                                      |
| `TEXT`       | 小的非二进制字符串                           | L+2字节，在此，L<2^16^                                     |
| `MEDIUMTEXT` | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24^                                     |
| `LONGTEXT`   | 大的非二进制字符串                           | L+4字节，在此，L<2^32^                                     |
| `ENUM`       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| `SET`        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

## 二进制类型

| 类型名称         | 说明                 | 存储需求                |
| :--------------- | :------------------- | :---------------------- |
| `BIT(M)`         | 位字段类型           | 大约 (M+7)/8 字节       |
| `BINARY(M)`      | 固定长度二进制字符串 | M 字节                  |
| `VARBINARY (M)`  | 可变长度二进制字符串 | M+1 字节                |
| `TINYBLOB (M)`   | 非常小的BLOB         | L+1 字节，在此，L<2^8^  |
| `BLOB (M)`       | 小 BLOB              | L+2 字节，在此，L<2^16^ |
| `MEDIUMBLOB (M)` | 中等大小的BLOB       | L+3 字节，在此，L<2^24^ |
| `LONGBLOB (M)`   | 非常大的BLOB         | L+4 字节，在此，L<2^32^ |