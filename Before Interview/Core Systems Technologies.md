
Notes are from : https://www.hellointerview.com/learn/system-design/in-a-hurry/key-technologies

I made my own notes but they were too long, so I asked chatGpt to make them more concise.

## Core Database

For DataBase I will pick DynamoDB as I have worked with it the most.

Here is a good advise that is written

%% Many candidates trip themselves up by trying to insert a comparison of relational and NoSQL databases into their answer. The reality is that these two technologies are _highly overlapping_ and broad statements like "I need to use a relational database because I have relationships in my data" (NoSQL databases can work great for this) or "I've gotta use NoSQL because I need scale and performance" (relational databases, used correctly, perform and scale incredibly well) are often yellow flags that reveal inexperience.

  

Here's the truth: _most interviewers don't need an explicit comparison of SQL and NoSQL databases_ in your session and it's a pothole you should completely avoid. Instead, talk about what you know about the database you're using and how it will help you solve the problem at hand. If you're asked to compare, focus on the differences in the databases you're familiar with and how they would impact your design. So "I'm using Postgres here because its ACID properties will allow me to maintain data integrity" is a great leader. %%

- **Relational Databases (RDBMS)**:  
    Key features:
    
    1. SQL Joins
    2. Indexes
    3. Transactions (ACID properties).
    
    Example: Postgres for data integrity via ACID.
    
- **NoSQL Databases**:  
    Flexible for evolving schemas, unstructured data, horizontal scaling.  
    Use cases: Real-time web apps, handling large data volumes.  
    Key features:
    
    1. Schema-less structure (e.g., JSON columns).
    2. Horizontal scalability.
    
**TIP**: Avoid generalizations like "NoSQL is for scalability" or "RDBMS is for relationships." Discuss database features specific to the design problem.

![[No_SQL.png]](/images/No_SQL.png)


TIP :

==The places where NoSQL databases excel are not necessarily places where relational databases fail (and vice-versa). For example, while NoSQL databases are great for handling unstructured data, relational databases can also have JSON columns with flexible schemas. While NoSQL databases are great for scaling horizontally, relational databases can also scale horizontally with the right architecture. When you're discussing NoSQL databases in your system design interview, make sure you're not making broad statements but instead discussing the specific features of the database you're using and how they will help you solve the problem at hand.==

## Blob Storage


- Use for large, unstructured data (e.g., videos, images).
- Examples:
    - **YouTube**: Store videos in blob storage; metadata in a database.
    - **Instagram**: Store media in blob storage; metadata in a database.

**Flow**:

- **Upload**: Client → Pre-signed URL → Blob Storage → Notify Server.
- **Download**: Pre-signed URL via CDN → Client.

![[Blob.png]](/images/Blob.png)

## Search Optimized Database

- Designed for efficient full-text search (e.g., ElasticSearch).
- **Core Techniques**:
    1. Inverted Index: Map words to documents.
    2. Tokenization: Break text into words.
    3. Stemming: Normalize words to root form.
    4. Fuzzy Search: Match terms with slight variations.
    5. Scaling: Use sharding and clustering.

## API Gateway


- Routes requests to appropriate backend services.
- Handles cross-cutting concerns like:
    1. Authentication.
    2. Rate limiting.
    3. Logging.
![[API_GATEWAY.png]](/images/API_GATEWAY.png)

## Load Balancer

- Distributes traffic across servers (horizontal scaling).
- **L4**: For persistent connections (e.g., WebSockets).
- **L7**: For flexible routing and minimizing connection load.

![[Load_Balancer.png]](/images/Load_Balancer.png)

if you have persistent connections like websockets, you'll likely want to use an L4 load balancer. Otherwise, an L7 load balancer offers great flexibility in routing traffic to different services while minimizing the connection load downstream

## Queue

- Smooth traffic spikes and decouple producers/consumers.
- **Use Cases**:
    1. Buffer bursty traffic (e.g., spikes).
    2. Distribute tasks across workers.
![[Queue.png]]

## Streams / Event Sourcing

- Retain data for configurable time; supports re-reading.
- **Use Cases**:
    1. Real-time analytics (e.g., social media engagement).
    2. Event sourcing (e.g., banking transaction history).
    3. Publish-subscribe patterns (e.g., chat apps).

**Key Concepts**:

1. **Partitioning**: Horizontal scaling via partition keys.
2. **Consumer Groups**: Process same data differently.
3. **Replication**: Fault tolerance.
4. **Windowing**: Batch event processing (e.g., hourly aggregates).

## Distributed Lock

- Prevent concurrent access to shared resources (e.g., tickets).
- **Techniques**:
    1. **Redlock** (Redis): Ensures safe, consistent locking.
    2. Expiry: Avoid stale locks after crashes.
    3. Deadlock prevention: Acquire locks consistently.

## Distributed Cache

- Store frequently accessed data in memory.
- **Strategies**:
    1. **Eviction Policies**: LRU, LFU, FIFO.
    2. **Write Strategies**:
        - Write-Through: Cache + datastore.
        - Write-Around: Skip cache.
        - Write-Back: Async write to datastore.
    3. **Data Structures**: Use appropriate structures (e.g., sorted sets for event rankings).

## CDN

- Cache content globally.
- **Use Cases**:
    1. Static assets (e.g., images, scripts).
    2. API responses.

**TIP**: Leverage TTL and cache invalidation for dynamic content.




