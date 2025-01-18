
Notes are from : https://www.hellointerview.com/learn/system-design/in-a-hurry/key-technologies

Keep in mind from the Levelling section that the amount of depth your interviewer is likely to probe is proportional to the level you're interviewing at. A mid-level candidate who can roughly describe [ElasticSearch](https://www.hellointerview.com/learn/system-design/deep-dives/elasticsearch) as a search index is likely to be fine, but a senior candidate who can't describe the inverted index or reason about its scaling is likely to be a yellow flag. In either case, focus on breadth before depth!

## Core Database

For DataBase I will pick DynamoDB as I have worked with it the most.

Here is a good advise that is written

%% Many candidates trip themselves up by trying to insert a comparison of relational and NoSQL databases into their answer. The reality is that these two technologies are _highly overlapping_ and broad statements like "I need to use a relational database because I have relationships in my data" (NoSQL databases can work great for this) or "I've gotta use NoSQL because I need scale and performance" (relational databases, used correctly, perform and scale incredibly well) are often yellow flags that reveal inexperience.

  

Here's the truth: _most interviewers don't need an explicit comparison of SQL and NoSQL databases_ in your session and it's a pothole you should completely avoid. Instead, talk about what you know about the database you're using and how it will help you solve the problem at hand. If you're asked to compare, focus on the differences in the databases you're familiar with and how they would impact your design. So "I'm using Postgres here because its ACID properties will allow me to maintain data integrity" is a great leader. %%

### Relational Databases

Relational databases (sometimes called RDBMS or Relational Database Management Systems) are the most common type of database

##### Things you should know about relational databases

1. **SQL Joins**
2. **Indexes**
3. **RDBMS Transactions**

If you want to dive deep check this out
https://cstack.github.io/db_tutorial/parts/part1.html

### NoSQL Databases

NoSQL databases are a broad category of databases designed to accommodate a wide range of data models, including key-value, document, column-family, and graph formats. Unlike relational databases, NoSQL databases do not use a traditional table-based structure and are often schema-less. This flexibility allows NoSQL databases to handle large volumes of unstructured, semi-structured, or structured data, and to scale horizontally with ease.

![[No_SQL.png]](/images/No_SQL.png)

NoSQL databases are strong candidates for situations where:

**Flexible Data Models:** Your data model is evolving or you need to store different types of data structures without a fixed schema. **Scalability:** Your application needs to scale horizontally (across many servers) to accommodate large amounts of data or high user loads.

**Handling Big Data and Real-Time Web Apps:** You have applications dealing with large volumes of data, especially unstructured data, or applications requiring real-time data processing and analytics.

TIP :

==The places where NoSQL databases excel are not necessarily places where relational databases fail (and vice-versa). For example, while NoSQL databases are great for handling unstructured data, relational databases can also have JSON columns with flexible schemas. While NoSQL databases are great for scaling horizontally, relational databases can also scale horizontally with the right architecture. When you're discussing NoSQL databases in your system design interview, make sure you're not making broad statements but instead discussing the specific features of the database you're using and how they will help you solve the problem at hand.==

## Blob Storage

##### What is blob storage and when should you use it?

When we need to store large, unstructured data like images videos or other files. Blob storage services are simple. You can upload a blob of data and that data is stored and get back a URL

Here are some common examples of when to use blob storage:

- Design Youtube -> Store videos in blob storage, store metadata in a database.
    
- Design Instagram -> Store images & videos in blob storage, store metadata in a database.
    
- Design Dropbox)-> Store files in blob storage, store metadata in a database.

![[Blob.png]](/images/Blob.png)

To upload:

- When clients want to upload a file, they request a presigned URL from the server.
- The server returns a presigned URL to the client, recording it in the database.
- The client uploads the file to the presigned URL.
- The blob storage triggers a notification to the server that the upload is complete and the status is updated.

To download:

- The client requests a specific file from the server and are returned a presigned URL.
- The client uses the presigned URL to download the file via the CDN, which proxies the request to the underlying blob storage.

## Search Optimized Database

##### What is a search optimized database and when should you use it?

SELECT * FROM documents WHERE document_text LIKE '%search_term%'

The above query will not be efficient because it requires a full table scan.

Search optimized databases, on the other hand, are specifically designed to handle full-text search

They use techniques like indexing, tokenization, and stemming to make search queries fast and efficient. In short, they work by building what are called [inverted indexes](https://www.hellointerview.com/learn/system-design/deep-dives/elasticsearch#lucene-segment-features). Inverted indexes are a data structure that maps from words to the documents that contain them. This allows you to quickly find documents that contain a given word. A simple example of an inverted index might look like this:

```
{
  "word1": [doc1, doc2, doc3],
  "word2": [doc2, doc3, doc4],
  "word3": [doc1, doc3, doc4]
}
```

##### Things to know about search optimized databases

1. **Inverted Indexes**: As just mentioned, search optimized databases use inverted indexes to make search queries fast and efficient. An inverted index is a data structure that maps from words to the documents that contain them. This allows you to quickly find documents that contain a given word.
    
2. **Tokenization**: Tokenization is the process of breaking a piece of text into individual words. This allows you to map from words to documents in the inverted index.
    
3. **Stemming**: Stemming is the process of reducing words to their root form. This allows you to match different forms of the same word. For example, "running" and "runs" would both be reduced to "run".
    
4. **Fuzzy Search**: Fuzzy search is the ability to find results that are similar to a given search term. Most search optimized databases support fuzzy search out of the box as a configuration option. In short, this works by using algorithms that can tolerate slight misspellings or variations in the search term. This is achieved through techniques like edit distance calculation, which measures how many letters need to be changed, added, or removed to transform one word into another.
    
5. **Scaling**: Just like traditional databases, search optimized databases scale by adding more nodes to a cluster and sharding data across those nodes.

## API Gateway

##### What is an API gateway and when should you use it?

An api gateway sits in front of your system and is responsible for routing the requests to the backend services.

For example, if the system receives a request to GET /users/123, the API gateway would route that request to the users service and return the response to the client. The gateway is also typically also responsible for handling cross-cutting concerns like authentication, rate limiting, and logging
![[API_GATEWAY.png]](/images/API_GATEWAY.png)

## Load Balancer

When you have a large amount of traffic, you will need to distribute that traffic across multiple machines (called horizontal scaling) to avoid overloading any single machine or creating a hotspot

![[Load_Balancer.png]](/images/Load_Balancer.png)

if you have persistent connections like websockets, you'll likely want to use an L4 load balancer. Otherwise, an L7 load balancer offers great flexibility in routing traffic to different services while minimizing the connection load downstream

## Queue

##### What are queues and when should you use them?

Queues serve as buffers for bursty traffic or as a means of distributing work across a system. A compute resource sends messages to a queue and forgets about them. On the other end, a pool of workers (also compute resources) processes the messages at their own pace. Messages can be anything from a simple string to a complex object.

The queue's function is to smooth out the load on the system. If I get a spike of 1,000 requests but can only handle 200 requests per second, 800 requests will wait in the queue before being processed — but they are not dropped! Queues also decouple the producer and consumer of a system, allowing you to scale them independently. I can bring down and up services behind a queue with negligible impact.

Common Use cases:

1. Buffer for Bursty Traffic
2. Distribute Work Across a System
![[Queue.png]]

## Streams / Event Sourcing

Unlike message queues, streams can retain data for a configurable period of time, allowing consumers to read and re-read messages from the same position or from a specified time in the past. Streams are a good choice...

1. **When you need to process large amounts of data in real-time.** Imagine designing a system for a social media platform where you need to display real-time analytics of user engagements (likes, comments, shares) on posts. You can use a stream to ingest high volumes of engagement events generated by users across the globe. A stream processing system (like Apache Flink or Spark Streaming) can process these events in real-time to update the analytics dashboard.
    
2. **When you need to support complex processing scenarios like event sourcing.** Consider a banking system where every transaction (deposits, withdrawals, transfers) needs to be recorded and could affect multiple accounts. Using event sourcing with a stream like Kafka, each transaction is an event that can be stored, processed, and replayed. This setup not only allows for real-time processing of transactions but also enables the bank to audit transactions, rollback changes, or reconstruct the state of any account at any point in time by replaying the events.
    
3. **When you need to support multiple consumers reading from the same stream.** In a real-time chat application, when a user sends a message, it's published to a stream associated with the chat room. This stream acts as a centralized channel where all chat participants are subscribers. As the message is distributed through the stream, each participant (consumer) receives the message simultaneously, allowing for real-time communication. **This is a great example of a publish-subscribe pattern, which is a common use case for streams.**

##### Things you should know about streams for your interview

1. **Scaling with Partitioning**: In order to scale streams, they can be partitioned across multiple servers. Each partition can be processed by a different consumer, allowing for horizontal scaling. Just like databases, you will need to specify a partition key to ensure that related events are stored in the same partition.
    
2. **Multiple Consumer Groups**: Streams can support multiple consumer groups, allowing different consumers to read from the same stream independently. This is useful for scenarios where you need to process the same data in different ways. For example, in a real-time analytics system, one consumer group might process events to update a dashboard, while another group processes the same events to store them in a database for historical analysis.
    
3. **Replication**: In order to support fault tolerance, just like databases, streams can replicate data across multiple servers. This ensures that if a server fails, the data can still be read from another server.
    
4. **Windowing**: Streams can support windowing, which is a way of grouping events together based on time or count. This is useful for scenarios where you need to process events in batches, such as calculating hourly or daily aggregates of data. Think about a real-time dashboard that shows mean delivery time per region over the last 24 hours.

## Distributed Lock

##### What are distributed locks and when should you use them?

When you're dealing with online systems like Ticketmaster, you might need a way to lock a resource - like a concert ticket - for a short time (~10 minutes in this case). This is so while one user is in the middle of buying a ticket, no one else can grab it. ==Traditional databases with ACID properties use transaction locks to keep data consistent, which is great for ensuring that while one user is updating a record, no one else can update it, but they're not designed for longer-term locking. This is where distributed locks come in handy==

Some common examples of when to use a distributed lock:

1. **E-Commerce Checkout System**
2. **Ride-Sharing Matchmaking**
3. **Distributed Cron Jobs**
4. **Online Auction Bidding System**

##### Things you should know about distributed locks for your interview

1. **Locking Mechanisms**: There are different ways to implement distributed locks. One common implementation uses Redis and is called Redlock. Redlock uses multiple Redis instances to ensure that a lock is acquired and released in a safe and consistent manner.
    
2. **Lock Expiry**: Distributed locks can be set to expire after a certain amount of time. This is important for ensuring that locks don't get stuck in a locked state if a process crashes or is killed.
    
3. **Locking Granularity**: Distributed locks can be used to lock a single resource or a group of resources. For example, you might want to lock a single ticket in a ticketing system or you might want to lock a group of tickets in a section of a stadium.
    
4. **Deadlocks**: Deadlocks can occur when two or more processes are waiting for each other to release a lock. Think about a situation where two processes are trying to acquire two locks at the same time. One process acquires lock A and then tries to acquire lock B, while the other process acquires lock B and then tries to acquire lock A. This can lead to a situation where both processes are waiting for each other to release a lock, causing a deadlock. You should be prepared to discuss how to prevent this - a common mistake is to have locks pulled from far-flung pieces of infrastructure or your code, this makes it hard to recognize and prevent deadlocks.

## Distributed Cache

A cache is just a server, or cluster of servers, that stores data in memory. They're great for storing data that's expensive to compute or retrieve from a database.

##### Things you should know about distributed caches for your interview

1. **Eviction Policy**: Distributed caches have different eviction policies that determine which items are removed from the cache when the cache is full. Some common eviction policies are:
    
    - Least Recently Used (LRU): Evicts the least recently accessed items first.
        
    - First In, First Out (FIFO): Evicts items in the order they were added.
        
    - Least Frequently Used (LFU): Removes items that are least frequently accessed.

2. **Cache Invalidation Strategy**: This is the strategy you'll use to ensure that the data in your cache is up to date. For example, if you are designing Ticketmaster and caching popular events, then you'll need to invalidate an event in the cache if the event in your Database was updated (like the venue changed).
3. **Cache Write Strategy**: This is the strategy you use to make sure that data is written to your cache in a consistent way. Some strategies are:
    
    - Write-Through Cache: Writes data to both the cache and the underlying datastore simultaneously. Ensures consistency but can be slower for write operations.
        
    - Write-Around Cache: Writes data directly to the datastore, bypassing the cache. This can minimize cache pollution but might increase data fetch times on subsequent reads.
        
    - Write-Back Cache: Writes data to the cache and then asynchronously writes the data to the datastore. This can be faster for write operations but can lead to data loss if the cache is not persisted to disk.

==Don't forget to be explicit about what data you are storing in the cache, including the data structure you're using. Remember, modern caches have many different datastructures you can leverage, they are not just simple key-value stores. So for example, if you are storing a list of events in your cache, you might want to use a sorted set so that you can easily retrieve the most popular events. Many candidates will just say, "I'll store the events in a cache" and leave it at that. This is a missed opportunity and may invite follow-up questions.==

## CDN

To cache content globally

##### Things you should know about CDNs

1. **CDNs are not just for static assets**. While CDNs are often used to cache static assets like images, videos, and javascript files, they can also be used to cache dynamic content. This is especially useful for content that is accessed frequently, but changes infrequently. For example, a blog post that is updated once a day can be cached by a CDN.
    
2. **CDNs can be used to cache API responses**. If you have an API that is accessed frequently, you can use a CDN to cache the responses. This can help reduce the load on your servers and improve the performance of your API.
    
3. **Eviction policies**. Like other caches, CDNs have eviction policies that determine when cached content is removed. For example, you can set a time-to-live (TTL) for cached content, or you can use a cache invalidation mechanism to remove content from the cache when it changes.




