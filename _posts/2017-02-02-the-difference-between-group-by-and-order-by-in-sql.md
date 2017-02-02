---
layout: post-zh
title: The difference between group by and order by in SQL
---

## [SQL - Group By](//www.tutorialspoint.com/sql/sql-group-by.htm)

SQL **GROUP BY** 子句与 SELECT 语句协作使用，以将相同的数据分组。

GROUP BY 子句位于 SELECT 语句中的 WHERE 子句之后，位于 ORDER BY 子句之前。

### 语法

下面给出了 GROUP BY 子句的基本语法。 GROUP BY 子句必须遵循 WHERE 子句中的条件，并且必须在使用 ORDER BY 子句之前。

```sql
SELECT column1, column2
FROM table_name
WHERE [ conditions ]
GROUP BY column1, column2
ORDER BY column1, column2
```

### 示例

假定 CUSTOMERS 表具有以下记录：

```sql
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```

如果你想知道每个客户的工资总额，那么 GROUP BY 查询将如下：

```sql
SQL> SELECT NAME, SUM(SALARY) FROM CUSTOMERS
     GROUP BY NAME;
```

这将产生以下结果：

```sql
+----------+-------------+
| NAME     | SUM(SALARY) |
+----------+-------------+
| Chaitali |     6500.00 |
| Hardik   |     8500.00 |
| kaushik  |     2000.00 |
| Khilan   |     1500.00 |
| Komal    |     4500.00 |
| Muffy    |    10000.00 |
| Ramesh   |     2000.00 |
+----------+-------------+
```

现在，让我们有下面的表，其中 CUSTOMERS 表有以下具有重复名称的记录：

```sql
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Ramesh   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | kaushik  |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```

现在再次，如果你想知道每个客户的工资总额，那么 GROUP BY 查询将如下：

```sql
SQL> SELECT NAME, SUM(SALARY) FROM CUSTOMERS
     GROUP BY NAME;
```

这将产生以下结果：

```sql
+---------+-------------+
| NAME    | SUM(SALARY) |
+---------+-------------+
| Hardik  |     8500.00 |
| kaushik |     8500.00 |
| Komal   |     4500.00 |
| Muffy   |    10000.00 |
| Ramesh  |     3500.00 |
+---------+-------------+
```

## [what is the difference between GROUP BY and ORDER BY in sql](http://stackoverflow.com/questions/1277460/what-is-the-difference-between-group-by-and-order-by-in-sql)

* http://stackoverflow.com/a/1277482

  ORDER BY 更改返回项目的顺序。

  GROUP BY 将按指定列聚合记录，这样您可以对非分组列（例如 SUM，COUNT，AVG 等）执行聚合功能。

  ```sql
  TABLE:
  +----+----------+
  | ID | NAME     |
  +----+----------+
  |  1 | Peter    |
  |  2 | John     |
  |  3 | Greg     |
  |  4 | Peter    |
  +----+----------+

  SELECT * FROM TABLE ORDER BY NAME;

  =
  3 Greg
  2 John
  1 Peter
  4 Peter

  SELECT Count(ID), NAME, FROM TABLE GROUP BY NAME;

  =
  1 Greg
  1 John
  2 Peter

  SELECT NAME FROM TABLE GROUP BY NAME HAVING Count(ID) > 1;

  =
  Peter
  ```

* http://stackoverflow.com/a/30988600

  **ORDER BY**：按升序或降序排序数据。

  假定 **CUSTOMERS** 表：

  ```sql
  +----+----------+-----+-----------+----------+
  | ID | NAME     | AGE | ADDRESS   | SALARY   |
  +----+----------+-----+-----------+----------+
  |  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
  |  2 | Khilan   |  25 | Delhi     |  1500.00 |
  |  3 | kaushik  |  23 | Kota      |  2000.00 |
  |  4 | Chaitali |  25 | Mumbai    |  6500.00 |
  |  5 | Hardik   |  27 | Bhopal    |  8500.00 |
  |  6 | Komal    |  22 | MP        |  4500.00 |
  |  7 | Muffy    |  24 | Indore    | 10000.00 |
  +----+----------+-----+-----------+----------+
  ```

  以下是一个示例，它会按 NAME 的升序对结果进行排序：

  ```sql
  SQL> SELECT * FROM CUSTOMERS
       ORDER BY NAME;
  ```

  这将产生以下结果：

  ```sql
  +----+----------+-----+-----------+----------+
  | ID | NAME     | AGE | ADDRESS   | SALARY   |
  +----+----------+-----+-----------+----------+
  |  4 | Chaitali |  25 | Mumbai    |  6500.00 |
  |  5 | Hardik   |  27 | Bhopal    |  8500.00 |
  |  3 | kaushik  |  23 | Kota      |  2000.00 |
  |  2 | Khilan   |  25 | Delhi     |  1500.00 |
  |  6 | Komal    |  22 | MP        |  4500.00 |
  |  7 | Muffy    |  24 | Indore    | 10000.00 |
  |  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
  +----+----------+-----+-----------+----------+
  ```

  **GROUP BY**：将相同的数据分组。

  现在，CUSTOMERS 表具有以下重复名称的记录：

  ```sql
  +----+----------+-----+-----------+----------+
  | ID | NAME     | AGE | ADDRESS   | SALARY   |
  +----+----------+-----+-----------+----------+
  |  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
  |  2 | Ramesh   |  25 | Delhi     |  1500.00 |
  |  3 | kaushik  |  23 | Kota      |  2000.00 |
  |  4 | kaushik  |  25 | Mumbai    |  6500.00 |
  |  5 | Hardik   |  27 | Bhopal    |  8500.00 |
  |  6 | Komal    |  22 | MP        |  4500.00 |
  |  7 | Muffy    |  24 | Indore    | 10000.00 |
  +----+----------+-----+-----------+----------+
  ```

  如果要将相同的名称分组为单个名称，则GROUP BY查询将如下所示：

  ```sql
  SQL> SELECT * FROM CUSTOMERS
       GROUP BY NAME;
  ```

  这将产生以下结果：（对于相同的名称，它将选择最后一个，最后以升序对列进行排序）

  ```sql
  +----+----------+-----+-----------+----------+
  | ID | NAME     | AGE | ADDRESS   | SALARY   |
  +----+----------+-----+-----------+----------+
  |  5 | Hardik   |  27 | Bhopal    |  8500.00 |
  |  4 | kaushik  |  25 | Mumbai    |  6500.00 |
  |  6 | Komal    |  22 | MP        |  4500.00 |
  |  7 | Muffy    |  24 | Indore    | 10000.00 |
  |  2 | Ramesh   |  25 | Delhi     |  1500.00 |
  +----+----------+-----+-----------+----------+
  ```

  正如你所推断的，它不使用 SQL 函数，如 sum，avg 等是没有用处的。

  所以通过这个定义来理解 GROUP BY 的正确使用：

  > GROUP BY 子句通过在 SELECT 列表中使用适当的聚合函数（如 COUNT()，SUM()，MIN()，AVG()）来对查询返回的行起作用，将相同的行汇总到单个/不同组中，并返回具有每个组的摘要的单个行。

  现在，如果你想知道每个客户（姓名）的工资总额，那么 GROUP BY 查询将如下：

  ```sql
  SQL> SELECT NAME, SUM(SALARY) FROM CUSTOMERS
       GROUP BY NAME;
  ```

  这将产生以下结果：（相同名称的工资总和，并在删除相同名称后对 NAME 列进行排序）

  ```sql
  +---------+-------------+
  | NAME    | SUM(SALARY) |
  +---------+-------------+
  | Hardik  |     8500.00 |
  | kaushik |     8500.00 |
  | Komal   |     4500.00 |
  | Muffy   |    10000.00 |
  | Ramesh  |     3500.00 |
  +---------+-------------+
  ```

  ​

