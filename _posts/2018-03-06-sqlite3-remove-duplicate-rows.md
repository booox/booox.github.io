---
layout: post
comments: true
title:  "在 SQlite3 中删除重复的记录"
categories: sqlite3
tags: sqlite3
date: 2018/03/06 11:04:34
---

* content
{:toc}

在 SQlite3 中删除重复的记录



## 查找不重复的记录

在使用 `SELECT`语句查询时可以添加 `DISTINCT` 选项，来将重复的记录移除。

`SELECT DISTINCT column_list FROM table;

对多列使用 `DISTINCT`

`SELECT DISTINCT city, country FROM customers ORDER BY country;`


### 查询重复的记录

`SELECT DataId, COUNT(*) c FROM DataTab GROUP BY DataId HAVING c > 1;`

### 从数据表中删除已重复的记录

**在删除记录之前，做好备份!!**

* 从 Follows 表中删除 userId 重复的记录

`DELETE FROM Follows WHERE rowid NOT IN (SELECT min(rowid) FROM Follows GROUP BY userId);`

* 从 sms 表中删除 address 与 body 同时重复的记录

`DELETE FROM sms WHERE rowid NOT IN (SELECT min(rowid) FROM sms GROUP BY address, body);`

[SQLite3: Remove duplicates](https://dba.stackexchange.com/questions/116868/sqlite3-remove-duplicates)






要删除

`DELETE FROM sms WHERE rowid NOT IN (SELECT min(rowid) FROM sms GROUP BY address, body);`
