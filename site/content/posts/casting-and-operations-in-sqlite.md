---
title: Casting and operations in SQLite
date: 2020-03-07T20:56:52.959Z
tags:
  - sqlite
  - snippets
---
SQLite allows not only to query tables in a database, but also to perform some calculations.

Let's suppose we want to divide a number taken from a column with data of type `integer` by an specific number, for that we need to cast the value as a float and then perform the operation:

```
SELECT CAST(a as float) / 10 FROM (SELECT SUM(my_column) as a FROM MYTABLE WHERE created_at>date("now","-7 days")); 
```
The result will be then a float number :)
