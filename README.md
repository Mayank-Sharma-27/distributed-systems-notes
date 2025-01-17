## Communication Protocols

Internally, for a typical microservice application which consistitues 90%+ of system design problems, either HTTP(S) or gRPC will do the job. Don't make things complicated.

Externally, you'll need to consider how your clients will communicate with your system: who initiates the communication, what are the latency considerations, and how much data needs to be sent.

Across choices, most systems can be built with a combination of HTTP(S), SSE or long polling, and Websockets. Browsers and apps are built to handle these protocols and they're easy to use and generally speaking most system design interviews don't deal with clients that need to have custom, high-performance protocols.

![[communication_protocols.png]](communication_protocols.png)

- Use HTTP(S) for APIs with simple request and responses. Because each request is stateless, you can scale your API horizontally by placing it behind a load balancer. Make sure that your services aren't assuming dependencies on the state of the client (e.g. sessions) and you're good to go.
- Websockets are necessary if you need realtime, bidirectional communication between the client and the server. From a system design perspective, websockets can be challenging because you need to maintain the connection between client and server. This can be a challenge for load balancers and firewalls, and it can be a challenge for your server to maintain many open connections. A common pattern in these instances is to use a message broker to handle the communication between the client and the server and for the backend services to communicate with this message broker. This ensures you don't need to maintain long connections to every service in your backend.
- Lastly, Server Sent Events (SSE) are a great way to send updates from the server to the client. They're similar to long polling, but they're more efficient for unidirectional communication from the server to the client. SSE allows the server to push updates to the client whenever new data is available, without the client having to make repeated requests as in long polling. This is achieved through a single, long-lived HTTP connection, making it more suitable for scenarios where the server frequently updates data that needs to be sent to the client. Unlike Websockets, SSE is designed specifically for server-to-client communication and does not support client-to-server messaging. This makes SSE simpler to implement and integrate into existing HTTP infrastructure, such as load balancers and firewalls, without the need for special handling.

## Scaling

![[scaling.png]](scaling.png)

Horizontal scaling is all about adding more machines to a system to increase its capacity. This is in contrast to vertical scaling, which is the process of adding more resources to a single machine to increase its capacity.

### Work Distribution
Work Distribution is done via load balancer, which helps choose which node from a group to use for incoming request.

### Data Distribution
Look for ways to partition your data such that a single node can access the data it needs without needing to talk to another node.

## Consistency

A **strongly consistent** system will ensure that, after the data is written, all subsequent reads will reflect that write. A **weakly consistent** or more commonly **eventually consistent** system will not make this guarantee, instead allowing for a period of time where the data is stale.

A social media feed can be eventually consistent -- if you post a tweet, it's okay if it takes a few seconds for your followers to see it. However, a banking system needs to be strongly consistent -- if you transfer money from one account to another, it's not okay if the money disappears from one account and doesn't appear in the other.

## Locking

Locking is the process of ensuring that only one client can access a shared resource at a time.

There's three things to worry about when employing locks:

**Granularity of the lock**

We want locks to be as fine-grained as possible. This means that we want to lock as little as possible to ensure that we're not blocking other clients from accessing the system. For example, if we're updating a user's profile, we want to lock only that user's profile and not the entire user table.

**Duration of the lock**

We want locks to be held for as short a time as possible. This means that we want to lock only for the duration of the critical section. For example, if we're updating a user's profile, we want to lock only for the duration of the update and not for the entire request.

**Whether we can bypass the lock**

In many cases, we can avoid locking by employing an "optimistic" concurrency control strategy, especially if the work to be done is either read-only or can be retried. In an optimistic strategy we're going to assume that we can do the work without locking and then check to see if we were right. In most systems, we can use a "compare and swap" operation to do this.

Optimistic concurrency control makes the assumption that most of the time we won't have contention (or multiple people trying to lock at the same time) in a system, which is a good assumption for many systems! That said, not all systems can use optimistic concurrency control. For example, if you're updating a user's bank account balance, you can't just assume that you can do the update without locking.

![An optimistic update](https://d248djf5mc6iku.cloudfront.net/excalidraw/1241f4c24908d0d2250105cd35eb527f)

# distributed-systems-notes
