# 第三章 关系数据库标准语言 SQL：重要知识点总结

> 来源：《数据库系统概论（第6版）》第3章。适合配合上机练习复习 SQL 的数据定义、查询、更新、空值和视图。

## 1. 本章学习主线

SQL 是关系数据库的标准语言，本章的核心是掌握 SQL 的基本功能和常用语句。

本章主要内容：

1. SQL 的产生、发展、特点和基本概念。
2. 数据定义：模式、基本表、索引、数据字典。
3. 数据查询：SELECT 语句及各种查询形式。
4. 数据更新：INSERT、UPDATE、DELETE。
5. 空值 NULL 的含义和处理。
6. 视图的定义、查询、更新和作用。

核心记忆：

```text
SQL = 数据定义 + 数据查询 + 数据操纵 + 数据控制
```

常用动词：

| 功能 | 动词 |
| --- | --- |
| 数据定义 | CREATE、DROP、ALTER |
| 数据查询 | SELECT |
| 数据操纵 | INSERT、UPDATE、DELETE |
| 数据控制 | GRANT、REVOKE |

## 2. SQL 概述

### 2.1 SQL 的特点

SQL 具有以下特点：

- 综合统一：集 DDL、DML、DCL 于一体。
- 高度非过程化：用户只需说明“做什么”，不必说明“怎么做”。
- 面向集合操作：操作对象和结果都可以是元组集合。
- 既是独立语言，也是嵌入式语言。
- 语言简洁，易学易用。

### 2.2 SQL 对三级模式的支持

SQL 支持数据库系统的三级模式结构：

- 外模式：对应视图。
- 模式：对应基本表。
- 内模式：对应存储文件。

关系数据库中常见对象：

- 基本表：实际独立存储的表。
- 视图：由基本表或其他视图导出的虚表。
- 存储文件：数据库内部物理存储结构。

## 3. 数据定义

SQL 的数据定义功能主要包括：

- 定义和删除模式。
- 定义、修改和删除基本表。
- 定义和删除索引。

### 3.1 模式的定义与删除

定义模式：

```sql
CREATE SCHEMA <模式名> AUTHORIZATION <用户名>;
```

如果没有指定模式名，模式名通常隐含为用户名。

删除模式：

```sql
DROP SCHEMA <模式名> CASCADE;
DROP SCHEMA <模式名> RESTRICT;
```

区别：

- `CASCADE`：级联删除该模式下的相关对象。
- `RESTRICT`：若模式中已有下属对象，则拒绝删除。

### 3.2 基本表的定义

定义基本表：

```sql
CREATE TABLE <表名> (
    <列名> <数据类型> [列级完整性约束],
    <列名> <数据类型> [列级完整性约束],
    ...
    [表级完整性约束]
);
```

常见完整性约束：

```sql
PRIMARY KEY
FOREIGN KEY REFERENCES
UNIQUE
NOT NULL
CHECK
```

例子：

```sql
CREATE TABLE Student (
    Sno CHAR(8) PRIMARY KEY,
    Sname VARCHAR(20) UNIQUE,
    Ssex CHAR(1),
    Sbirthdate DATE,
    Smajor VARCHAR(20)
);
```

### 3.3 SQL 常用数据类型

| 类型 | 含义 |
| --- | --- |
| CHAR(n) | 固定长度字符串 |
| VARCHAR(n) | 最大长度为 n 的变长字符串 |
| CLOB | 字符串大对象 |
| BLOB | 二进制大对象 |
| INT / INTEGER | 整数 |
| SMALLINT | 短整数 |
| BIGINT | 大整数 |
| NUMERIC(p,d) | 定点数，总 p 位，小数 d 位 |
| DECIMAL(p,d) | 定点数 |
| REAL | 单精度浮点数 |
| DOUBLE PRECISION | 双精度浮点数 |
| FLOAT(n) | 可选精度浮点数 |
| BOOLEAN | 逻辑布尔量 |
| DATE | 日期 |
| TIME | 时间 |
| TIMESTAMP | 时间戳 |
| INTERVAL | 时间间隔 |

### 3.4 修改基本表

修改基本表使用 `ALTER TABLE`。

常见格式：

```sql
ALTER TABLE <表名>
ADD <列名> <数据类型> [完整性约束];

ALTER TABLE <表名>
DROP COLUMN <列名> CASCADE;

ALTER TABLE <表名>
DROP COLUMN <列名> RESTRICT;

ALTER TABLE <表名>
ADD CONSTRAINT <约束名> <约束条件>;

ALTER TABLE <表名>
DROP CONSTRAINT <约束名>;

ALTER TABLE <表名>
ALTER COLUMN <列名> TYPE <数据类型>;
```

### 3.5 删除基本表

```sql
DROP TABLE <表名> CASCADE;
DROP TABLE <表名> RESTRICT;
```

区别：

- `RESTRICT`：若该表被其他对象引用，则不能删除。
- `CASCADE`：删除该表的同时级联删除依赖它的对象。

### 3.6 索引

索引用于提高查询效率，但会增加更新维护成本。

建立索引：

```sql
CREATE [UNIQUE] [CLUSTER] INDEX <索引名>
ON <表名> (<列名> [ASC|DESC], ...);
```

修改索引名：

```sql
ALTER INDEX <旧索引名> RENAME TO <新索引名>;
```

删除索引：

```sql
DROP INDEX <索引名>;
```

要点：

- `UNIQUE` 表示唯一索引。
- `CLUSTER` 表示聚簇索引。
- 索引属于内模式范畴，一般由系统使用和维护。
- 查询频繁而更新较少的列适合建索引。

### 3.7 数据字典

数据字典是 DBMS 内部的一组系统表，记录数据库中的定义信息。

包括：

- 关系模式定义。
- 视图定义。
- 索引定义。
- 完整性约束定义。
- 用户权限。
- 统计信息。

数据字典是 DBMS 执行 SQL、进行查询优化和权限检查的重要依据。

## 4. 数据查询

### 4.1 SELECT 语句基本格式

```sql
SELECT [ALL | DISTINCT] <目标列表达式> [, <目标列表达式>] ...
FROM <表名或视图名> [, <表名或视图名>] ...
[WHERE <条件表达式>]
[GROUP BY <列名> [HAVING <条件表达式>]]
[ORDER BY <列名> [ASC | DESC]]
[LIMIT <行数> [OFFSET <行数>]];
```

执行含义：

1. 从 `FROM` 指定的表或视图中取数据。
2. 用 `WHERE` 筛选满足条件的元组。
3. 用 `GROUP BY` 分组。
4. 用 `HAVING` 筛选分组。
5. 用 `SELECT` 产生目标列。
6. 用 `ORDER BY` 排序。

### 4.2 单表查询

查询指定列：

```sql
SELECT Sno, Sname
FROM Student;
```

查询全部列：

```sql
SELECT *
FROM Student;
```

去重：

```sql
SELECT DISTINCT Smajor
FROM Student;
```

查询经过计算的值：

```sql
SELECT Sname, 2026 - EXTRACT(YEAR FROM Sbirthdate) AS Age
FROM Student;
```

### 4.3 WHERE 条件查询

常见条件：

| 条件 | 含义 |
| --- | --- |
| `=`、`>`、`<`、`>=`、`<=`、`<>` | 比较 |
| `AND`、`OR`、`NOT` | 逻辑连接 |
| `BETWEEN ... AND ...` | 范围 |
| `IN (...)` | 集合 |
| `LIKE` | 字符串匹配 |
| `IS NULL` | 空值判断 |
| `EXISTS` | 存在性判断 |

### 4.4 LIKE 字符串匹配

常用通配符：

- `%`：匹配任意长度字符串。
- `_`：匹配任意单个字符。

例子：

```sql
SELECT Sno, Sname
FROM Student
WHERE Sno LIKE '2018%';
```

如果要把 `%` 或 `_` 当作普通字符，需要使用 `ESCAPE`。

```sql
SELECT Cno, Ccredit
FROM Course
WHERE Cname LIKE 'DB\_Design' ESCAPE '\';
```

### 4.5 ORDER BY

排序：

```sql
SELECT Sno, Grade
FROM SC
WHERE Cno = '81002'
ORDER BY Grade DESC;
```

要点：

- `ASC`：升序，默认。
- `DESC`：降序。

### 4.6 聚集函数

常见聚集函数：

| 函数 | 含义 |
| --- | --- |
| COUNT(*) | 统计元组个数 |
| COUNT(列名) | 统计该列非空值个数 |
| SUM | 求和 |
| AVG | 平均值 |
| MAX | 最大值 |
| MIN | 最小值 |

注意：

- 除 `COUNT(*)` 外，聚集函数通常忽略 NULL。
- 聚集函数常与 `GROUP BY` 配合使用。

### 4.7 GROUP BY 与 HAVING

分组查询：

```sql
SELECT Cno, COUNT(*) AS Num
FROM SC
GROUP BY Cno;
```

分组后筛选：

```sql
SELECT Cno, AVG(Grade) AS AvgGrade
FROM SC
GROUP BY Cno
HAVING AVG(Grade) >= 80;
```

区别：

- `WHERE`：对元组筛选，分组前执行。
- `HAVING`：对分组筛选，分组后执行。

### 4.8 连接查询

等值连接：

```sql
SELECT Student.Sno, Sname, Grade
FROM Student, SC
WHERE Student.Sno = SC.Sno;
```

自身连接：

```sql
SELECT FIRST.Cno, SECOND.Cpno
FROM Course FIRST, Course SECOND
WHERE FIRST.Cpno = SECOND.Cno;
```

要点：

- 多表查询时，若列名可能冲突，应使用表名前缀或别名。
- 自身连接必须给同一张表起不同别名。

### 4.9 嵌套查询

嵌套查询是把一个 SELECT 查询块嵌入另一个查询块。

常见形式：

```sql
SELECT ...
FROM ...
WHERE <列名> IN (
    SELECT ...
    FROM ...
    WHERE ...
);
```

不相关子查询：

- 子查询不依赖父查询。
- 通常可以先执行子查询，再执行外层查询。

相关子查询：

- 子查询依赖父查询中的当前元组。
- 子查询需要随外层元组反复执行。

### 4.10 EXISTS 查询

`EXISTS` 判断子查询结果是否非空。

```sql
SELECT Sname
FROM Student
WHERE EXISTS (
    SELECT *
    FROM SC
    WHERE SC.Sno = Student.Sno
);
```

要点：

- `EXISTS` 只关心是否存在结果，不关心具体列值。
- `NOT EXISTS` 常用于表达“不存在”或“全部”类查询。

### 4.11 集合查询

SQL 支持集合操作：

```sql
UNION
INTERSECT
EXCEPT
```

含义：

- `UNION`：并。
- `INTERSECT`：交。
- `EXCEPT`：差。

注意：参加集合操作的查询结果通常要求列数相同，对应列类型兼容。

## 5. 数据更新

### 5.1 插入数据

插入单个元组：

```sql
INSERT INTO <表名> [(<属性列1>, <属性列2>, ...)]
VALUES (<常量1>, <常量2>, ...);
```

如果省略属性列名，则 `VALUES` 必须按表定义的属性顺序给出所有列值。

插入子查询结果：

```sql
INSERT INTO <表名> [(<属性列1>, <属性列2>, ...)]
SELECT ...
FROM ...
WHERE ...;
```

### 5.2 修改数据

```sql
UPDATE <表名>
SET <列名> = <表达式> [, <列名> = <表达式>] ...
[WHERE <条件>];
```

注意：

- 如果省略 `WHERE`，会修改表中所有元组。
- `SET` 中可以使用表达式，如 `Grade = Grade - 5`。
- 子查询可以嵌套在 `UPDATE` 中作为修改条件。

### 5.3 删除数据

```sql
DELETE FROM <表名>
[WHERE <条件>];
```

注意：

- 如果省略 `WHERE`，会删除表中所有元组。
- `DELETE` 删除的是表中的数据，不删除表定义。
- 删除操作可能破坏参照完整性，需要 DBMS 检查和控制。

## 6. 空值 NULL

### 6.1 空值的含义

NULL 表示“不知道”“不存在”或“无意义”的值。

常见产生场景：

- 应该有值，但暂时不知道。
- 不应该有值。
- 不便填写或暂时缺失。

NULL 不是 0，也不是空字符串。

### 6.2 空值判断

判断空值必须使用：

```sql
IS NULL
IS NOT NULL
```

不能使用：

```sql
= NULL
<> NULL
```

### 6.3 空值与聚集函数

要点：

- `COUNT(*)` 统计所有元组。
- `COUNT(列名)` 只统计该列非 NULL 的元组。
- `SUM`、`AVG`、`MAX`、`MIN` 通常忽略 NULL。

### 6.4 空值与逻辑运算

涉及 NULL 的比较结果可能是 UNKNOWN。

SQL 的逻辑结果不只是 TRUE 和 FALSE，还可能是 UNKNOWN。复合条件中要特别注意 NULL 对查询结果的影响。

## 7. 视图

### 7.1 视图的概念

视图是从一个或多个基本表或视图中导出的表。

特点：

- 视图是虚表。
- 数据库中只存储视图定义，一般不存储视图对应的数据。
- 视图数据来自基本表。

### 7.2 定义视图

```sql
CREATE VIEW <视图名> [(<列名>, <列名>, ...)]
AS
SELECT ...
FROM ...
WHERE ...
[WITH CHECK OPTION];
```

若视图中包含聚集函数、表达式或分组查询，通常需要明确给出视图属性列名。

### 7.3 删除视图

```sql
DROP VIEW <视图名>;
DROP VIEW <视图名> CASCADE;
```

说明：

- 普通删除只删除指定视图定义。
- `CASCADE` 会级联删除由该视图导出的其他视图。
- 删除基本表后，由它导出的视图不能再使用，但视图定义未必自动删除。

### 7.4 查询视图

视图定义后，用户可以像查询基本表一样查询视图。

```sql
SELECT Sno, Sbirthdate
FROM F_Student
WHERE Sbirthdate >= DATE '2004-01-01';
```

DBMS 通常会把对视图的查询转换为对基本表的查询。

### 7.5 更新视图

并不是所有视图都可更新。

通常较容易更新的视图：

- 来自单个基本表。
- 不含聚集函数。
- 不含分组。
- 不含 DISTINCT。
- 不含复杂表达式。

不可更新或更新受限的视图：

- 分组视图。
- 连接视图。
- 含聚集函数的视图。
- 含计算列的视图。

### 7.6 视图的作用

视图的主要作用：

- 简化用户操作。
- 使用户从多角度看待同一数据。
- 对重构数据库提供一定逻辑独立性。
- 提供安全保护，只暴露用户需要的数据。
- 更清晰地表达查询。

## 8. 易混概念辨析

### 8.1 WHERE 与 HAVING

| 项目 | WHERE | HAVING |
| --- | --- | --- |
| 作用对象 | 元组 | 分组 |
| 执行时机 | 分组前 | 分组后 |
| 是否常与聚集函数配合 | 一般不直接用聚集函数筛选组 | 常用于聚集函数条件 |

### 8.2 DELETE、DROP、TRUNCATE 思路辨析

本章重点是 `DELETE` 和 `DROP`：

- `DELETE`：删除表中元组，表结构仍在。
- `DROP TABLE`：删除表定义及相关对象。
- `DROP VIEW`：删除视图定义。
- `DROP INDEX`：删除索引。

### 8.3 基本表与视图

| 概念 | 是否存储数据 | 来源 |
| --- | --- | --- |
| 基本表 | 存储实际数据 | 独立定义 |
| 视图 | 通常不存储实际数据 | 由基本表或视图导出 |

### 8.4 NULL 与普通值

| 写法 | 含义 |
| --- | --- |
| `IS NULL` | 判断为空值 |
| `IS NOT NULL` | 判断非空 |
| `= NULL` | 错误或无效思路 |
| `COUNT(*)` | 包括 NULL 所在元组 |
| `COUNT(列名)` | 不统计该列 NULL |

### 8.5 子查询与连接查询

很多查询既可以用嵌套查询，也可以用连接查询完成。

一般理解：

- 子查询更接近“分步查找”的思维。
- 连接查询更接近“把相关表合起来筛选”的思维。

## 9. 高频考点

- SQL 的特点：综合统一、非过程化、面向集合、两种使用方式、简洁易用。
- SQL 对三级模式的支持：视图、基本表、存储文件。
- SQL 常用动词：CREATE、DROP、ALTER、SELECT、INSERT、UPDATE、DELETE、GRANT、REVOKE。
- `CREATE TABLE` 的语法和完整性约束。
- `ALTER TABLE` 的常见修改方式。
- `DROP TABLE` 中 `CASCADE` 与 `RESTRICT` 的区别。
- 索引的建立、删除和使用场景。
- SELECT 语句基本格式。
- WHERE、GROUP BY、HAVING、ORDER BY 的作用。
- `DISTINCT` 的作用。
- `LIKE` 中 `%`、`_` 和 `ESCAPE` 的用法。
- 聚集函数与 NULL 的关系。
- 连接查询、自身连接、嵌套查询、相关子查询。
- `EXISTS` 和 `NOT EXISTS` 的含义。
- INSERT、UPDATE、DELETE 的语法和省略 WHERE 的风险。
- NULL 的含义、判断方法和三值逻辑。
- 视图的定义、删除、查询、更新限制和作用。

## 10. 速记版

```text
SQL 四大功能：
定义、查询、操纵、控制

定义：
CREATE、DROP、ALTER

查询：
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ...

更新：
INSERT、UPDATE、DELETE

空值：
NULL 不是 0，也不是空字符串；判断用 IS NULL

视图：
虚表，只存定义，数据来自基本表

WHERE 筛元组，HAVING 筛分组

DELETE 删除数据，DROP 删除定义
```

