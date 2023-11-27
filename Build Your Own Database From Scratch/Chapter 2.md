# Chapter 2 - Indexing

Al,ost all queries can be broken into three types of disk operations:

- Scan the whole data set. (No index is used)
- Point Query. (Query the index by a specific key)
- Range Query. (Query the index by a range of keys) (The index is sorted)

Database indexes are mostly about range queries and point queries

We will build a Key Value store before attempting to build a database on top of it. Let's explore our options first.

## Hashtables

Hashtables are the first to be ruled out when designing a general-purpose KV store.

The main reason is sorting. Many real world application do require sorting. Sorting is not possible with hashtables.

Another headache of hashtables is the resizing operation, naive resizing is O(n) and causes a sudden increase in disk usage and IO. It is possible ti resize a hashtable incrementally, but this adds complexity.

## B-Trees

B-Trees are the most common index type in databases. They are used in almost every relational database, and many non-relational databases as well.

Balanced binary trees can be queried and updated in O(log n) time and can be range-queried.

Some advantages of B-Trees:

- Less space overhead
- Faster in memory
- Less disk seeks
