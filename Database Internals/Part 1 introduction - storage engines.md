# Storage engines

- The primary goals of any DBMS is reliably store data and make it available for retrieval and modification by users.
- **Storage engine** is a software component of the DBMS which is responsible for storing, retrieving and managing data in memory and on the disk. While database is responsible for complex queries, storage engines allows users to create, update, delete and read records.
- One way to look at this is that DBMS are application that build on the top of the storage engines, offering schema, query language, indexing, transactions, replication, and many more features.
- For flexibility, both keys and values can be arbitrary sequence of bytes with no prescribed form, but their storing and representation semantics are defined in higher level subsystems. For example, the key maybe `int32` and value may be `ascii` string, from the storage engine's point of view, they are just sequences of bytes.
- Storage engines like Berkeley DB, LevelDB, RocksDB, InnoDB, etc. are developed independently from the DBMS and noe embedded into it. DBMSs like MySql enable users to switch the storage engines.

# Comparing Databases

The choice of a database may have long-term consequences. If there is a chance that a database is not a good fit because of performance problems, consistency issues or operational challenges, it is better to find out int the development cycle rather than after the system has been deployed to production since it can be non-trivial to switch to migrate to another database.

Every database has its own set of trade-offs, and it is important to invest some time before choosing a database.

Trying to compare databases based on their components (e.g. storage engine), their popularity or the programming language used to implement it my lead to invalid and premature conclusions. Understanding the fundamental ideas of how each database works and what is inside can help you land a more well-informed decision.

Every comparison should start by defining the goal, so if you are searching for a database that can fit your workloads you have or expect, the best thing is to simulate the workload and see how each database performs.

Simulation the real-world workloads may nit only helps you how each database will perform, it also will helps you to learn how to operate, debug and find out how friendly the and helpful the database and its community are.

Note that performance are not only the only thing to consider, it is usually better ti use a database that slowly saves the data than on that quickly loses it.

To compare that database, it is helpful to understand the use cases and define the variable, such as:

- Schema and records sizes
- Number of clients
- Types of queries and access patterns
- Rates of reads and writes
- Expected changes in any of these parameters over time

Knowing these variable can help you understand the following questions:

- Does the database supports the required queries?
- Is the database able to handle the amount of data you have?
- How many writes and reads per second can the database handle?
- How many nodes the system needs to handle the load?
- What is the maintenance cost of the database?

Based on these questions, you may start construct a test environment and run the workload against the database. Note that most databases have already stress tools that can be used to simulate the workload.

If the test show positive results, iy may be helpful to familiarize yourself with the database code and understand the database components. Having even a rough idea about the database codebase helps you better understand the log records it produces, its configuration parameters, and helps you find issues in the application that uses it and even in the database code itself.

It would be great if we could use databases as black boxes and never ave to take a look inside, but the practice shoes that sooner of later a bug, performance issue or a crash will popup, it it is better to be prepared for that.

Choosing a database is a long-term decision, and its best to keep track of newly released versions, understand what exactly has changed and why, and have an upgrade strategy. New releases usually contain improvements and fixes for bugs and security issues, but may introduce new bugs, performance regressions, or unexpected behavior, so testing new versions before rolling them out is also critical.
