# System Design

Single Server setup

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019015226022.png" alt="image-20211019015226022" style="zoom:50%;" />i

DNS 

Domain name system -> url to IP

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019015449495.png" alt="image-20211019015449495" style="zoom:50%;" />



### Databases - Which databases to use?

You can choose between a traditional relational database and a non-relational database. Let us examine their differences.

##### Relational

RDBMS) or SQL database. The most popular ones are MySQL, Oracle database, PostgreSQL, etc. Relational databases represent and store data in tables and rows. You can perform join operations using SQL across different database tables.

##### Non-Relational

Non-Relational databases are also called NoSQL databases. Popular ones are CouchDB, Neo4j, Cassandra, HBase, Amazon DynamoDB, etc. [2]. These databases are grouped into four categories: key-value stores, graph stores column stores, and document stores. Join operations are generally not supported in non-relational databases.

##### Go for Non-relational 

Your application requires super-low latency.
Your data are unstructured, or you do not have any relational data.
You only need to serialize and deserialize data (JSON, XML, YAML, etc.).
You need to store a massive amount of data

##### Load balancer

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019020223836.png" alt="image-20211019020223836" style="zoom: 33%;" />

Easy to horizontally scale
1 goes down then website stays up
Server IP are private from the real world - better security

#### Database replication

Database replication can be used in many database management systems, usually with a master/slave relationship between the original (master) and the copies (slaves)

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019021011709.png" alt="image-20211019021011709" style="zoom:50%;" />

Master write
Slaves Read 

Advantages of database replication:

* Better performance: In the master-slave model, all writes and updates happen in master nodes; whereas, read operations are distributed across slave nodes. This model improves performance because it **allows more queries to be processed in parallel**.
* Reliability: If one of your database servers is destroyed by a natural disaster, such as a typhoon or an earthquake, data is still preserved. You do not need to worry about data loss because data is replicated across multiple locations.
* High availability: By replicating data across different locations, your website remains in operation even if a database is offline as you can access data stored in another database server.



#### Cache

A cache is a temporary storage area that stores the result of expensive responses or frequently accessed data in memory so that subsequent requests are served more quickly. As illustrated in Figure 1-6, every time a new web page loads, one or more database calls are executed to fetch data. The application performance is greatly affected by calling the database repeatedly. The cache can mitigate this problem.



Cache considerations for using a cache system:

* Decide when to use cache. Consider using cache when data is read frequently but modified infrequently. Since cached data is stored in volatile memory, a cache server is not ideal for persisting data. For instance, if a cache server restarts, all the data in memory is lost. Thus, important data should be saved in persistent data stores.
* Expiration policy. It is a good practice to implement an expiration policy. Once cached data is expired, it is removed from the cache. When there is no expiration policy, cached data will be stored in the memory permanently. It is advisable not to make the expiration date too short as this will cause the system to reload data from the database too frequently. Meanwhile, it is advisable not to make the expiration date too long as the data can become stale.
* Consistency: This involves keeping the data store and the cache in sync. Inconsistency can happen because data-modifying operations on the data store and cache are not in a single transaction. When scaling across multiple regions, maintaining consistency between the data store and cache is challenging. For further details, refer to[…]
* Mitigating failures: A single cache server represents a potential single point of failure (SPOF), defined in Wikipedia as follows: “A single point of failure (SPOF) is a part of a system that, if it fails, will stop the entire system from working” [8]. As a result, multiple cache servers across different data centers are recommended to avoid SPOF. Another recommended approach is to overprovision the required memory by certain percentages. This provides a buffer as the memory usage increases.
* Eviction Policy: Once the cache is full, any requests to add items to the cache might cause existing items to be removed. This is called cache eviction. Least-recently-used (LRU) is the most popular cache eviction policy. Other eviction policies, such as the Least Frequently Used (LFU) or First in First Out (FIFO), can be adopted to satisfy different use cases.



#### Content delivery network (CDN) 

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019023317295.png" alt="image-20211019023317295" style="zoom:50%;" />



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019023330151.png" alt="image-20211019023330151" style="zoom:50%;" />



#### Considerations of using a CDN

* Cost: CDNs are run by third-party providers, and you are charged for data transfers in and out of the CDN. Caching infrequently used assets provides no significant benefits so you should consider moving them out of the CDN.
* Setting an appropriate cache expiry: For time-sensitive content, setting a cache expiry time is important. The cache expiry time should neither be too long nor too short. If it is too long, the content might no longer be fresh. If it is too short, it can cause repeat reloading of content from origin servers to the CDN.
* CDN fallback: You should consider how your website/application copes with CDN failure. If there is a temporary CDN outage, clients should be able to detect the problem and request resources from the origin.
* Invalidating files: You can remove a file from the CDN before it expires by performing one of the following operations:
* Invalidate the CDN object using APIs provided by CDN vendors.
* Use object versioning to serve a different version of the object. To version an object, you can add a parameter to the URL, such as a version number. For example, version number 2 is added to the query string: image.png?v=2



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019023559780.png" alt="image-20211019023559780" style="zoom:50%;" />

1. Static assets (JS, CSS, images, etc.,) are no longer served by web servers. They are fetched from the CDN for better performance.

2. The database load is lightened by caching data.



#### Stateful architecture

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019024027316.png" alt="image-20211019024027316" style="zoom:50%;" />

In Figure 1-12, user A’s session data and profile image are stored in Server 1. To authenticate User A, HTTP requests must be routed to Server 1. If a request is sent to other servers like Server 2, authentication would fail because Server 2 does not contain User A’s session data. Similarly, all HTTP requests from User B must be routed to Server 2; all requests from User C must be sent to Server 3.
The issue is that every request from the same client must be routed to the same server. This can be done with sticky sessions in most load balancers [10]; however, this adds the overhead. Adding or removing servers is much more difficult with this approach. It is also challenging to handle server failures.





#### Stateless architecture

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019024214216.png" alt="image-20211019024214216" style="zoom:50%;" />

we move the session data out of the web tier and store them in the persistent data store. The shared data store could be a relational database, Memcached/Redis, NoSQL, etc. The NoSQL data store is chosen as it is easy to scale. Autoscaling means adding or removing web servers automatically based on the traffic load. After the state data is removed out of web servers, auto-scaling of the web tier is easily achieved by adding or removing servers based on traffic load.”



#### Data centers

To improve availability and provide a better user experience across wider geographical areas, supporting multiple data centers is crucial.



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019025031019.png" alt="image-20211019025031019" style="zoom:50%;" />



Technical challenges must be resolved to achieve multi-data center setup

Data syntonisation
Traffic redirection
Test and deployment

#### Message queue

Long running tasks
Between micro services



#### Logging, metrics, automation

Logging: Monitoring error logs is important because it helps to identify errors and problems in the system. You can monitor error logs at per server level or use tools to aggregate them to a centralized service for easy search and viewing.
Metrics: Collecting different types of metrics help us to gain business insights and understand the health status of the system. Some of the following metrics are useful:

* Host level metrics: CPU, Memory, disk I/O, etc.
* Aggregated level metrics: for example, the performance of the entire database tier, cache tier, etc.
* Key business metrics: daily active users, retention, revenue, etc.



Automation: When a system gets big and complex, we need to build or leverage automation tools to improve productivity. Continuous integration is a good practice, in which each code check-in is verified through automation, allowing teams to detect problems early. Besides, automating your build, test, deploy process, etc. could improve developer productivity significantly.
Adding message queues and different tools
Figure 1-19 shows the updated design. Due to the space constraint, only one data center is shown in the figure.

1. The design includes a message queue, which helps to make the system more loosely coupled and failure resilient.
2. Logging, monitoring, metrics, and automation tools are included.



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019045427644.png" alt="image-20211019045427644" style="zoom:50%;" />

#### Database scaling

There are two broad approaches for database scaling: vertical scaling and horizontal scaling.

##### Vertical scaling

Vertical scaling, also known as scaling up, is the scaling by adding more power (CPU, RAM, DISK, etc.) to an existing machine. There are some powerful database servers. According to Amazon Relational Database Service (RDS) [12], you can get a database server with 24 TB of RAM. This kind of powerful database server could store and handle lots of data. For example, stackoverflow.com in 2013 had over 10 million monthly unique visitors, but it only had 1 master database [13]. However, vertical scaling comes with some serious drawbacks:

* You can add more CPU, RAM, etc. to your database server, but there are hardware limits. If you have a large user base, a single server is not enough.
* Greater risk of single point of failures.
* The overall cost of vertical scaling is high. Powerful servers are much more expensive.

##### Horizontal scaling

Horizontal scaling, also known as sharding, is the practice of adding more servers.

Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.

User data is allocated to a database server based on user IDs. Anytime you access data, a hash function is used to find the corresponding shard. In our example, user_id % 4 is used as the hash function. If the result equals to 0, shard 0 is used to store and fetch data. If the result equals to 1, shard 1 is used. The same logic applies to other shards.

<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019045821344.png" alt="image-20211019045821344" style="zoom:50%;" />



Sharding is a great technique to scale the database but it is far from a perfect solution. It introduces complexities and new challenges to the system:

* Resharding data: Resharding data is needed when 1) a single shard could no longer hold more data due to rapid growth. 2) Certain shards might experience shard exhaustion faster than others due to uneven data distribution. When shard exhaustion happens, it requires updating the sharding function and moving data around. Consistent hashing, which will be discussed in Chapter 5, is a commonly used technique to solve this problem.
* Celebrity problem: This is also called a hotspot key problem. Excessive access to a specific shard could cause server overload. Imagine data for Katy Perry, Justin Bieber, and Lady Gaga all end up on the same shard. For social applications, that shard will be overwhelmed with read operations. To solve this problem, we may need to allocate a shard for each celebrity. Each shard might even require further partition.
* Join and de-normalization: Once a database has been sharded across multiple servers, it is hard to perform join operations across database shards. A common workaround is to de-normalize the database so that queries can be performed in a single table.
  In Figure 1-23, we shard databases to support rapidly increasing data traffic. At the same time, some of the non-relational functionalities are moved to a NoSQL data store to reduce the database load. Here is an article that covers many use cases of NoSQL [14].



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019050204772.png" alt="image-20211019050204772" style="zoom:50%;" />



#### Summary

* Keep web tier stateless
* Build redundancy at every tier
* Cache data as much as you can
* Support multiple data centers
* Host static assets in CDN
* Scale your data tier by sharding
* Split tiers into individual services
* Monitor your system and use automation tools



### A 4-step process for effective system design interview

Step 1 - Understand the problem and establish design scope
Step 2 - Propose high-level design and get buy-in
Step 3 - Design deep dive
Step 4 - Wrap up





<img src="/Users/congle/Library/Application Support/typora-user-images/image-20211019182222349.png" alt="image-20211019182222349" style="zoom:50%;" />

