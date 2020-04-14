# Subqueries

A subquery is a query in a query. It is also called an inner query or a nested query. A subquery can be used anywhere an expression is allowed. It is a query expression enclosed in parentheses. Subqueries can be used with `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statements.

Subqueries are often performed on tables, which have some relationship.

```
# Cars
+----+------------+--------+
| Id | Name       | Cost   |
+----+------------+--------+
|  1 | Audi       |  52642 |
|  2 | Mercedes   |  57127 |
|  3 | Skoda      |   9000 |
|  4 | Volvo      |  29000 |
|  5 | Bentley    | 350000 |
|  6 | Citroen    |  21000 |
|  7 | Hummer     |  41400 |
|  8 | Volkswagen |  21600 |
+----+------------+--------+

# Customers
+------------+-------------+
| CustomerId | Name        |
+------------+-------------+
|          1 | Paul Novak  |
|          2 | Terry Neils |
|          3 | Jack Fonda  |
|          4 | Tom Willis  |
+------------+-------------+

# Reservations
+----+------------+------------+
| Id | CustomerId | Day        |
+----+------------+------------+
|  1 |          1 | 2009-11-22 |
|  2 |          2 | 2009-11-28 |
|  3 |          2 | 2009-11-29 |
|  4 |          1 | 2009-11-29 |
|  5 |          3 | 2009-12-02 |
+----+------------+------------+
```

## Subquery with the INSERT statement

We want to create a copy of the Cars table. Into another table called Cars2. We will create a subquery for this.

```sql
INSERT INTO Cars2 SELECT * FROM Cars;
```

## Scalar subqueries

A scalar subquery returns a single value.

The query enclosed in parentheses is the subquery. It returns one single scalar value. The returned value is then used in the outer query.

```sql
SELECT Name FROM Customers WHERE CustomerId=(SELECT CustomerId FROM Reservations WHERE Id=5);

/*
+------------+
| Name       |
+------------+
| Jack Fonda |
+------------+
*/
```

## Table subqueries

A table subquery returns a result table of zero or more rows.

> Note: The use of `IN` where multiple rows are returned.

```sql
SELECT Name FROM Customers WHERE CustomerId IN (SELECT DISTINCT CustomerId FROM Reservations);
/*
+-------------+
| Name        |
+-------------+
| Paul Novak  |
| Terry Neils |
| Jack Fonda  |
+-------------+
*/
```
