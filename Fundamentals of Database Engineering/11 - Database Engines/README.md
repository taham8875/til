# Database Engines

Database engine is the software that is responsible for low level storing and retrieving data from the underlying database, doing crud operations, managing transactions, etc.

# What is a database engine?

- Library that takes care of the disk storage, crud operations, transactions, etc.
- It can be simple as a key-value store
- Or a rich complex fully support ACID and transactions
- DBMS can use the database engine and build features on top of it (server, client, replication, isolation, etc.)
- Want to write a new database? You can use an existing database engine and build on top of it.
- Sometimes called storage engine, ro embedded database engine

- Some databases give the user the option to select the database engine (MySQL, MariaDB)
- Some databases don't give the user the option to select the database engine (PostgreSQL)

# MyISAM

- Stands for Indexed Sequential Access Method
- Uses B-tree for indexing
- To transaction support
- Open source & owned by Oracle
- Inserts are fast, updates and deletes are problematic
- Database crashes can corrupt data
- Table level locking (not row level locking)
- MySQL 5.5 and earlier versions used MyISAM as the default storage engine

# InnoDB

- B+tree for indexing
- Replaces MyISAM as the default storage engine in MySQL 5.5
- Default storage engine in MySQL
- Supports ACID
- Foreign key support
- Tablespace support
- Row level locking
- Owned by Oracle

# XtraDB

- High performance fork of InnoDB
- Was default storage engine in MariaDB until 10.1
- Starting from MariaDB 10.2, InnoDB is the default storage engine again
- That is because XtraDB couldn't be kept up to date with the latest InnoDB features

# Sqlite

- Very popular embedded database engine for local data storage
- B-tree for indexing (LSM as extension)
- Postgres like syntax
- ACID support
- Table level locking
- Concurrent reads and writes
- Included in many operating systems and browsers by default

# Aria

- Very similar to MyISAM
- Crash safe unlike MyISAM
- Not owed by Oracle
- Designed specifically for MariaDB

# BerkeleyDB

- Key-value store
- Owned by Oracle
- Supports ACID transactions, replication, etc.

# LevelDB

- Written in 2011
- LSM tree (great for hight insert rates and SSD)
- No transactions
- Levels of files: (More means slower) (it moves to a greater level when the current level reaches its size)
  - Memtable (in memory)
  - Level 0 (younger level)
  - Levels 1 - 6
- As files grow in size, they are merged into the next level
- Used in bitcoin core, AuoCAD, Minecraft, etc.

# RocksDB

- Facebook fork of LevelDB (2012)
- Transaction support
- High performance, multi-threaded
- A list of features that are not supported by RocksDB [here](https://github.com/facebook/rocksdb/wiki/Features-Not-in-LevelDB)
- MySQL, MariaDB and Percona uses MyRocks
- MongoDB uses MongoRocks
- Many many more
