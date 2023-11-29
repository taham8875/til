# ACID

## What is a transaction?

Transaction is a set of operations (sql queries) that are executed as a single unit of work. A transaction may consist of one or more operations. A transaction groups all the operations as a single unit. If any of the operations fail, the transaction fails. If all the operations succeed, the transaction succeeds.

### Transaction Lifespan

1. Begin Transaction: The transaction begins when the first operation is executed.
2. Commit Transaction: The transaction is committed when all the operations are executed successfully.
3. Rollback Transaction: The transaction is rolled back when any of the operations fail so the database is not affected by partial transactions.

## Atomicity

Atomicity means that a transaction must be treated as an atomic unit, that is, either all of its operations are executed or none. There must be no state in a database where a transaction is left partially completed. If an error occurs in the middle of a transaction, the entire transaction must be rolled back. So all queries in transaction must succeed for transaction to succeed.

## Isolation

Isolation means that the execution of transactions concurrently may result in an inconsistent state of the database. So, isolation ensures that concurrent execution of transactions leaves the database in a consistent state. Isolation is the main goal of concurrency control; depending on the method used, the effects of an incomplete transaction might not even be visible to other transactions.

Why do we need isolation?

- To prevent dirty reads: A dirty read occurs when a transaction is allowed to read data from a row that has been modified by another running transaction and not yet committed.
- To prevent non-repeatable reads: A non-repeatable read occurs when a transaction is allowed to reread data it has previously read and finds that data has been modified by another transaction (that committed since the initial read).
- To prevent phantom reads: A phantom read occurs when a transaction is allowed to reread data it has previously read and finds that new rows have been inserted by another transaction (that committed since the initial read).
- To prevent lost updates: A lost update occurs when two transactions that access the same row are executed concurrently and the value of the row is updated and saved by the first transaction before the second transaction can read it. The second transaction then overwrites the value written by the first transaction and the first transaction is lost.

### Levels of Isolation

- Read Uncommitted: This is the lowest isolation level. In this level, one transaction may read not yet committed changes made by other transaction. This level is not used in practice. (but it is technically faster)
- Read Committed: Each query in a transaction only sees committed changes by other transactions. This is the default isolation level in most systems.
- Repeatable Read: The transaction will make sure that when a query reads a row, it will get the same data for that row every time in the same transaction.
- Snapshot Isolation: This level guarantees that all reads made in a transaction will see a consistent snapshot of the database (all the changes that have been committed up to the start of the transaction). This level guarantees that a transaction will never see any data that has not been committed.

### Database implementations of Isolation

- Pessimistic Concurrency Control: Locks are used to prevent other transactions from modifying data that is being read by a transaction. (Slower)
- Optimistic Concurrency Control: Transactions proceed without any locks and when a transaction wants to commit, it verifies that no other transaction has modified the data it has read. It will fail if another transaction has modified the data. (Faster)
- Repeatable Read: Locks the rows it reads and releases the locks when the transaction is committed or rolled back.

## Consistency

### Consistency in data

Consistency in data means that only valid data will be written to the database. This comes out from referential integrity.

### Consistency in read

Consistency in read means that only committed data will be read from the database. (If I committed a transaction, other users will be able to read the data I committed). Most databases solve this problem by eventual consistency.

## Durability

Durability means that once a transaction has been committed, it will remain so (saved in the storage), even in the event of power loss, crashes, or errors. In a relational database, for instance, once a group of SQL statements execute, the results need to be stored permanently (even if the database crashes immediately thereafter). To defend against power loss, transactions (or their effects) must be recorded in a non-volatile memory.

### Durability techniques

- Write-Ahead Logging: The changes made by a transaction must be written to the log before the changes are written to the database. If the database crashes before the changes are written to the database, the changes can be recovered from the log.
- Asynchronous snapshot: As we write we keep everything in the memory, but periodically we write everything to the disk. If the database crashes, we can recover the data from the last snapshot.
- AOF (Append Only File): We write all the commands to a file. If the database crashes, we can recover the data from the file.

### OS cache

OS cache is a cache that is used by the operating system to store the data that is read from the disk. If the database crashes, we can recover the data from the OS cache. But the OS cache is not persistent, so if the OS crashes, we cannot recover the data from the OS cache.

To solve this problem, we can use `fsync` to force the OS to write the data from the OS cache to the disk. (flushing the OS cache) (This is slower)

Is using os cache dangerous? No, crashes are rare.
