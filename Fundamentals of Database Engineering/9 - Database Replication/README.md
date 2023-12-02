# Database Replication

Database replication is the frequent electronic copying of data from a database in one computer or server to a database in another so that all users share the same level of information. The result is a distributed database in which users can access data relevant to their tasks without interfering with the work of others.

The goal of database replication is to ensure that each replicated database remains synchronized and consistent

## Master/Backup Replication

- One master database that accepts writes.
- One or more backup databases that receive those writes.
- Simple to implement.

## Synchronous vs Asynchronous replication

### Synchronous replication

- The master database waits for the backup database to confirm that it has received the write before confirming the write to the client.
- May be it is required to write to all the replicas or first 2 or 1 replica.

### Asynchronous replication

- The write is considered complete as soon as the master database confirms the write to the client. Then asynchronously the master database will send the write to the backup database.

## Pros and Cons

Pros:

- Horizontal scaling
- Region based scaling (DB per region)
- Simple to implement in master/backup replication

Cons:

- Eventual consistency
- Slow writes in synchronous replication
- Complex to implement in multi-master replication
