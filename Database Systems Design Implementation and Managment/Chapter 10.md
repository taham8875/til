# Chapter 10 - Transaction Management and Concurrency Control

# What is a Transaction?

**Transaction**: A sequence of database requests that accesses the database. A logical unit of work that must be either entirely completed or aborted. No intermediate state is acceptable.

All transactions must have the following properties:

- Atomicity
- Consistency
- Isolation
- Durability

**Consistent database state**: A database state that satisfies all its integrity constraints.

To ensure consistency, every transaction must begin with the database in a known consistent state. the DBMS must ensure that all transactions are executed in their entirety or not at all.

Most real-world database transactions are formed by two or more database requests. A database request is the equivalent of a single SQL statement in an application program or transaction.

## Transaction Properties

Aka ACID properties.

### Atomicity

A transaction must be all or nothing. If one part of the transaction fails, the entire transaction fails.

### Consistency

A transaction must transform the database from one consistent state to another consistent state. The database must be in a consistent state before the transaction begins and after the transaction ends.

### Isolation

A transaction must be isolated from other transactions during its execution. The intermediate state of a transaction should not be visible outside the transaction. The database should not be affected by other transactions until the transaction is complete.

### Durability

Once a transaction has been committed, the effects of that transaction, including any updates to the database, must be permanent. The changes persist even in the event of a system failure.

Another important property is **serializability**. A property in which the selected order of concurrent transaction operations creates the same final database state that would have been produced if the transactions had been executed in a serial fashion.

Naturally, if only a single transaction is executed, serializability is not an issue.

## Transaction Management With SQL

### Transaction Control Commands

- `COMMIT`: Makes all changes to the database permanent.
- `ROLLBACK`: Undoes all changes to the database since the last `COMMIT` or `ROLLBACK` command.

## The Transaction Log

**Transaction log**: A file that contains a history of all transactions and database changes made by each transaction.

# Concurrency Control

**Concurrency control**: The process of managing simultaneous operations against a database so that data integrity is maintained.

## Concurrency Problems

- **Lost update**: A problem that occurs when two or more transactions select the same row, and then update the row based on the original value selected by the other transaction so data updates are lost.
- **Uncommitted data**: A problem that occurs when one transaction updates a row and that row is not committed (rolled back after the second transaction accessed the data) when a second transaction reads the same row.
- **Inconsistent retrievals**: A problem that occurs when one transaction reads a row and then a second transaction reads the same row and updates the row based on the original value selected by the first transaction so the data retrieved by the first transaction is inconsistent.

## The Scheduler

**Scheduler**: A DBMS component that establishes the order in which concurrent transaction operations are executed to ensure serializability.

The scheduler also makes sure that the computer’s central processing unit (CPU) and storage systems are used efficiently. If there were no way to schedule the execution of transactions, all of them would be executed on a first-come, first-served basis. The problem with that approach is that processing time is wasted when the CPU waits for a READ or WRITE operation to finish, thereby losing several CPU cycles. In short, first-come, first-served scheduling tends to yield unacceptable response times within the multiuser DBMS environment. Therefore, some other scheduling method is needed to improve the efficiency of the overall system.

# Concurrency Control with Locking Methods

**Lock**: A DBMS feature used to temporarily block access to data that is being updated by a transaction to prevent other transactions from accessing the same data during the update process.

## Lock Granularity

**Lock granularity**: The size or scope of a lock.

- **Database-level lock**: A lock that is placed on the database itself, which means that all data in the database is unavailable to any user or transaction that does not have the proper access privilege. This type of lock is unsuitable for most multiuser database environments because it prevents all users from accessing the database.
- **Table-level lock**: A lock that is placed on an entire database table.
- **Page-level lock**: A lock that is placed on a database page, which is a fixed-length block of data (addressable section of a disk with a fixed size such ad 4K, 8K, 16K) that is used by the DBMS to store data in the database. Page-level locks are currently the most frequently used locking method for multiuser DBMSs.
- **Row-level lock**: A lock that is placed on a single row in a database table. Although the row-level locking approach improves the availability of data, its management requires high overhead because a lock exists for each row in a table of the database involved in a conflicting transaction. Modern DBMSs automatically escalate a lock from a row level to a page level when the application session requests multiple locks on the same page.
- **Field-level lock**: A lock that is placed on a single field in a database table. This type of lock is not used in most multiuser DBMSs because of the high overhead required to manage it.

## Lock Types

- **Binary lock**: A lock that is either locked or unlocked. A binary lock is also called a Boolean lock.
- **Shared lock**: A lock that allows concurrent transactions to read (but not update) the locked data.
- **Exclusive lock**: A lock that prevents other transactions from accessing the locked data.

**Deadlock**: A situation in which two or more transactions are unable to proceed because each is waiting for another to release a lock.

The three basic techniques for dealing with deadlocks are:

- **Deadlock prevention**: A technique for avoiding deadlocks by ensuring that transactions do not lock data items in a way that causes deadlocks.
- **Deadlock detection**: A technique for dealing with deadlocks by periodically checking the database to determine whether a deadlock has occurred. Then a (victim) transaction is rolled back to break the deadlock.
- **Deadlock avoidance**: A technique for dealing with deadlocks by using a timestamp to determine which transaction should be rolled back to break the deadlock. An example of this technique is making sure that a transaction obtains all the locks it needs before it begins to execute.

## Two-Phase Locking

**Two-phase locking (2PL)**: A set of rules that governs how transactions acquire and relinquish locks.
It consists of two phases:

- Growing phase: A transaction acquires all required locks without unlocking any data.
- Shrinking phase: A transaction releases all locks and cannot obtain a new lock.

So:

- No two transactions can have conflicting locks.
- No unlock operation can precede a lock operation in the same transaction.
- No data is affected until all locks are obtained.

# Concurrency Control with Timestamp Methods

**Timestamp**: A unique identifier that is assigned to each transaction by the DBMS scheduler to determine the order in which transactions are executed.

Timestamps should have the following properties:

- Unique: No two transactions should have the same timestamp.
- Monotonic: The timestamp of a new transaction should be greater than the timestamp of any transaction that has already been assigned and completed.

The disadvantage of timestamp methods is that they require more overhead and memory. The DBMS must maintain a list of all active transactions and their timestamps. In addition, the DBMS must check the timestamp of each transaction to determine whether it is allowed to read or write data.

## Wait/Die and Wound/Wait

### Wait/Die

- If the transaction requesting the lock is the older of the two transactions, it will wait until the other transaction is completed and the locks are released.
- If the transaction requesting the lock is the younger of the two transactions, it will die (roll back) and is rescheduled using the same time stamp.

In short, in the wait/die scheme, the older transaction waits for the younger one to complete and release its locks.

### Wound/Wait

- If the transaction requesting the lock is the older of the two transactions, it will preempt (wound) the younger transaction by rolling it back. T1 preempts T2 when T1 rolls back T2.
The younger, preempted transaction is rescheduled using the same time stamp.
- If the transaction requesting the lock is the younger of the two transactions, it will wait until the other transaction is completed and the locks are released.

In short, in the wound/wait scheme, the older transaction preempts the younger one by rolling it back and rescheduling it.

To prevent deadlocks, each lock request has an associated time-out value. If the lock is not granted before the time-out expires, the transaction is rolled back

# Concurrency Control with Optimistic Methods

**Optimistic method**: A concurrency control method that assumes that multiple transactions can frequently complete their updates without interfering with each other.

t. The optimistic approach requires neither locking nor time stamping techniques. Instead, a transaction is executed without restrictions until it is committed. Using an optimistic approach, each transaction moves through two or three phases, referred to as read, validation, and write.

- **Read phase**: The first phase of an optimistic method in which a transaction executes the needed commands and make update to  private copies of data items.
- **Validation phase**: The second phase of an optimistic method in which a transaction compares its private copies of data items with the values in the database to ensure the data integrity and consistency. If the data is valid, the transaction is allowed to commit. If the data is invalid, the transaction is rolled back.
- **Write phase**: The third phase of an optimistic method in which a transaction writes its updates to the database.

The optimistic approach is acceptable for most read or query database systems that require few update transactions. In a heavily used DBMS environment, the management of deadlocks— their prevention and detection—constitutes an important DBMS function.

# ANSI Levels of Transaction Isolation

Transaction isolation levels refer to the degree to which transaction data is “protected or isolated” from other concurrent transactions.

The types of reads are:

- **Dirty read**: A read operation that retrieves uncommitted data, which is data that is still being processed and has not yet been rolled back.
- **Nonrepeatable read**: A read operation that retrieves different data because data was updated by another transaction after the original read operation.
- **Phantom read**: A read operation that retrieves additional data because data was added by another transaction after the original read operation.

Based on the above operations, ANSI defined four levels of transaction isolation: Read Uncommitted, Read Committed, Repeatable Read, and Serializable.

## Read Uncommitted

- **Dirty read**: Allowed
- **Nonrepeatable read**: Allowed
- **Phantom read**: Allowed

No locks are placed on the data. This is the lowest level of isolation and the highest level of concurrency. It is used in systems that require the highest level of concurrency and do not require data integrity.

## Read Committed

- **Dirty read**: Not allowed
- **Nonrepeatable read**: Allowed
- **Phantom read**: Allowed

The default isolation level for most DBMSs.

## Repeatable Read

- **Dirty read**: Not allowed
- **Nonrepeatable read**: Not allowed
- **Phantom read**: Allowed

Repeatable Read isolation level ensures that queries return consistent results. This type of isolation level uses shared locks to ensure other transactions do not update a row after the original query reads it.

## Serializable

- **Dirty read**: Not allowed
- **Nonrepeatable read**: Not allowed
- **Phantom read**: Not allowed
-

The Serializable isolation level is the most restrictive level defined by the ANSI SQL standard. However, it is important to note that even with a Serializable isolation level, deadlocks are always possible

# Database Recovery Management

**Database recovery**: The process of restoring a database to a correct state in the event of a failure.

Recovery techniques are based on **atomic transaction property**: A transaction must be all or nothing. If one part of the transaction fails, the entire transaction fails.

Example of critical failures:

- Hardware failure
- Software failure
- Human caused failure (either intentional or unintentional)
- Natural disaster

## Transaction Recovery

Before continuing, we need to define the following terms:

- **Write-ahead log (WAL)**: A log that records the update activity of a database so that the database can be restored to a permenant storage in the event of a failure.
- **redundant transaction log**: A backup copy of the transaction log that is stored on a separate disk from the primary transaction log to ensure that physical failure of the disk containing the primary transaction log does not result in the loss of the transaction log.
- **Buffer**: A temporary storage area in memory or on disk that contains the updated data before it is written to the database.
- **Checkpoint**: In transaction management, an operation in which the database management system writes all of its updated buffers to disk.

When the recovery procedure uses a **deferred-write technique** (also called a **deferred update**), the transaction operations do not immediately update the physical database. Instead, only the transaction log is updated. The database is physically updated only with data from committed transactions, using information from the transaction log. If the transaction aborts before it reaches its commit point, no changes (no ROLLBACK or undo) need to be made to the database because it was never updated.

When the recovery procedure uses a **write-through technique** (also called an **immediate update**), the database is immediately updated by transaction operations during the transaction’s execution, even before the transaction reaches its commit point. If the transaction aborts before it reaches its commit point, a ROLLBACK or undo operation needs to be done to restore the database to a consistent state. In that case, the ROLLBACK operation will use the transaction log “before” values
