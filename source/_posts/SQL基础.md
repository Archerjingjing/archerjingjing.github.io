---
title: 《SQL必知必会第四版》笔记
date: 2020-02-18 18:22:15
tags: SQL数据库
categories: 学习路程
---
# SQL:Structured Query Language(结构化查询语言)
## 以下代码皆以MySQL例
## 目录：以《SQL必知必会》学习顺序记录

* [1.SELECT查询](#1)
* [2.检索不同值](#2)
* [3.限制结果](#3)
* [4.排序检索](#4)
* [5.过滤数据](#5)
* [6.通配符](#6)
* [7.计算字段](#7)
* [8.数值处理函数](#8)
* [9.聚集函数](#9)
* [10.分组数据](#10)
* [11.子查询](#11)
* [12.联结表](#12)
* [13.高级联结](#13)
* [14.组合查询](#14)
* [15.插入数据](#15)
* [16.更新和删除数据](#16)
* [17.创建和操作表](#17)
* [18.视图](#18)
* [19.存储过程](#19)
* [20.事务处理](#20)
* [21.游标](#21)
* [22.高级SQL特性](#22)
## 表
1· 表(table)：是一种结构化的文件，定义了数据在表中如何存储、包含存储什么数据、数据如何分解、信息的命名。

2· 模式(mode)：描述表的信息就是模式。

3· 列(column)：表中的字段，存储信息。

4· 行(row)：表中的记录。

5· 主键(primary key)：一列，唯一标识表中的一行
> 满足以下条件：
1· 任意两行有不同的主键值
2· 每一行都必须有一个主键值(不可为NULL)
3· 主键列中值不可修改和更新
4· 主键值不可重用

### SELECT 
<h3 id="1">1.SELECT查询</h3>

```
SELECT column FROM  table;
```
<h3 id="2">2.检索不同值</h3>

```
SELECT DISTINCT column FROM table; 
```
<h3 id="3">3.限制结果</h3> 

```
1· SELECT TOP 5 column FROM table;
2· SELCET column FROM table LIMIT 5 OFFSET 5;
-- 表示返回5行开始5行数据
```
<h3 id="4">4.排序检索：默认升序排序</h3> 

```
1· SELECT column FROM table ORDER BY column;
-- 应保证有此关键字时ORDER BY 在SELCET 子句最后
2· SELECT column FROM table ORDER BY 2，3;
-- 按列位置排序
3· SELECT column FROM table ORDER BY column DESC(ASC);
-- 指定排序顺序，如果对多个列排序要在每个列前加DESC(ASC)
```
<h3 id="5">5.过滤数据</h3> 

```
1· SELECT column FROM table WHERE column+条件;
2· SELECT column FROM table WHERE column BETWEEN 条件 AND 条件;-- BETWEEN AND是WHERE的子句，为范围查询
3· 如果指定过滤不包含的值，那么NULL是不会查询到的，因为NULL是未知数空
4· SELECT column FROM table WHERE column IN ( , );
 -- 范围查询，注意字符值与数值不同
```
> IN最大的优点就是可以包含其他的SELECT语句形成**子查询语句**，能动态建立WHERE子句。
<h3 id="6">6.通配符：实际是WHERE中子句的特殊含义字符，子句必须包含LIKE操作符)</h3> 

```
1· SELECT column FROM table WHERE column LIKE 'F%'; 
-- 表示匹配任何以F开头字符，&可以匹配0、1、多个字符(NULL不会匹配)
2· SELECT column FROM table WHERE column LIKE 'F_';
 --下划线_ 值匹配单个字符
3· SELECT column FROM table WHERE column LIKE 'F[JM]';
 -- 只匹配[]字符集里面的单个字符
```
<h3 id="7">7.计算字段：在运行时SELCET语句内创建，一般存储在数据库中的数据不是要的数据格式，所以需要计算字段的形式查询显示，拼接(concatenate)</h3> 

```
1· SELCET Concat(TRIM(column),'(',TRIM(column),')') FROM column AS 别名; 
-- 将以计算字段AS后面的别名方式返回查询结果给客户端，其中LTRIM（）和RTRIM（）函数分别表示去掉column的左和右空格，还有TRIM（）
2· SELCET column + column FROM table AS 别名;
-- 其中+可为*、/、- 
```
<h3 id="8">8.数值处理函数</h3> 

```
ABS（）：绝对值
COS（）：余弦值
EXP（）：指数值
PI（）：返回圆周率
SQRT（）:平方值
......
```
<h3 id="9">9.聚集函数</h3> 

```
SUM（）：和
AVG（）：平均值
MAX（）：最大值
COUNT（）：某列的行数
......
-- 其中AVG()只能用于单列，多列需要多个AVG(),忽略NULL行
-- COUNT（*）时不会忽略NULL行

SELECT AVG(DISTINCT column) AS 别名;
-- 使用DISTINCT指定去掉重复值,后面必须跟列名
```
<h3 id="10">10.分组数据：SELECT子句：HAVING、GROUP BY</h3> 

```
1· GROUP BY 在WHERE 后，在ORDER BY 前，可以任意数目列分组和嵌套分组，GROUP BY 后面子句的列必须包含所有SELECT后面的检索列和有效表达式（如果是表达式，则以同样表达式，不能是聚集函数），GROUP BY子句后面不能跟变长数据类型。其中NULL为一组

2·过滤分组：WHERE只能过滤行，HAVING和WHERE类似，但是用来过滤分组，所以HAVING可以代替所有WHERE，在GROUP BY 后

SELECT column FROM table GROUP BY column HAVING 条件

-- WHERE和HAVING的区别：
WHERE过滤在分组前，HAVING过滤在分组后
HAVING过滤后的顺序不一定是分组顺序，所以最好加ORDER BY
```
<h3 id="11">11.子查询：嵌套SELECT，使用IN实现</h3> 

```
SELECT column FROM row WHERE column IN (SELECT column FROM table WHERE column 条件)
-- 作为子查询的SELECT语句只能查询单列，查询多列会报错
```
<h3 id="12">12.联结表（join）：在关系型数据库中伸缩性好,联结不是实体，只在查询执行期间存在</h3> 

```
SELECT column1,column2,column3  FROM table1,table2 WHERE table1.column = table2.column;
-- 如果没有联结，那查询结果为笛卡尔积

内联结（等值联结）：column INNER JOIN column ON 条件
SELECT column1,column2,column3  FROM table1 INNER JOIN table2 ON table1.column = table2.column;
```
<h3 id="13">13.高级联结(SQL可以对列名、表名、计算字段取别名)</h3> 

```
- 联结
    - 自联结：使用表别名对同一表进行等值联结，因为使用表别名，所以实际是两个表。
    可以使用自联结代替子查询，因为效率比较高。
    自联结的结果是包含重复查询的。

    - 自然联结：是特殊的等值联结，去除了重复查询，需要自己指定自然联结，一般使用通配符（*）对一个表使用，而其他表指定列。
    SELECT table1.*,table2.column2,table2.column3  FROM table1,table2 WHERE table1.column = table2.column;

    - 外联结：包含了相关表中没有关联行的行，使用FROM table1 LEFT(RIGHT) OUTER JOIN table2 ON 条件；  （或者是全外联结：FULL OUTER JOIN）
```
<h3 id="14">14.组合查询（UNION操作符合并查询结果）</h3> 

```
-- 直接在多个SELECT语句间使用，结果将去掉重复的查询（可以使用UNION ALL ）
-- SELECT语句间必须包含相同的列、表达式或聚集函数，顺序可以不同，列的数据类型必须兼容。
-- 只能使用一个ORDER BY ,且在组合语句最后
```
<h3 id="15">15.插入数据（INSERT语句）：INSERT一般只插入一行，但INSERT INTO SELECT 除外</h3> 

```
1· INSERT INTO table
   VALUES(值)；
-- VALUES插入的顺序为表的顺序

2· INSERT INTO table(列名)
   VALUES(值)
-- 此时VALUES插入值顺序为table中列名顺序
如果省略列：（必须满足以下条件）
    -- 该列允许NULL；
    -- 该列在表定义中有默认值；

3· INSERT INTO table1
   SELECT column
   FROM table2;
-- 插入检索的数据

4· SELECT column
   INTO tablenew
   FROM table1；
   或
   CREATE TABLE tablenew AS
   SELECT column FROM table1；
-- 用于将一个表复制到新的表。与INSERT INTO SELECT不同
```
<h3 id="16">16.更新和删除数据(UPDATE、DELETE语句)：UPDATE可以删除列，而DELETE删除行还有不能删除表</h3> 

> 删除所有行可以使用TRUNCATE TABLE；DELETE操作前最好建立外键；
```
1· UPDATE table
   SET column = 值
   WHERE column = 条件；（或者 WHERE column IN 条件；）

2· DELETE FROM table
   WHERE column = 条件；
```
<h3 id="17">17.创建和操作表（需要删除了旧表才可创建同名表）:CREAT TABLE && ALTER TABLE</h3> 

```
- CREATE TABLE必须满足：
1· 新表的名字，在TABLE后；
2· 表列的名字和定义，用逗号分隔；
3· 有些DBMS要求指定表位置
CREATE TABLE table
(
   column1  CHAR(10)       NOT NULL,
   column2  DECIMAL(8,2)   NOT NULL,
   column3  TEXT(100)                   DEFAULT  'NO'，   
   -- 指定默认值'NO' 
   column4  INTEGER        NOT NULL
); --创建表

- ALTER TABLE必须满足：
1· 后面跟需要更改的表名
2· 列出需要做的操作
ALTER TABLE table
ADD column CHAR(20); -- 添加列

ALTER TABLE table
DROP COLUMN column ; --删除列

- DROP TABLE:
DROP TABLE table；-- 删除表
```
<h3 id="18">18.视图:是虚拟的表，只包含了使用时动态检索数据的查询，不包含任何数据，包含的是查询，因为是动态的，所以返回的是原始数据动态的查询。</h3> 

> 和表一样必须有唯一命名；不能索引，所以不能关联触发器或默认值；
```
- CREATE VIEW viewname AS
......；
-- 创建视图

- DROP VIEW viewname；
-- 删除视图
```
<h3 id="19">19.存储过程：能复合多个SQL语句，视为批文件</h3> 

```
- EXECUTE procedure(参数=值 或 按次序给参数)
- CREATE procedure：具体操作看具体DBMS文档
```
<h3 id="20">20.管理事务处理：COMMIT && ROLLBACK,确保成批SQL操作要么完全执行，要么完全不执行（具体看相关DBMS）</h3> 

```
- 术语：
   - 事务(transation):指一组SQL语句
   - 回退(rollback):指撤回指定SQL语句过程
   - 提交(commit):指将未存储SQL语句结果写入数据表
   - 保留点(savepoint):指事务处理中设置的临时占位符(placeholder)
事务处理用来管理INSERT 、UPDATE、DELETE语句，不能回退SELECT语句，也不能回退CREATE、DROP操作

- 控制事务管理：具体看具体DBMS文档

- 使用ROLLBACK：用来回退SQL语句

- 使用COMMIT:确保COMMIT事务处理中批语句都完全执行才执行该事务

- SAVEPOINT语句：
SAVEPOINT transation；
ROLLBACK TO transation；
```
<h3 id="21">21.游标：是一个存储在DBMS服务器上的数据库查询（结果集）</h3> 

```
- 使用游标前必须声明定义它，这个过程实际没有检索数据
- 一旦声明，就必须打开游标使用
- 在结束游标时，必须关闭游标或释放游标

DECLEAR cursorname CURSOR
FOR
SELECT .....
-- 定义游标

OPEN cursorname；
-- 打开游标

FETCH ：使用游标，具体看相关DBMS文档；

CLOSE cursorname；
-- 关闭游标
```
<h3 id="22">22.高级SQL特性：约束（CONSTRAINT）、索引、触发器</h3> 

```
ALTER TABLE tableneme
ADD CONSTRAINT PRIMARY KEY (columnname);
-- 添加主键约束

ALTER TABLE tablename1
ADD CONSTRAINT
FOREING KEY (column1) REFERENCES tablename2 (column2);
-- 添加外键约束

唯一约束：保证一列或一组列中数据是唯一的，类似主键但有以下区别：
可以使用UNIQUE或CONSTRAINT定义：
   - 表可以包含多个唯一约束，但每个表主键只能有一个
   - 唯一约束可以包含NULL
   - 唯一约束可以修改和更新
   - 唯一约束可以重复使用
   - 与主键不一样，唯一约束不可以用来定义外键

检查约束：
CREATE TABLE table
(
   column1  CHAR(10)       NOT NULL,
   column2  DECIMAL(8,2)   NOT NULL,
   column3  TEXT(100)      NOT NULL  CHECK (条件)，          
   column4  INTEGER        NOT NULL
); 
或
ADD CONSTRAINT CHECK(条件)；
-- 检查约束
```
```
索引：作用是类似于字典，方便数据的查询和排序，存储在DBMS服务器中
CREATE INDEX index_name
ON table_name (column);
-- 索引名必须唯一 即index_name。
```
```
触发器：与单个表关联，是特殊的存储过程，在数据库活动发生时自动执行，可以和INSERT、UPDATE、DELETE操作关联
具体操作看相关DBMS文档
```