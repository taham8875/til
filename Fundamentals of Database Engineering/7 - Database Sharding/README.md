# Database Sharding

Sharding is a technique to split data into multiple database instances (shards). This is done to improve performance and manageability of the database.

# Consistent Hashing

Consistent hashing is a special kind of hashing such that when a hash table is resized, only `K/n` keys need to be remapped on average, where `K` is the number of keys, and `n` is the number of slots.

For example, imagine we have a hash function that maps objects to integers between `0` and `10` partitions depends on the modulo of the hash value. If we add a new partition, we need to remap `1/10` of all keys to the new partition. If we add two new partitions, we need to remap `2/10` of all keys.

# Advantages of Sharding

- Scalability: Sharding allows us to add more machines to support more load.
- Security Isolation: Sharding allows us to isolate data for security reasons.
- Optimal and smaller index size: Sharding allows us to have smaller indexes, which may fit in memory.

# Disadvantages of Sharding

- Complex client (it must be aware of all shards)
- Transaction across shards is complex
- Rollbacks are complex
- Schema changes are complex
- Joins from multiple shards is complex
- Has to be something you know in the query to shard on (if you don't have the sharding key, you have to search all shards)

# When you should shard your database?

The last thing you want to do is shard your database. It's a lot of work and it's a lot of complexity. You should only shard your database when you have to. You should only shard your database when you have a problem that you can't solve any other way.

So before sharding, there are many optimizations you can do:

- Horizontal partitioning
- Replication (master-slave)
- Replication (master-master)
