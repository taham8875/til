# Database Partitioning

Partitioning is a technique to split a large table into smaller tables and let the database to decide which table to hit based on the query. This is done to improve performance and manageability of the database.

# Vertical vs Horizontal Partitioning

- Horizontal partitioning is splitting the table by rows or range of values. For example, we can split the table by user id so every 100K users are in a separate table.
- Vertical partitioning is splitting the table by columns. For example, if we have a column that store blob data, we can move that column to a separate table in a slow and cheap storage in its own tablespaces.

# Partitioning Types

- By Range: Split the table by a range of values (Dates or ids). For example, each year (2021, 2020, 2019) is a separate table.
- By List: Split the table by a list of values. For example, each state (CA, LA, NY) is a separate table.
- By Hash: Split the table by a hash function. For example, the hash of the user id modulo 100 is the table number. (Consistent hashing is used in distributed systems)

# Partitioning vs Sharding

- Partitioning is splitting a table into smaller tables in the same database.
- Sharding is splitting a table into smaller tables in different databases.
- So partitioning and sharding are the same except that sharding is done in different databases or servers.

# Partitioning Pros and Cons

- Pros:
  - Improve query performance by reducing the number of rows in each table.
  - Archive old data that are barely accessed in a separate table in cheap storage.

- Cons:
  - Updates that moves that data from one partition to another are expensive.
  - Inefficient queries could accidentally hit all partitions resulting in slower performance.
  - Schema changes can be challenging.