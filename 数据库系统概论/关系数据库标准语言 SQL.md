# 第三章 关系数据库标准语言 SQL：重要知识点总结

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

## 2. [[SQL 概述]]

## 3. [[数据定义]]

## 4. [[数据查询]]

## 5. [[数据更新]]

## 6. [[空值 NULL]]
## 7. [[视图]]

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

