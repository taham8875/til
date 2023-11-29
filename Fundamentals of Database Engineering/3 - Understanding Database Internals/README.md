# How tables and indexes are stored on disk

## What is a table?

Tables are the main component of a relational database. A table is a collection of rows and columns. Each row represents a single entity, and each column represents a field in the entity.

## What is ROW_ID

ROW_ID is a unique identifier for each row in a table. It is used to identify the row in the table. It is also used to identify the row in the indexes. It is internal and system maintained. It is not visible to the user.

In certain databases (MySQL) it is the same as the primary key. In other databases (Postgres) it is a separate column.

| ROW_ID | EMPLOYEE_ID | FIRST_NAME | LAST_NAME |
| ------ | ----------- | ---------- | --------- |
| 1      | 984651      | John       | Doe       |
| 2      | 984652      | Jane       | Doe       |
| 3      | 984653      | Jack       | Doe       |

## What is a page?

A page is a unit of data storage in a database. It is a fixed-size block of memory. It is the smallest unit of data that can be read from or written to disk by the database engine.

So the database cannot read a single row from the disk. It has to read the whole page. So if a page is 8KB, and a row is 1KB, the database will read 8 rows at once.

Each page has a size (e.g. 8KB in Postgres and 16KB in MySQL). The size of the page is fixed and cannot be changed.

## What is a IO?

IO stands for Input/Output. It is the process of reading data from or writing data to a storage device (e.g. disk).

We try to minimize the number of IOs because it is slow. Less IOs means faster queries.

An IO cannot read a single row from the disk. It has to read the whole page. So if a page is 8KB, and a row is 1KB, the database will read 8 rows at once.

Some IOs goes to the os cache instead of the disk.

## What is a heap?

A heap is a table without any indexes. It is the default table type in Postgres.

This is where everything is stored.

Traversing the heap is very expensive, so we use indexes to speed up the queries and to tell us what part of the heap we need to read.

## What is a index?

An index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure.

You can use index on one column or multiple columns.

Index tells you exactly which page to fetch from the page instead of traversing the whole heap to find the rows.

The smaller the index, the faster the queries.

B-tree is the most common index data structure.

## Notes

- Sometimes the heap table can be organized around a single index. This is called a clustered index.
- Primary key is also a clustered index unless otherwise specified.

# Row vs Column oriented databases

## What is a row oriented database?

In a row oriented database, the data is stored in rows. The data of a single row is stored together. The data of a single column is spread across multiple rows.

## What is a column oriented database?

In a column oriented database, the data is stored in columns. The data of a single column is stored together. The data of a single row is spread across multiple columns.

## Which one is better?

| Row oriented database | Column oriented database |
| --------------------- | ------------------------ |
| Optimal for reads/writes | Slow for writes |
| OLTP (Online Transaction Processing) | OLAP (Online Analytical Processing) |
| Compression is not effective | Compression is effective |
| Aggregation is slow | Aggregation is fast |
| Efficient queries on with multiple columns | Efficient queries on with single column |
