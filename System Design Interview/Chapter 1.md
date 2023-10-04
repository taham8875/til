# Chapter 1 : Scale From Zero to Millions of Users

The goal of this chapter is to learn how to scale a system from zero to millions of users. We will start with a simple system and add more components to it to support more users. We will also learn how to scale each component of the system. By the end of this chapter, you will have a good understanding of how to scale a system to support millions of users.

Let's understand this setup:

![simple-web-server](assets/chapter1/simple-web-service.png)


1. User access the website using a browser. through the domain name `www.example.com`, which is resolved to the IP address of the web server by the DNS server. (this is more human readable)
2. Internet protocol (IP) address is returned to the browser.
3. Browser sends a request to the web server via the HTTP protocol.
4. Web server receives the request and sends back the response. (either `text/html` ar `application/json` or `image/png` etc)


## Database

With the growth of the website, we need to store the data somewhere. We can use a database to store the data. We need multiple servers: one for the web/mobile traffic (web tier) and one for the database server (data tier). This allows us to scale both independently.

![Alt text](assets/chapter1/two-servers-for-both-traffic-and-db.png)

## What database to use?

- **Relational database** (SQL): data are stored in tables. (e.g. MySQL, PostgreSQL, Oracle, SQL Server). You can perform join operations on the tables. 
- **Non-relational database** (NoSQL): data are stored in key-value stores, graph stores, column stores, or document stores. (e.g. MongoDB, Cassandra, Redis, DynamoDB). Join operations are not supported generally.


For most developers, relational databases are more familiar. They are around for a long time and have been used in many applications. However, if the relational database are not suitable for your application, you may use non-relational databases if :
- You app required very low latency
- Your data are unstructured, or you don't have relational data
- Tou need to serialize and deserialize data frequently (e.g. JSON)
- You need to store large volume of data

## Vertical Scaling vs Horizontal Scaling

- **Vertical scaling**: add more resources (CPU, RAM, Disk) to the existing server. (scale up)

    - Cons : 
        - Limited by the hardware (you can't add infinite resources)
        - If one server goes down, the whole system goes down

- **Horizontal scaling** (scale out): add more servers to the existing system. (scale out) (is more desirable)



## Load Balancer

In our design, users are connected to the web server directly. However, if the traffic is high, the web server may not be able to handle all the requests and the server fails or the response time becomes very long. We can add a load balancer in front of the web server to distribute the traffic to multiple web servers.

![load-balancer](assets/chapter1/load-balancer.png)


As shown, user is connected directly to the load balancer IP address so the web servers are unreachable directly. (for better security, the web servers IP addresses are not exposed to the internet directly)

If server 1 fails or is under heavy load, the load balancer will distribute the traffic to server 2 

If the two servers traffics grows radially, the load balancer will handle this problems gracefully. You anly need to add more servers to the web server pool and the load balancer will distribute the traffic to the new servers.

Now the web tier looks good, what about the data tier? 

## Database Replication

We can add more servers to the data tier as well. However, we need to make sure that the data are consistent across all the servers. We can use database replication to achieve this.

![database-replication](assets/chapter1/database-replication.png)

As shown, we have two database servers. The master server is the primary server and the slave servers is the secondary servers. The master server is the only server that handles write operation. The slave server get copies of the data from the master server. The slave server can handle read operations. Most applications have more read operations than write operations.


Advantages of database replication:

- **Better Performance**: read operations can be handled by the slave servers and can process parallel queries due to existence of many servers. The master server can focus on write operations.
- **Reliability**: if the master server fails or destroyed, the slave server can take over and become the master server. And no loss of data.
- **High availability**: if the master server is under heavy load or fails, the slave servers can handle the read operations and reduce the load on the master server.


**What if one of the database servers fails?**
- If the master server fails, the slave server can get promoted and become the master server. 
- If there are only one slave server, read operations will be directed to the master server temporarily. (until the slave server is up again)


## Wrapping up

the system design after adding the load balancer and database replication:

![Alt text](assets/chapter1/system-design-after-load-balancer-and-db-replication.png)

Let us take a look at the design:
- A user gets the IP address of the load balancer from DNS.
- A user connects the load balancer with this IP address.
- The HTTP request is routed to either Server 1 or Server 2.
- A web server reads user data from a slave database.
- A web server routes any data-modifying operations to the master database. This includes write, update, and delete operations.

Now, let's improve the load/response time of the website by adding a cache layer and shifting the static content to a content delivery network (CDN).

## Cache

A cache is a temporary storage area that stores the result of expensive responses or frequently accessed data in memory so that subsequent requests are served more quickly. In the previous figure, every time a user requests a web page, the web server has to query the database to get the data. This is expensive and time-consuming.

### Cache Tier

![Alt text](assets/chapter1/cache-tier.png)

After receiving the request, the web server will check if the data is in the cache. If it is, the web server will return the data directly. If not, the web server will query the database and store the data in the cache. The next time the same request is received, the web server will return the data from the cache. This caching strategy is called **read-through cache**.


### Considerations for using cache

- Decide when to use cache. Use it when data is read frequently and modified infrequently. Since cache is stored in volatile memory, it should not be used for persistent data.
- Expiration policy. Once data is expired in the cache, it should be removed. Don't make the expiration time too long or too short. If it is too long, the data may be stale. If it is too short, the system will need to reload the data from the database frequently.
- Consistency: keep the database and cache in sync. When the data is modified in the database, the cache should be updated as well. When scaling across multiple servers and regions, this can be challenging.
- Eviction policy: when the cache is full, which data should be removed? Least recently used (LRU), least frequently used (LFU) or first in first out (FIFO) are three common eviction policies.

## Content Delivery Network (CDN)

A CDN is a geographically distributed group of servers that work together to provide fast delivery of Internet content. A CDN allows for the quick transfer of assets needed for loading Internet content including HTML pages, javascript files, stylesheets, images, and videos.

![cdn](assets/chapter1/cdn.png)

This is how CDN works:
- A user requests a web page from the website.
- The closest CDN server will be selected based on the user's location to deliver the static content if it exists in the CDN server. If not the CDN server will get the content from the web server and cache it.
- The further the user is from the CDN server, the longer it will take to deliver the content. (due to network latency)


### Considerations for using CDN

- **Cost**: CDNs are run by third-party providers, and you are charged for data transfers in and out of the CDN. Don't cache inconsistent data in the CDN.
- **expiration time**: set the expiration time for the content in the CDN. If the expiration time is too long, the content may be stale. If it is too short, the CDN will need to fetch the content from the web server frequently.
- **CDN fallback**: if the CDN is down, the web server should be able to serve the content directly. (this is called CDN fallback)


The following figure shows the system design after adding the cache layer and CDN:

![Alt text](assets/chapter1/system-design-after-cache-and-cdn.png)

## Stateless web tier

Now it is time to scale the web tier horizontally. For this, we need to move the state (e.g. user session) from the web tier to a persistance storage like the database.

### Stateful architecture

![Alt text](assets/chapter1/stateful-architecture.png)

A stateful server remembers client data (state) from one request to the next. A stateless server keeps no state information.

Note how in the previous figure the user A's session data are stored in server 1, if the user A's request is routed to server 2, the user A' will not be authenticated.


### Stateless architecture

![Alt text](assets/chapter1/stateless-architecture.png)


In this stateless architecture, HTTP requests from users can be sent to any web servers, which fetch state data from a shared data store. State data is stored in a shared data store and kept out of web servers. A stateless system is simpler, more robust, and scalable.

The following figure shows the updated system design after with the stateless web tier:

![Alt text](assets/chapter1/system-design-after-stateless-web-tier.png)

we move the session data out of the web tier and store them in the persistent data store.

## Data centers 

The following figure shows an example setup with two data centers. In normal operation, users are geoDNS-routed, also known as geo-routed, to the closest data center, with a split traffic of $x\%$ and $(100 - x)\%$ between the two data centers. geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user. If one data center fails, all traffic is routed to the other data center.

![Alt text](assets/chapter1/data-centers.png)


### Considerations for using multiple data centers

- **Traffic redirection**: : Effective tools are needed to direct traffic to the correct data center. GeoDNS can be used to direct traffic to the nearest data center depending on where a user is located.
- **Date synchronization**: Data in the two data centers should be synchronized. This can be achieved by using database replication.
- **Test and deployment**: You need to test and deploy your application in your data centers in different location. This can be challenging if you have a large number of servers.

## Message Queue

A message queue is a form of asynchronous service-to-service communication used in serverless and microservices architectures. Messages are stored on the queue until they are processed and deleted. Each message is processed only once, by a single consumer. Message queues can be used to decouple heavyweight processing, to buffer or batch work, and to smooth spiky workloads.

![Alt text](assets/chapter1/message-queue.png)

Consider the following use case: your application supports photo customization, including cropping, sharpening, blurring, etc. Those customization tasks take time to complete. In the following figure, web servers publish photo processing jobs to the message queue. Photo processing workers pick up jobs from the message queue and asynchronously perform photo customization tasks. The producer and the consumer can be scaled independently. When the size of the queue becomes large, more workers are added to reduce the processing time. However, if the queue is empty most of the time, the number of workers can be reduced.

![Alt text](assets/chapter1/message-queue-example.png)

## Logging, metrics and automation

In small website that runs on a few servers, logging metrics and automation are best practices but not a necessity. However, as the website grows, they become more important.


- **Logging**: logging is the process of recording application actions to a secondary storage device. Logs are used for debugging, auditing, and analytics purposes. Logs can be stored in a file or a database. In a distributed system, they are collected from multiple servers and stored in a centralized location.
- **Metrics**: metrics are used to measure the performance of the system. They are used to identify bottlenecks and to improve the performance of the system. for example:
    - Host metrics: CPU, memory, disk, network
    - Aggregated metrics: requests per second, average response time, error rate, the performance of the database
    - Key business metrics: number of active users, number of new users, number of paying users, etc.
- **Automation**: automation is the process of automating manual tasks. For example, you can automate the deployment process so that you can deploy your application to multiple servers with a single command. You can also automate the process of scaling up and down your servers based on the traffic. This improves the productivity of the team and reduces the chance of human errors.

The following figure shows the system design after adding the message queue logging, metrics and automation:

![Alt text](assets/chapter1/system-design-after-logging-metrics-and-automation.png)

## Database scaling

### Vertical scaling

Vertical scaling is the process of adding more resources (CPU, RAM, Disk) to the existing server. This is the simplest way to scale a database. However, it has the following limitations:

- **Limited by the hardware**: you can't add infinite resources to the server. At some point, you will reach the limit of the hardware.
- **Single point of failure**: if the server fails, the whole system goes down.
- **Expensive**: the cost of vertical scaling is high. You need to buy expensive hardware.

### Horizontal scaling

Horizontal scaling (also known as sharding) is the process of adding more servers to the existing system. 

The following figure compares vertical scaling with horizontal scaling:

![Alt text](assets/chapter1/vertical-scaling-vs-horizontal-scaling.png)

Sharding separates large databases into smaller, faster, more easily managed parts called data shards. 

The following figure shows an example of sharded databases. User data is allocated to a database server based on user IDs. Anytime you access data, a hash function is used to find the corresponding shard. In our example, user_id % 4 is used as the hash function. If the result equals to 0, shard 0 is used to store and fetch data. If the result equals to 1, shard 1 is used. The same logic applies to other shards.

![Alt text](assets/chapter1/sharding.png)

The most important factor to consider when implementing a sharding strategy is the choice of the sharding key. The sharding key should be chosen such that it distributes the data evenly across all the shards. If the sharding key is not chosen properly, some shards will have more data than others, which will lead to unbalanced load across the shards. For example, if you use user_id as the sharding key, the shards will be unbalanced if the user_id is not evenly distributed.

Sharding is a great technique to scale the database but it is far from a perfect solution. It introduces complexities and new challenges to the system:

- **Resharding**: Resharding is needed when a single shard cannot hold more records, oe certain shards are overloaded.
- **Celebrity problem** (Hotspot key problem): Some records are more popular than others. For example, some users have more followers than others. This leads to unbalanced load across the shards. 
- **Join and denormalization**: Join operations is hard across database shards sharded databases. You need to denormalize the data to avoid join operations.

In the following figure, we shard databases to support rapidly increasing data traffic:

![Alt text](assets/chapter1/system-design-after-sharding.png)

## Millions of users and beyond


Scaling a system is an iterative process. Iterating on what we have learned in this chapter could get us far. More fine-tuning and new strategies are needed to scale beyond millions of users. For example, you might need to optimize your system and decouple the system to even smaller services. All the techniques learned in this chapter should provide a good foundation to tackle new challenges. To conclude this chapter, we provide a summary of how we scale our system to support millions of users:

- Keep web tier stateless
- Build redundancy at every tier
- Cache data as much as you can
- Support multiple data centers
- Host static assets in CDN
- Scale your data tier by sharding
- Split tiers into individual services
- Monitor your system and use automation tools
