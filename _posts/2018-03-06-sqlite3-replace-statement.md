---
layout: post
comments: true
title:  "SQlite3  Replace 用法 (译)"
categories: sqlite3
tags: sqlite3
date: 2018/03/06 14:09:17
---

* content
{:toc}

通过本文可以了解到如何使用 SQlite Replace 在数据表当中插入或替换已经存在的记录。





## 什么是 SQLite Replace?

在 SQLite 中 `replace` 就是，当 `UNIQUE` 或 `RPIMARY KEY` 限制冲突发生时，`replace` 会做两件事：

* 首先，删除(`delete`)导致冲突的记录
* 然后，插入(`insert`)一条新的记录

在第二步，如果有其它限制，导致插入操作不能完成，那么 `replace` 语句会略去 `insert` 并对数据库的操作进行回溯，即回到 `replace` 操作之前（存疑）。

## 如何使用 Replace

 语法：
 `INSERT OR REPLACE INTO table(column_list) VALUES(value_list);`

 或再简洁一些如下：
 `REPLACE INTO table(column_list) VALUES(value_list);`


## SQLite UNIQUE index 实例

### 准备测试数据

创建 `positions` 表

`CREATE TABLE IF NOT EXISTS positions ( id integer PRIMARY KEY, title text NOT NULL, min_salary numeric );`

插入测试数据

`INSERT INTO positions (title, min_salary) VALUES ('DBA', 120000), ('Developer', 100000), ('Architect', 150000);`

查看数据是否正常：

`SELECT id, title, min_salary FROM positions;`

创建唯一索引：

`CREATE UNIQUE INDEX idx_positions_title ON positions (title);`

假定，想添加一条记录到上述数据表当中，如果待新增 `postion` 数据不存在，则新增；如果存在，则修改为新的值。

`REPLACE INTO positions (title, min_salary) VALUES ('Full Stack Developer', 140000);`

要注意的是： **`replace` 表示`insert`或`replace`，但不是`insert`或`update`**

例如：

```
REPLACE INTO positions (id, min_salary)
VALUES
 (2, 110000);
```

上面这条 SQL 会如何操作呢？

* 首先，因为 `id=2` 的 `position` 已经存在了，所以 `replace` 会将之移除
* 接着，SQLite 试图插入新的记录，而这条记录只有两列有数据 `(id, min_salary)`，而 `position` 则没有数据，这与 `title` 列中的 `NOT NULL` 限制违背。于是 SQLite 就回滚了操作。s




## Links

* [SQLite REPLACE Statement](http://www.sqlitetutorial.net/sqlite-replace-statement/)
