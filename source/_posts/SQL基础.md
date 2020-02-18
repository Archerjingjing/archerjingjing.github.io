---
title: 《SQL必知必会第四版》笔记
date: 2020-02-18 18:22:15
tags: SQL数据库
categories: 学习路程
---
# SQL:Structured Query Language(结构化查询语言)
## 以下代码皆以MySQL例
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

## SELECT 
1· 
```
SELECT column FROM  table;
```
2·检索不同值：
```
SELECT DISTINCT column FROM table; 
```
3· 限制结果： 
```
1· SELECT TOP 5 column FROM table;
2· SELCET column FROM table LIMIT 5 OFFSET 5;
-- 表示返回5行开始5行数据
```
4· 排序检索：默认升序排序
```
1· SELECT column FROM table ORDER BY column;
-- 应保证有此关键字时ORDER BY 在SELCET 子句最后
2· SELECT column FROM table ORDER BY 2，3;
-- 按列位置排序
3· SELECT column FROM table ORDER BY column DESC(ASC);
-- 指定排序顺序，如果对多个列排序要在每个列前加DESC(ASC)
```
5· 过滤数据
```
1· SELECT column FROM table WHERE column+条件;
2· SELECT column FROM table WHERE column BETWEEN 条件 AND 条件;-- BETWEEN AND是WHERE的子句，为范围查询
3· 如果指定过滤不包含的值，那么NULL是不会查询到的，因为NULL是未知数空
4· SELECT column FROM table WHERE column IN ( , );
 -- 范围查询，注意字符值与数值不同
```
> IN最大的优点就是可以包含其他的SELECT语句形成**子查询语句**，能动态建立WHERE子句。

6· 通配符：实际是WHERE中子句的特殊含义字符，子句必须包含LIKE操作符)
```
1· SELECT column FROM table WHERE column LIKE 'F%'; 
-- 表示匹配任何以F开头字符，&可以匹配0、1、多个字符(NULL不会匹配)
2· SELECT column FROM table WHERE column LIKE 'F_';
 --下划线_ 值匹配单个字符
3· SELECT column FROM table WHERE column LIKE 'F[JM]';
 -- 只匹配[]字符集里面的单个字符
```
7· 计算字段：在运行时SELCET语句内创建，一般存储在数据库中的数据不是要的数据格式，所以需要计算字段的形式查询显示，拼接(concatenate)
```
1· SELCET Concat(TRIM(column),'(',TRIM(column),')') FROM column AS 别名; 
-- 将以计算字段AS后面的别名方式返回查询结果给客户端，其中LTRIM（）和RTRIM（）函数分别表示去掉column的左和右空格，还有TRIM（）
2· SELCET column + column FROM table AS 别名;
-- 其中+可为*、/、- 
```
8· 数值处理函数
```
ABS（）：绝对值
COS（）：余弦值
EXP（）：指数值
PI（）：返回圆周率
SQRT（）:平方值
......
```
9· 聚集函数
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
10· 分组数据：SELECT子句：HAVING、GROUP BY
```
1· GROUP BY 在WHERE 后，在ORDER BY 前，可以任意数目列分组和嵌套分组，GROUP BY 后面子句的列必须包含所有SELECT后面的检索列和有效表达式（如果是表达式，则以同样表达式，不能是聚集函数），GROUP BY子句后面不能跟变长数据类型。其中NULL为一组

2·过滤分组：WHERE只能过滤行，HAVING和WHERE类似，但是用来过滤分组，所以HAVING可以代替所有WHERE，在GROUP BY 后

SELECT column FROM table GROUP BY column HAVING 条件

-- WHERE和HAVING的区别：
WHERE过滤在分组前，HAVING过滤在分组后
HAVING过滤后的顺序不一定是分组顺序，所以最好加ORDER BY
```
11· 子查询：嵌套SELECT，使用IN实现
```
SELECT column FROM row WHERE column IN (SELECT column FROM table WHERE column 条件)
-- 作为子查询的SELECT语句只能查询单列，查询多列会报错
```
12· 联结表（join）：在关系型数据库中伸缩性好,联结不是实体，只在查询执行期间存在
```
SELECT column1,column2,column3  FROM table1,table2 WHERE table1.column = table2.column;
-- 如果没有联结，那查询结果为笛卡尔积

内联结（等值联结）：column INNER JOIN column ON 条件
SELECT column1,column2,column3  FROM table1 INNER JOIN table2 ON table1.column = table2.column;
```
13· 高级联结(SQL可以对列名、表名、计算字段取别名)
```
- 联结
    - 自联结：使用表别名对同一表进行等值联结，因为使用表别名，所以实际是两个表。
    可以使用自联结代替子查询，因为效率比较高。
    自联结的结果是包含重复查询的。

    - 自然联结：是特殊的等值联结，去除了重复查询，需要自己指定自然联结，一般使用通配符（*）对一个表使用，而其他表指定列。
    SELECT table1.*,table2.column2,table2.column3  FROM table1,table2 WHERE table1.column = table2.column;

    - 外联结：包含了相关表中没有关联行的行，使用FROM table1 LEFT(RIGHT) OUTER JOIN table2 ON 条件；  （或者是全外联结：FULL OUTER JOIN）
```
14· 组合查询（UNION操作符合并查询结果）
```
-- 直接在多个SELECT语句间使用，结果将去掉重复的查询（可以使用UNION ALL ）
-- SELECT语句间必须包含相同的列、表达式或聚集函数，顺序可以不同，列的数据类型必须兼容。
-- 只能使用一个ORDER BY ,且在组合语句最后
```
15· 插入数据（INSERT语句）：INSERT一般只插入一行，但INSERT INTO SELECT 除外
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
16· 更新和删除数据(UPDATE、DELETE语句)：UPDATE可以删除列，而DELETE删除行还有不能删除表
> 删除所有行可以使用TRUNCATE TABLE；DELETE操作前最好建立外键；
```
1· UPDATE table
   SET column = 值
   WHERE column = 条件；（或者 WHERE column IN 条件；）

2· DELETE FROM table
   WHERE column = 条件；
```
17· 创建和操作表
```

```