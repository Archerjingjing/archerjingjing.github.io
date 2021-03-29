---
title: 数据库-MySQL
date: 2020-02-19 12:54:21
tags: SQL数据库
categories: 学习路程
---

#### [菜鸟教程-MySQL](https://www.runoob.com/mysql/mysql-data-types.html)
#### [朱双印-MySQL](http://www.zsythink.net/archives/category/%e5%ad%98%e5%82%a8/mysql/page/4/)


* [1.一文了解InnoDB引擎](#1)
* [2.SQL中JOIN中ON和WHERE的区别](#2)

<h3 id="1">1.一文了解InnoDB引擎</h3>

* [文章链接](https://www.jianshu.com/p/519fd7747137)

<h3 id="2">2.检索不同SQL中JOIN中ON和WHERE的区别（）</h3>

* [文章链接](https://www.runoob.com/w3cnote/sql-join-the-different-of-on-and-where.html)

<h3 id="3">3.关于分页查询的效率问题</h3>

* 子查询是在索引上完成的，而普通的查询时在数据文件上完成的，通常来说，索引文件要比数据文件小得多，所以操作起来也会更有效率。
实际可以利用类似策略模式的方式去处理分页，比如判断如果是一百页以内，就使用最基本的分页方式，大于一百页，则使用子查询的分页方式。

<h3 id="4">4.delete，drop，truncate的区别</h3>

* delete 和 truncate 仅仅删除表数据，drop 连表数据和表结构一起删除，打个比方，delete 是单杀，truncate 是团灭，drop 是把电脑摔了。
* delete 是**DML** 语句，操作完以后如果没有不想提交事务还可以回滚，truncate 和 drop 是 **DDL** 语句，操作完马上生效，不能回滚，打个比方，delete 是发微信说分手，后悔还可以撤回，truncate 和 drop 是直接扇耳光说滚，不能反悔。
* 执行的速度上，drop>truncate>delete，打个比方，drop 是神舟火箭，truncate 是和谐号动车，delete 是自行车。

<h3 id="5">5.MySQL中正则表达式语法(REGEXP)</h3>

* [文章链接](https://www.runoob.com/mysql/mysql-regexp.html)

<h3 id="6">6.关于索引的一些问题</h3>

* 索引是一种特殊的文件（InnoDB 数据表上的索引是表空间的一个组成部分），它们包含着对数据表里所有记录的引用指针。索引不是万能的，索引可以加快数据检索操作，但会使数据修改操作变慢。每修改数据记录，索引就必须刷新一次。为了在某种程度上弥补这一缺陷，许多 SQL 命令都有一个 DELAY_KEY_WRITE 项。这个选项的作用是暂时制止 MySQL 在该命令每插入一条新记录和每修改一条现有之后立刻对索引进行刷新，对索引的刷新将等到全部记录插入/修改完毕之后再进行。在需要把许多新记录插入某个数据表的场合，DELAY_KEY_WRITE 选项的作用将非常明显。另外，索引还会在硬盘上占用相当大的空间。因此应该只为最经常查询和最经常排序的数据列建立索引。注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。


从理论上讲，完全可以为数据表里的每个字段分别建一个索引，但 MySQL 把同一个数据表里的索引总数限制为16个。

1. InnoDB 数据表的索引
    
    与 InnoDB数据表相比，在 InnoDB 数据表上，索引对 InnoDB 数据表的重要性要大得多。在 InnoDB 数据表上，索引不仅会在搜索数据记录时发挥作用，还是数据行级锁定机制的基础。“数据行级锁定”的意思是指在事务操作的执行过程中锁定正在被处理的个别记录，不让其他用户进行访问。这种锁定将影响到（但不限于）SELECT、LOCKINSHAREMODE、SELECT、FORUPDATE 命令以及 INSERT、UPDATE 和 DELETE 命令。出于效率方面的考虑，InnoDB 数据表的数据行级锁定实际发生在它们的索引上，而不是数据表自身上。显然，数据行级锁定机制只有在有关的数据表有一个合适的索引可供锁定的时候才能发挥效力。

2. 限制

        如果 WHERE 子句的查询条件里有不等号（WHERE coloum !=），MySQL 将无法使用索引。类似地，如果 WHERE 子句的查询条件里使用了函数（WHERE DAY（column）=），MySQL 也将无法使用索引。在 JOIN 操作中（需要从多个数据表提取数据时），MySQL 只有在主键和外键的数据类型相同时才能使用索引。

        如果 WHERE 子句的查询条件里使用比较操作符 LIKE 和 REGEXP，MySQL 只有在搜索模板的第一个字符不是通配符的情况下才能使用索引。比如说，如果查询条件是 LIKE 'abc%‘，MySQL 将使用索引；如果查询条件是 LIKE '%abc’，MySQL 将不使用索引。

        在 ORDER BY 操作中，MySQL 只有在排序条件不是一个查询条件表达式的情况下才使用索引。（虽然如此，在涉及多个数据表查询里，即使有索引可用，那些索引在加快 ORDER BY 方面也没什么作用）。如果某个数据列里包含许多重复的值，就算为它建立了索引也不会有很好的效果。比如说，如果某个数据列里包含的净是些诸如 “0/1” 或 “Y/N” 等值，就没有必要为它创建一个索引。

<h3 id="7">7.ALTER子句：CHANGE、MODIFY、ALTER、RENAME TO</h3>
```
ALTER TABLE testalter_tbl MODIFY c CHAR(10);
ALTER TABLE testalter_tbl CHANGE i j BIGINT;
ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;
ALTER TABLE testalter_tbl RENAME TO alter_tbl;
```

<h3 id="8">8.可以用查询的方法创建临时表</h3>
```
CREATE TEMPORARY TABLE 临时表名 AS
(
    SELECT *  FROM 旧的表名
    LIMIT 0,10000
);
```
<h3 id="9">9.防止表中出现重复数据</h3>

* 你可以在 MySQL 数据表中设置指定的字段为 PRIMARY KEY（主键） 或者 UNIQUE（唯一） 索引来保证数据的唯一性。
* INSERT IGNORE INTO 与 INSERT INTO 的区别就是 INSERT IGNORE 会忽略数据库中已经存在的数据，如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据。这样就可以保留数据库中已经存在数据，达到在间隙中插入数据的目的。
* INSERT IGNORE INTO 当插入数据时，在设置了记录的唯一性后，如果插入重复数据，将不返回错误，只以警告形式返回。 而 REPLACE INTO 如果存在 primary 或 unique 相同的记录，则先删除掉。再插入新记录。

另一种设置数据的唯一性方法是添加一个 UNIQUE 索引

<h3 id="10">10.MySQL 导出数据/导入数据</h3>

* [文章链接-导出数据](https://www.runoob.com/mysql/mysql-database-export.html)
* [文章链接-导入数据](https://www.runoob.com/mysql/mysql-database-import.html)


