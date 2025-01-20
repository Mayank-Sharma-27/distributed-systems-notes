Resources :https://www.hellointerview.com/learn/system-design/deep-dives/redis
https://www.youtube.com/watch?v=h30k7YixrMo

What to use if for in the interview if needed?
1. Redis as a Cache
2. Redis as a Distributed Lock
3. Redis for Leaderboards
4. Redis for Rate Limiting
5. Redis for Proximity Search
6. Redis for Event Sourcing


Redis has remained quite simple and good at what it does best: executing simple operations **fast**.

Some of the most fundamental data structures supported by Redis:

- Strings
- Hashes (Objects)
- Lists
- Sets
- Sorted Sets (Priority Queues)
- Bloom Filters
- Geospatial Indexes
- Time Series

Redis also supports different communication patterns like Pub/Sub and Streams, partially standing in for more complex setups like Apache Kafka or Amazon's Simple Notification Service.

Redis clients cache a set of "hash slots" which map keys to a specific node. This way clients can directly connect to the node which contains the data they are requesting. 

Each node maintains some awareness of other nodes via a gossip protocol so, in limited instances, if you request a key from the wrong node you can be redirected to the correct node.

![[Redis.png]](/images/Redis.png)

## Performance
Redis can handle O(100k) writes per second and read latency is often in the microsecond range

## Capabilities

### Redis as a Cache

![[Redis_As_Cache.png]](/images/Redis_As_Cache.png)

Just a hashmap distributed across nodes. Using Redis in this fashion doesn't solve one of the more important problems caches face: the ["hot key" problem](https://www.hellointerview.com/learn/system-design/deep-dives/redis#hot-key-issues),

### Redis as a Distributed Lock

### Redis for Leaderboards

Redis has [sorted sets](https://redis.io/glossary/redis-sorted-sets/) which maintain ordered data and it can be queried in log time which can help them appropriate for leaderboard applications.

### Redis for Rate Limiting

Fixed window rate limiter as an example

### Redis for Proximity Search

### Redis for Event Sourcing

## Shortcomings and Remediations

### Hot Key Issues

Potential solutions

1. Add an in-memory cache in our clients so that they are not making requests
2. Store same data in multiple keys and randomize the requests so they are spread across the cluster.
3. - We can add read replica instances and dynamically scale these with load.

==In the interview settings solve this proactively==




