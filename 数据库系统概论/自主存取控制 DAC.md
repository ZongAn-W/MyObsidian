### 6.1 自主存取控制的含义

自主存取控制（DAC）中，用户对不同数据库对象拥有不同权限，并且某些权限可以由授权用户授予其他用户。

SQL 标准主要通过：

```sql
GRANT
REVOKE
```

实现自主存取控制。

### 6.2 常见数据库对象和权限

常见对象：

- 数据库
- 模式
- 基本表
- 视图
- 索引
- 属性列

常见操作权限：

| 对象 | 常见权限 |
| --- | --- |
| 数据库/模式 | CREATE |
| 基本表 | CREATE TABLE、ALTER TABLE |
| 视图 | CREATE VIEW |
| 索引 | CREATE INDEX |
| 表或视图 | SELECT、INSERT、UPDATE、DELETE、REFERENCES |
| 属性列 | SELECT、INSERT、UPDATE、REFERENCES |

### 6.3 授权 GRANT

基本格式：

```sql
GRANT <权限> [, <权限>] ...
ON <对象类型> <对象名>
TO <用户> [, <用户>] ...
[WITH GRANT OPTION];
```

例子：

```sql
GRANT SELECT
ON TABLE Student
TO U1;
```

将多个权限授予用户：

```sql
GRANT SELECT, INSERT, UPDATE
ON TABLE Student
TO U1, U2;
```

授予某列权限：

```sql
GRANT UPDATE(Sno), SELECT
ON TABLE Student
TO U4;
```

授予传播权限：

```sql
GRANT INSERT
ON TABLE SC
TO U5
WITH GRANT OPTION;
```

`WITH GRANT OPTION` 表示被授权用户还可以把该权限再授予其他用户。

### 6.4 权限回收 REVOKE

基本格式：

```sql
REVOKE <权限> [, <权限>] ...
ON <对象类型> <对象名>
FROM <用户> [, <用户>] ...
[CASCADE | RESTRICT];
```

例子：

```sql
REVOKE SELECT
ON TABLE Student
FROM U1;
```

要点：

- 回收权限会影响用户后续访问。
- 若权限曾继续传播给其他用户，回收时可能涉及级联回收。
- 是否级联由系统和语句选项决定。