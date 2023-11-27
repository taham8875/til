# Chapter 0 - Introduction

This book aims to be a short introduction to a minimal database implementation. It is not meant to be a complete reference, but rather a starting point for further exploration.

This book focuses on important ideas rather than implementation details, it aims to cover three main topics:

- Persistence. How not to lose data. Recovering from crashes.
- Indexing. How to find data efficiently.
- Concurrency. How to allow multiple clients to access the database at the same time.

## Persistence

When you simply write the data directly into files instead of a database. What happens when your process crashes? What happens when your computer loses power? Does the file just loses the last few changes? Does it become corrupted? Does it lose all the data ending with more corrupted state?

## Indexing

There are tow types of database queries:

- Analytical (OLAP) queries. These are queries that scan a large amount of data , with aggregation, grouping, or join operations.
- Transactional (OLTP) queries. These are queries that look up a small number of indexed records. (The most common type of query)

In order to make the transactional queries fast, we need to build indexes. An index is a data structure that allows you to find all the data matching a particular value very quickly.

Common data structures used for indexes are B-Trees and LSM-Trees.

## Concurrency

Concurrency is the ability of multiple clients to use a database at the same time. It is a very important feature of a database, because it allows many users to access the data simultaneously.

There are two levels of concurrency control:

- Concurrency between readers.
- Concurrency between readers and writers.

Concurrency is easier within a process, which is why most databases are designed to be only accessed via a server.
