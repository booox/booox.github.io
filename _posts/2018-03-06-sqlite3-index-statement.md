---
layout: post
comments: true
title:  "SQlite3 Index 用法 (译)"
categories: sqlite3
tags: sqlite3
date: 2018/03/06 12:23:26
---

* content
{:toc}

通过本文可以了解到如何使用 SQlite Index 来快速查询数据，加速排序以及加强唯一性检查。





## 什么是 Index?

在关系数据库（ MySql, SQlite3 等），一张表由许多行组成。每一行都有着相同的列的结构，存在着同类的数据。每一行都有一个连续的序号 `rowid` 用来标识行。因为我们可以将一张数据表看作是由许多 `(rowid, row)` 对组成的列表。

与表格不一样的是，`index` （索引） 有着相反的关系: `(row, rowid)`。`index` 是一种额外的数据结构，它能够加速查询、联合以及组合等操作。

SQLite 使用 `B-tree` index。`B` 代表平衡。

## index 如何工作？

每一个 `index` 必须与特定的表关联起来。一个 `index` 包含同一表中的一列或多列。一个表可以有多个 `index`。

当你创建了一个 `index`，SQLite 就创建了一个 `B-tree` 结构来存储 `index` 数据。

`index` 包含你在索引中指定的列中的数据，及对应的 `rowid` 值。这会帮助 SQLite 快速地找到行。

想像一下，数据表中的 `index` 就像一本 `index` 的书。通过查找 `index`，你可以快速地找到基于关键词所在的页码。

## SQLite 创建 INDEX

使用 `create index` 语句：

`CREATE [UNIQUE] INDEX index_name ON table_name(indexed_column);`

在创建索引时，需指定三个信息：

* `index` 所关联的表
* 哪一列
* `index` 的名字

## SQLite UNIQUE index 实例

### 单列 index

`CREATE TABLE contacts ( first_name text NOT NULL, last_name text NOT NULL, email text NOT NULL );`

假定，你想确保 `email` 是唯一的，那么可以创建如下的 `index`：

`CREATE UNIQUE INDEX idx_contacts_email ON contacts (email);`

为了验证，可以先插入一行到 `contacts` 中：

```sql
INSERT INTO contacts (first_name, last_name, email)
VALUES
 (
 'John',
 'Doe',
 'john.doe@sqlitetutorial.net'
 );
```

如果再次执行上述插入语句时，则 SQLite 会报错：

`UNIQUE constraint failed: contacts.email:`

当你通过 `email` 列来查询 `contacts` 表时，SQLite 会使用 `index` 来定位数据。

```sql
SELECT
 first_name,
 last_name,
 email
FROM
 contacts
WHERE
 email = 'lisa.smith@sqlitetutorial.net';
```

为了验证 SQLite 是否使用了 `index` ，可以使用 `explain query plan`：

```sql
EXPLAIN QUERY PLAN
SELECT
 first_name,
 last_name,
 email
FROM
 contacts
WHERE
 email = 'lisa.smith@sqlitetutorial.net';
```

### 多列 index

如果 index 只包含一列，SQLite 会使用该列作为排序的 key。
如果你创建包含多列的 index，SQLite 会将其他列分别作为第二、第三...排序的 key。

SQLite 对基于多列 `index` 对数据进行排序时，会首先根据在创建 `index` 时指定的第一列来进行排序。

因此，在创建多列 `index` 时，列的顺序是非常重要的。

而了更有效地利用多列索引，查询时包含的条件必须按照定义 `index` 的顺序。

下面在 `contacts`表中创建一个基于 `first_name` 与 `last_name` 的多列 `index` 。

`CREATE INDEX idx_contacts_name ON contacts (first_name, last_name);`

按照定义的顺序查询，则使用了 `index`

`EXPLAIN QUERY PLAN SELECT * from contacts WHERE first_name = 'John';`

`EXPLAIN QUERY PLAN SELECT * from contacts WHERE first_name = 'John' AND last_name = 'Doe';`

> `SEARCH TABLE contacts USING INDEX idx_contacts_name`

没有按照定义的顺序查询，则没有使用 `index`

`EXPLAIN QUERY PLAN SELECT * from contacts WHERE last_name = 'Doe';`

`EXPLAIN QUERY PLAN SELECT * from contacts WHERE last_name = 'Doe' OR first_name = 'John';`

> `SCAN TABLE contacts`


## 删除 index

使用 `drop index` 来删除 `index`

`DROP INDEX [IF EXISTS] index_name;`

如：

`DROP INDEX IF EXISTS idx_contacts_name;`

## Links

* [SQLite Index Statement](http://www.sqlitetutorial.net/sqlite-index/)
