# Tips to increase the performance of your queries

## What is sql performance tuning?

SQL performance tuning is the process of optimizing the query performance by making the query efficient. It is the process of improving the performance of a database system by optimizing the queries that are executed on the database.

## Three types of SQL performance tuning

- **Query Tuning**: This is the process of optimizing the query itself to make it more efficient.
- **Index Tuning**: This is the process of designing, implementing and the indexes on the tables to make the queries that are executed on the tables more efficient.
- **Server Tuning**: This is the process of optimizing the server that the database is running on to make the queries that are executed on the database more efficient.

## What affects the performance of a query?

There are several factors that can affect the performance of a query. Some of them are:

- Table size: The size of the table can affect the performance of a query. The larger the table, the longer it takes to retrieve the data.
- Joins: The number of joins in a query can affect the performance of the query. The more joins, the longer it takes to retrieve the data. Note that the result of a join is a Cartesian product of the tables involved in the join, so it can easily become very large.
- Aggregations: The use of aggregations in a query can affect the performance of the query. The more aggregations, the longer it takes to retrieve the data. More processing is required to get the result than simply retrieving the data.

Query time can also be affected by other factors that you cannot control:

- Other users running queries on the same database, a good system design can help to minimize the impact of this and distribute the load.
- The database itself, how is it implemented, how is it configured, how is it maintained, etc.

### 1. Use `EXPLAIN` to analyze the query

Before you start optimizing your query, it is important to understand how your database executes the query.

`EXPLAIN` is a keyword in SQL that returns information about how your database executes a query. It can be used to analyze the query and identify the parts of the query that are slow.

```sql
EXPLAIN SELECT * FROM students WHERE year > 3;
```

The output of the `EXPLAIN` command will show you the execution plan of the query, including the indexes used, the number of rows scanned, and the time taken to execute the query.

Try to understand the output of the `EXPLAIN` command and use it to identify the parts of the query that are slow. Fix the slow parts of the query and check the output of the `EXPLAIN` command again to see if the query is faster.

For more on `EXPLAIN` command, see the postgreSQL documentation [here](https://www.postgresql.org/docs/current/sql-explain.html).

### 2. Use indexes

Before you start creating indexes, Let's take a look on how to create a postgres table with millions of dummy  records (if you want to search for a dataset online (from kaggle for example) it is up to you):

1. run postgres container via docker:

```bash
$ docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres
$ docker exec -it postgres psql -U postgres
```

2. create a table with millions of dummy records:

```sql
postgres=# CREATE TABLE temp(t INT);
postgres=# INSERT INTO temp SELECT random() * 100 FROM generate_series(1, 1000000);
postgres=# SELECT * FROM temp LIMIT 10;
 t  
----
 54
  2
 23
 78
 31
  4
 60
 31
  5
 59
(10 rows)
postgres=# SELECT COUNT(*) FROM temp;
  count  
---------
 1000001
(1 row)
```

An index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure. Indexes are used to quickly locate data without having to search every row in a database table every time a database table is accessed.

Imagine a phone book, it is an index that allows you to quickly find the phone number of a person by searching for their name. Without the phone book, you would have to search every page of the phone book to find the phone number of a person.

If you have a large table called `Employees` (10 millions rows) with the following columns:

| column name | data type | description |
|-------------|-----------|-------------|
| id          | integer   | primary key |
| name        | text      |             |

Note that by default, the primary key column is indexed.

Try to use the `EXPLAIN ANALYZE` command to see the difference between the query execution time with and without an index:

```sql
postgres=# EXPLAIN ANALYZE SELECT id FROM Employees WHERE id = 1000000; 
```

This is blazing fast because the primary key column is indexed.

Now, let's retrieve the name of the employee with `id = 1000000`:

```sql
postgres=# EXPLAIN ANALYZE SELECT name FROM Employees WHERE id = 1000000; 
```

A little bit slower, postgres retrieves the name of the employee with `id = 1000000` by scanning the index to find the required row, the fetching the page containing the row from the heap.

Now, let's retrieve the id of the employee with `name = 'John'`:

```sql
postgres=# EXPLAIN ANALYZE SELECT id FROM Employees WHERE name = 'John'; 
```

This is way way slower because the only way to get such data is by sequential scanning, postgres goes row by row trying to find out the record of `John`.

> Note that repeating any query twice will be faster the second time since postgres caches the result of the first query.

Let's create an index on the `name` column:

```sql
postgres=# CREATE INDEX name_index ON Employees(name);
```

let's retrieve the id of the employee with `name = 'John'` again:

```sql
postgres=# EXPLAIN ANALYZE SELECT id FROM Employees WHERE name = 'John'; 
```

Notice that the query is now faster because the `name` column is indexed.

> Note that using `WHERE name = 'John'` is faster than using `WHERE name LIKE 'John%'` because the index can be used to retrieve the data directly, while the `LIKE` operator requires a sequential scan trying to match the pattern.
>
> This is where `EXPLAIN` comes in handy, it can show you if the index is used as expected or not. If not, try to figure out why and fix it.

The are more to cover on database indexing, for more on that, I recommend [hussien nasser's videos on databases](https://www.youtube.com/playlist?list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2) and [use the index luke](https://use-the-index-luke.com/)

It is advised to get your hands dirty and try to create a table with millions of records and try to create indexes and see the difference in the query execution time. You can also see this [blog post](https://hinty.io/devforth/sql-query-optimization-understanding-key-principle/) which does some experiments on the performance of the queries with and without indexes with numbers and charts.

### 3. Reduce the table size

Only retrieve the columns and rows you need. This can be done by using the `SELECT` statement to retrieve only the columns you need and the `WHERE` clause to retrieve only the rows you need.

This is the simplest and most forward way to improve the performance of your queries. For example, if you are interested in the `name` and `age` of all senior students (assuming the collage is a 4-year program):

```sql
-- Bad
SELECT * FROM students;

-- Good
SELECT name, age FROM students WHERE year > 3;
```

Another trick is to use the `LIMIT` clause to limit the number of rows returned. This can be useful when you are only interested in the first few rows of a large result set.

```sql
SELECT * FROM students LIMIT 10;
```

But notice that `LIMIT` has nothing to do with the performance in the case of using aggregations. For example, if you want to get the average age of all students, you have to retrieve all the rows and then calculate the average. In this case, `LIMIT` will not help. The aggregation will be calculated on the whole result set then the first 10 rows will be returned if requested.

```sql
SELECT AVG(age) FROM students LIMIT 10;
```

Note also using `LIMIT` on subqueries can return undesired results:

```sql
SELECT AVG(age) FROM (SELECT * FROM students LIMIT 10);
```

the above query will return the average age of the first 10 students in the table, not the average age of all n informative query reality unless you have your reasons to do so.

### 4. Make joins less complex

It is better to reduce the table sizes before joining them, the following example shows a case when breaking the query into an outer query and a subquery can improve the performance:

Let's consider an example where we have to tables `Customers`, `Orders` and `Products` table, `Orders` table has a `product_id` column that references the `id` column in the `Products` table.

The goal is to retrieve a list of customers who ordered a specific product along with the total number of orders by each customer.

```sql
SELECT Customers.customer_id, Customers.customer_name, COUNT(Orders.order_id) AS total_orders
FROM Customers
JOIN Orders ON Customers.customer_id = Orders.customer_id
WHERE Orders.product_id =  12345
GROUP BY Customers.customer_id, Customers.customer_name;
```

The above query is simple and straightforward, but it can be slow if the `Orders` table is large (which is usually the case, e.g 1000000 records).

the result of the previous query:

| customer_id | customer_name | total_orders |
|-------------|---------------|--------------|
| 1           | John          | 5            |
| 2           | Alice         | 3            |
| 3           | Bob           | 7            |
| 4           | Carol         | 2            |

To improve the performance of the previous query, we can break it into an outer query and a subquery:

The subquery aggregates the orders by customer and product, then the outer query filters the result of the subquery to get the customers who ordered the specific product.

To optimize this, we can pre-aggregate the orders for the specific product before joining with the Customers table:

```sql
SELECT customer_id, COUNT(order_id) AS total_orders
FROM Orders
WHERE product_id =  12345
GROUP BY customer_id
```

The result of the previous query:

| customer_id | total_orders |
|-------------|--------------|
| 1           | 5            |
| 2           | 3            |
| 3           | 7            |
| 4           | 2            |

Note that the output of this query is way smaller than the output of the first query (e.g. 1000 records instead of 1000000 records), so it is faster to join the result of this query with the `Customers` table.

```sql
SELECT Customers.customer_id, Customers.customer_name, subquery.total_orders
FROM Customers
JOIN (SELECT customer_id, COUNT(order_id) AS total_orders
      FROM Orders
      WHERE product_id =  12345
      GROUP BY customer_id) AS subquery
ON Customers.customer_id = subquery.customer_id;
```

the result of the previous query is the same of the first query the join is less complex:

| customer_id | customer_name | total_orders |
|-------------|---------------|--------------|
| 1           | John          | 5            |
| 2           | Alice         | 3            |
| 3           | Bob           | 7            |
| 4           | Carol         | 2            |

### Conclusion

- Monitor your queries and the performance of your database regularly to identify the most slow queries and optimize them.
- Use the `EXPLAIN` command to analyze the query and identify the parts of the query that are slow.
- Use indexes to improve the performance of your queries.
- Reduce the table size by retrieving only the columns and rows you need.
- Make joins less complex by breaking the query into an outer query and a subquery.
