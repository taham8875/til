# Chapter 12 - Distributed Database Management Systems

**Distributed Database Management System (DDBMS)**: A software system that permits the management of the distributed database across several different sites.

# DDBMS Components

- Computer workstations or remote devices that are connected to the network
- Network hardware and software components
- Communication media
- Transaction processor (TP) : A software system that manages the execution of distributed transactions across several different sites.
- Data processor (DP) : The DP is responsible for managing the local data in the computer and coordinating access to that data.

# Distributed Database Transparency Features

The end user should have the sense of working with a centralized DBMS. For this reason, the minimum desirable DDBMS transparency features are:

- **Distribution transparency**: The user should not know that the database is distributed across multiple sites, partitioned or replicated.
- **Transaction transparency**: Transaction transparency ensures that the transaction will be either entirely completed or aborted at more than one network site.
- **Failure transparency**: Ensures that the system will continue to operate in the event of a node or network failure.
- **Performance transparency**: Allows the system to perform as if it were a centralized DBMS. The system will not suffer any performance degradation due to its use on a network or because of the network’s platform differences.
- **Heterogeneity transparency**: A feature that allows a system to integrate several centralized DBMSs into one logical DDBMS.

## Distribution Transparency

Three levels of distribution transparency:

- **Fragmentation transparency**: The ability to hide the fact that the database is partitioned across multiple sites, The user must not know the location or the fragment names. The highest level of distribution transparency.
- **Location transparency**: The user must specify the database fragment but not the location of the fragment. (lower level of distribution transparency)
- **Local mapping transparency**: The user must specify both the location and the fragment name. (lowest level of distribution transparency)

Depending on the level of distribution transparency support, you may examine three query cases.

Case 1: The Database Supports Fragmentation Transparency

```sql
SELECT * 
FROM EMPLOYEE
WHERE EMPLOYEE.SALARY > 50000
```

Case 2: The Database Supports Location Transparency

```sql
SELECT *
FROM EMPLOYEE@SITE1
WHERE EMPLOYEE.SALARY > 50000
UNION
SELECT *
FROM EMPLOYEE@SITE2
WHERE EMPLOYEE.SALARY > 50000
UNION
SELECT *
FROM EMPLOYEE@SITE3
WHERE EMPLOYEE.SALARY > 50000
```

Case 3: The Database Supports Local Mapping Transparency

```sql
SELECT *
FROM EMPLOYEE@SITE1 NODE NY
WHERE EMPLOYEE.SALARY > 50000
UNION
SELECT *
FROM EMPLOYEE@SITE2 NODE LA
WHERE EMPLOYEE.SALARY > 50000
UNION
SELECT *
FROM EMPLOYEE@SITE3 NODE CH
WHERE EMPLOYEE.SALARY > 50000
```

## Transaction Transparency

**Remote request**: A request that is sent to a remote site for processing.

**Remote transaction**: A transaction (formed by several requests) that is executed at a remote site.

**Distributed request**: A single request that accesses data at several remote sites.

**Distributed transaction**: A transaction that accesses data in several remote sites.

### Two-Phase Commit Protocol

**Two-phase commit protocol**: A protocol that ensures that a distributed transaction is either entirely completed or aborted at all sites.

**Do-Undo-Redo protocol**: A protocol used by a data processor (DP) to roll back or roll forward transactions with the help of a system’s transaction log entries.

**Write ahead protocol**: A protocol that requires that all log records be written to the log file before the corresponding data values are written to the database.

# Distributed Database Design

Whether the database is centralized or distributed, the design principles and concepts described in earlier chapters are still applicable. However, the design of a distributed database introduces three new issues:

- How to partition the database into fragments
- Which fragments to replicate
- Where to locate those fragments and replicas

## Data Fragmentation

**Data fragmentation**: The process of dividing an object into several fragments and storing those fragments at several sites. The object might be a user's data, a system database or a table. (Keep in mind that a fragmented table can always be re-created from its fragmented parts by a combination of unions and joins.)

There are three types of data fragmentation:

- **Horizontal fragmentation**: The distributed database design process that breaks a table into subsets of unique rows
- **Vertical fragmentation**: The distributed database design process that breaks a table into subsets of unique columns
- **Mixed fragmentation**: The distributed database design process that breaks a table into subsets of rows and columns

How to choose the criteria for horizontal fragmentation?

There are various methods for horizontal fragmentation:

- Round-robin partitioning. The rows of a table are distributed among the sites in a round-robin fashion. For example, if there are three sites, the first row is stored at site 1, the second row at site 2, the third row at site 3, the fourth row at site 1, and so on.

One disadvantage of round-robin partitioning is that it does not take into account the value of the partitioning attribute. For example, if the partitioning attribute is the user's info, querying all users from new york would require a distributed query to all sites.

- Range partitioning based on a partition key, A partition key is one or more attributes in a table that determine the fragment in which a row will be stored. For example, if you want to provide location awareness, a good partition key would be the customer state field. This is the most common and useful data partitioning strategy.

## Data Replication

**Data replication**: The process of storing a copy of a database fragment at more than one site.

Replicated data is subject to the mutual consistency rule, which requires that all copies of data fragments be identical. Therefore, to maintain data consistency among the replicas, the DDBMS must ensure that a database update is performed at all sites where replicas exist.

There are basically two styles of replication:

- **Push replication**: After a data update, the originating DP node sends the changes to the replica nodes to ensure that data is immediately updated. (Focus on consistency over availability)
- **Pull replication**: After a data update, the originating DP node sends “messages” to the replica nodes to notify them of the update. The replica nodes decide when to apply the updates to their local fragment. (Focus on availability over consistency)

Although replication has some benefits, such as improved data availability, better load distribution, improved data failure tolerance, and reduced query costs, it also imposes additional DDBMS processing overhead because each data copy must be maintained by the system.

Three replications scenarios exists:

- Fully replicated database (impractical due to overhead)
- partially replicated database
- unreplicated replicated database

Several factors influence the decision to use data replication:

- Database size
- Usage frequency
- Cost

## Data Allocation

**Data allocation**: The process of determining where database fragments should be stored in a distributed database.

There are three strategies for data allocation:

- **Centralized allocation**: A data allocation strategy that stores all fragments at a single site.
- **Partitioned allocation**: A data allocation strategy that stores multiple fragment at multiple site.
- **Replicated allocation**: A data allocation strategy that stores multiple copies of a fragment at multiple sites.

# CAP Theorem

**CAP Theorem**: A distributed database system can only guarantee two of the following three properties:

- **Consistency**: Every read receives the most recent write or an error.
- **Availability**: Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
- **Partition tolerance**: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

Since network partitions are inevitable, the CAP theorem implies that every distributed database must choose between consistency and availability.

The choice depends on whether the application's requirement is for consistency or availability, for example:

- **CP**: A distributed database that is CP is a system that is consistent and partition tolerant, but not available. This means that the system will return an error if a partition occurs such that a write cannot be performed on all nodes and the system cannot guarantee that the data is consistent across all nodes.

- **AP**: A distributed database that is AP is a system that is available and partition tolerant, but not consistent. This means that the system will always return a response to a request (the most recent write may not be returned).

CP is suitable for applications that require consistency, such as banking applications, whereas AP is suitable for applications that require availability, such as social media applications.
